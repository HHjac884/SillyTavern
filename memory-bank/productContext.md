# Product Context - SillyTavern

## Why This Project Exists

SillyTavern was created to solve the fragmentation problem in the LLM interaction space. Users wanting to use different AI providers (OpenAI, Anthropic, local models, etc.) had to switch between different interfaces, losing character data and conversation context in the process. SillyTavern provides a **unified, extensible frontend** that works with any LLM backend while maintaining character definitions, chat history, and user preferences.

## Problems It Solves

### 1. Multi-Provider Fragmentation
**Problem**: Users can't easily switch between OpenAI, Claude, local models, or other providers without losing their character setups and conversation history.

**Solution**: Abstracted backend API (`src/endpoints/backends/`) that normalizes different provider interfaces into a unified response format.

### 2. Character Data Portability
**Problem**: Character definitions exist in various formats (TavernAI, Character Hub, etc.) and can't be shared between platforms.

**Solution**: Comprehensive character card parsing (`src/character-card-parser.js`) supporting multiple formats and PNG metadata embedding.

### 3. Context Management
**Problem**: LLMs lose relevant information over long conversations, especially with complex scenarios or multiple characters.

**Solution**: World Info system for injecting relevant context, and vector storage for long-term memory retrieval.

### 4. Advanced User Features Missing
**Problem**: Basic LLM interfaces lack power-user features like branching conversations, quick replies, and deep customization.

**Solution**: Feature-rich UI with macros, presets, quick replies, and extensive configuration options.

## How It Works

### Architecture Flow
```
User → Web UI → Express Server → API Endpoints → LLM Providers
         ↓            ↓              ↓
      localStorage  file system   Vector Store
```

### User Experience

1. **Setup**: User installs locally or via Docker, runs server, configures API keys in `config.yaml`
2. **Character Creation**: Import or create characters with personality definitions, example dialogue, and metadata
3. **Chat**: Select character and backend, begin conversation with streaming responses
4. **Advanced Features**: Use World Info for context, Quick Replies for macros, Presets for prompt engineering
5. **Persistence**: Chats and characters saved to local filesystem; can export/import character cards

### Key User Workflows

#### Character Role-Playing
- Import character card
- Select AI provider (OpenAI, Claude, etc.)
- Chat with character maintaining personality
- Use Quick Replies for common actions
- Save/export conversation

#### Multi-Character Group Chat
- Create group with multiple characters
- Set world info context for the scenario
- Characters interact with each other and user
- Tag characters for directed messages

#### AI Development/Testing
- Use presets for different prompt templates
- Test with different backends
- Analyze token usage and logprobs
- Export data for analysis

## User Experience Goals

1. **Familiarity**: UI should feel intuitive for users of chat applications
2. **Responsiveness**: Streaming responses with minimal latency
3. **Reliability**: Graceful handling of API failures, retries, and error recovery
4. **Privacy**: Data stored locally by default; user controls what gets sent to external APIs
5. **Customizability**: Every aspect should be tweakable via settings or code modification
6. **Extensibility**: Plugin system and UI extensions for adding new functionality

## Success Metrics (Implicit)

- Successful connection to diverse LLM providers
- Low error rates for API interactions
- Fast response times for streaming
- Easy character import/export flow
- Positive user engagement with advanced features (World Info, Quick Replies, etc.)

## Pain Points to Address

1. **API Key Management**: Different providers have different key formats and rate limits
2. **Context Limits**: Managing token budgets across different models
3. **Format Compatibility**: Supporting various character card formats as they evolve
4. **Real-time Updates**: Keeping UI in sync with streaming responses
5. **Mobile Experience**: Ensuring usability on smaller screens
