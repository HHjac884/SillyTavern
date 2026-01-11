# System Patterns - SillyTavern

## System Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         Browser (Client)                        │
│  ┌─────────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │   script.js     │  │   style.css  │  │  index.html      │  │
│  │  (Vanilla JS)   │  │              │  │                  │  │
│  └────────┬────────┘  └──────────────┘  └──────────────────┘  │
│           │                                                         
│           │ WebSocket / SSE / REST                                
│           ▼                                                         
├─────────────────────────────────────────────────────────────────┤
│                      Express Server (Node.js)                    │
│  ┌───────────────┐  ┌──────────────────┐  ┌─────────────────┐  │
│  │ server-main.js│  │   Middleware     │  │  Event Emitters │  │
│  │               │  │  (auth, CORS,    │  │                 │  │
│  │               │  │   rate-limit)    │  │                 │  │
│  └───────┬───────┘  └──────────────────┘  └─────────────────┘  │
│          │                                                          
│          ▼                                                          
│  ┌──────────────────────────────────────────────────────────────┐ │
│  │              API Endpoints (src/endpoints/)                   │ │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────┐  │ │
│  │  │characters│ │  chats   │ │  users   │ │   backends/   │  │ │
│  │  └──────────┘ └──────────┘ └──────────┘ │  LLM Adapter │  │ │
│  │                                         └──────────────┘  │ │
│  └──────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
           │                   │                   │
           ▼                   ▼                   ▼
    ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
    │ File System │     │   Vector DB │     │External APIs │
    │(characters) │     │ (memory)    │     │  (OpenAI,    │
    │  (chats)    │     │             │     │  Claude,     │
    └─────────────┘     └─────────────┘     │  etc.)       │
                                            └─────────────┘
```

## Key Technical Decisions

### 1. Backend Abstraction Layer
**Decision**: Create a unified abstraction for all LLM providers

**Rationale**: Different providers have incompatible APIs. Normalizing them enables:
- Switch backends without changing UI
- Consistent streaming response handling
- Unified error handling

**Implementation**: 
- Directory: `src/endpoints/backends/`
- Each backend implements a common interface pattern
- Returns normalized response objects

### 2. Character Card PNG Embedding
**Decision**: Store character data as both JSON files and embedded PNG metadata

**Rationale**: 
- JSON for easy programmatic access
- PNG embedding for portability (single file sharing)
- Supports community standards (Character Hub, etc.)

**Implementation**:
- `src/character-card-parser.js` handles reading/writing PNG metadata
- Uses `png-chunk-text` and `png-chunks-extract` for manipulation
- Supports multiple card format versions

### 3. WebSocket + SSE Hybrid
**Decision**: Use WebSocket for UI state, SSE for response streaming

**Rationale**:
- WebSocket: Bidirectional, event-driven updates (typing indicators, etc.)
- SSE: Unidirectional, perfect for streaming LLM responses
- Separates concerns for cleaner code

**Implementation**:
- `src/server-events.js` manages WebSocket connections
- Streaming responses use Express's SSE pattern

### 4. File System Storage
**Decision**: Store all data as JSON files rather than database

**Rationale**:
- Simpler deployment (no DB setup required)
- Easy backup/migration (just copy directory)
- Transparent for users to inspect/edit
- Git-friendly for version control

**Trade-offs**:
- Slower queries at scale (not indexed)
- No ACID guarantees for concurrent writes
- Solution: Use file locking and atomic writes where critical

### 5. ESM Modules
**Decision**: Use ES modules (import/export) throughout

**Rationale**:
- Modern JavaScript standard
- Better tree-shaking with Webpack
- Consistent with contemporary Node.js development

**Implementation**:
- `"type": "module"` in package.json
- All imports use `.js` extension

## Design Patterns in Use

### 1. Strategy Pattern (Backends)
Each LLM provider implements a strategy for:
- Request formatting
- Response parsing
- Streaming handling
- Error conversion

**File**: `src/endpoints/backends/*.js`

### 2. Repository Pattern (Data Access)
Separation between business logic and data storage:

- `src/characters.js` - Character repository
- `src/chat.js` (implied) - Chat repository
- File system abstraction via utility functions

### 3. Middleware Pattern (Express)
Express middleware chain handles:
- Authentication (CSRF, sessions)
- Rate limiting
- Request validation
- Error handling

**Location**: `src/middleware/`

### 4. Event-Driven Architecture
Server-side events for:
- Character updates
- Chat notifications
- Connection status
- Real-time sync

**File**: `src/server-events.js`

### 5. Plugin Architecture
Dynamic loading of extensions:

**Files**:
- `src/plugin-loader.js`
- `plugins/` directory
- UI extension system

## Component Relationships

### Core Data Flow: Chat Generation

```
1. User sends message
   ↓
2. POST /api/chats/generate
   ↓
3. Request validated (middleware)
   ↓
4. Backend adapter selected
   ↓
5. Prompt constructed (character + history + world info)
   ↓
6. API call to LLM provider
   ↓
7. Stream response via SSE
   ↓
8. WebSocket update to client
   ↓
9. Client appends message to UI
   ↓
10. Chat saved to file system
```

### Character Loading Flow

```
1. Request: GET /api/characters/:id
   ↓
2. Check file system for character.json
   ↓
3. Parse character data
   ↓
4. Extract PNG metadata if needed
   ↓
5. Load associated assets (avatar, expressions)
   ↓
6. Return unified character object
```

### World Info Context Injection

```
1. User's message received
   ↓
2. Message scanned for keywords
   ↓
3. Matching World Info entries found
   ↓
4. Entries ranked by priority/recency
   ↓
5. Context injected into prompt
   ↓
6. LLM generates response with awareness of world info
```

## Critical Implementation Paths

### 1. Server Startup
**Entry Point**: `server.js`

```
server.js
  ↓
src/command-line.js (parse args)
  ↓
src/server-directory.js (set paths)
  ↓
src/server-main.js
  ├─ src/server-startup.js (init)
  ├─ src/config-init.js (load config)
  ├─ src/plugin-loader.js (load plugins)
  └─ src/server-events.js (setup WebSocket)
```

### 2. API Request Handling
**Pattern**: All endpoints follow Express route handler pattern

```javascript
// Typical endpoint structure
router.post('/endpoint', async (req, res) => {
    // 1. Validate request
    // 2. Perform business logic
    // 3. Interact with storage/API
    // 4. Return response
});
```

### 3. LLM Backend Selection
**File**: Dynamic based on user's preset selection

Each backend implements:
- `generate(request)` - main generation method
- `getModels()` - list available models
- Configuration handling

### 4. Character Card Parsing
**File**: `src/character-card-parser.js`

Critical for:
- Importing from external sources
- Exporting to shareable formats
- Reading embedded PNG data

## Important Technical Patterns

### Error Handling
```javascript
// Standard pattern across codebase
try {
    // operation
} catch (error) {
    console.error('Context:', error);
    return res.status(500).json({ error: error.message });
}
```

### Atomic File Writes
```javascript
import { writeFileAtomic } from 'write-file-atomic';
// Ensures no corruption on interrupted writes
```

### Request Proxy
**File**: `src/request-proxy.js`

Handles:
- Custom headers
- Proxy configuration
- Request logging
- Rate limiting

## Security Patterns

### 1. CSRF Protection
- Enabled by default
- Token-based validation
- Configurable via `--disableCsrf` flag

### 2. User Isolation
- Each user has isolated directory structure
- Session-based authentication
- File system access restricted to user's data

### 3. API Key Storage
- Keys stored in server-side config
- Never exposed to client
- Environment variable support

## Performance Considerations

### Streaming Responses
- Use Server-Sent Events (SSE) for LLM responses
- Chunked responses for large payloads
- WebSocket for real-time updates

### Image Processing
- Use WASM-based Jimp for performance
- Async processing to avoid blocking
- Thumbnail generation for previews

### Tokenization
- Multiple tokenizer implementations
- Cached token counts where possible
- Efficient context window management
