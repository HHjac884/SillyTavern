# Tech Context - SillyTavern

## Technologies Used

### Core Backend

| Technology | Version | Purpose |
|------------|---------|---------|
| Node.js | 18+ | Runtime environment |
| Express.js | 4.21.0 | Web server framework |
| WebSocket (ws) | 8.18.3 | Real-time communication |
| body-parser | 1.20.2 | Request body parsing |
| cookie-parser | 1.4.6 | Cookie handling |
| cookie-session | 2.1.1 | Session management |

### Frontend

| Technology | Purpose |
|------------|---------|
| Vanilla JavaScript | Main application logic |
| jQuery | DOM manipulation (legacy) |
| jQuery UI | UI components |
| Popper.js | Tooltip positioning |
| Morphdom | Efficient DOM updates |
| FontAwesome | Icons |
| Highlight.js | Syntax highlighting |
| Webpack | Build tool |

### AI/ML Integration

| Technology | Version | Purpose |
|------------|---------|---------|
| tiktoken | 1.0.22 | OpenAI tokenization |
| @agnai/web-tokenizers | 0.1.3 | SentencePiece tokenization |
| @agnai/sentencepiece-js | 1.1.1 | BPE/SentencePiece |
| sillytavern-transformers | 2.14.6 | On-device inference |
| vectra | 0.2.2 | Vector storage |

### Image Processing

| Technology | Version | Purpose |
|------------|---------|---------|
| @jimp/* | 1.6.0 | Image manipulation (WASM-based) |
| png-chunk-text | 1.0.0 | PNG metadata reading |
| png-chunks-extract | 1.0.0 | PNG chunk extraction |

### Data Handling

| Technology | Version | Purpose |
|------------|---------|---------|
| yaml | 2.8.1 | YAML parsing |
| node-persist | 4.0.4 | Simple key-value storage |
| localforage | 1.10.0 | Client-side storage |
| node-fetch | 3.3.2 | HTTP requests |
| proxy-agent | 6.5.0 | Proxy support |

### Security & Middleware

| Technology | Version | Purpose |
|------------|---------|---------|
| helmet | 8.1.0 | Security headers |
| cors | 2.8.5 | CORS handling |
| csrf-sync | 4.2.1 | CSRF protection |
| compression | 1.8.1 | GZIP compression |
| ipaddr.js | 2.2.0 | IP address handling |
| ip-regex | 5.0.0 | IP validation |

### Utilities

| Technology | Version | Purpose |
|------------|---------|---------|
| lodash | 4.17.21 | Utility functions |
| moment | 2.30.1 | Date/time manipulation |
| mime-types | 3.0.2 | MIME type detection |
| bytes | 3.1.2 | Byte formatting |
| seedrandom | 3.0.5 | Seeded RNG |
| simple-git | 3.28.0 | Git operations |
| yargs | 17.7.1 | CLI argument parsing |

### Other Features

| Technology | Version | Purpose |
|------------|---------|---------|
| @zeldafan0225/ai_horde | 5.2.0 | AI Horde API |
| google-translate-api-x | 10.7.2 | Translation |
| bing-translate-api | 4.1.0 | Translation |
| archiver | 7.0.1 | ZIP creation |
| yauzl | 3.2.0 | ZIP extraction |
| fuse.js | 7.1.0 | Fuzzy search |
| showdown | 2.1.0 | Markdown parsing |
| dompurify | 3.2.6 | HTML sanitization |
| html-entities | 2.6.0 | HTML entity encoding |
| rate-limiter-flexible | 5.0.5 | Rate limiting |

## Development Setup

### Prerequisites
- Node.js 18 or higher
- npm (comes with Node.js)
- Git (optional, for development)

### Installation Steps

```bash
# Clone repository
git clone https://github.com/HHjac884/SillyTavern.git
cd SillyTavern

# Install dependencies
npm install

# Run development server
npm start

# Or run in debug mode
npm run debug
```

### Environment Variables
Set via command line or `config.yaml`:

| Variable | Purpose |
|----------|---------|
| `DATA_ROOT` | Data storage directory |
| `NODE_ENV` | Environment (development/production) |

## Configuration

### Main Config File
Location: `{server directory}/config.yaml`

Key settings:
- Server host/port
- Default API keys
- Authentication settings
- CORS configuration
- Storage paths

### Command Line Arguments
Parsed via `src/command-line.js`:

| Flag | Purpose |
|------|---------|
| `--help` | Show help |
| `--port <number>` | Override port |
| `--dataRoot <path>` | Set data directory |
| `--disableCsrf` | Disable CSRF protection |
| `--global` | Global installation mode |

## Project Structure

```
sillytavern/
├── src/                    # Server-side source code
│   ├── endpoints/         # API route handlers
│   │   ├── backends/      # LLM provider integrations
│   │   ├── users/         # User management
│   │   └── *.js           # Various API endpoints
│   ├── middleware/        # Express middleware
│   ├── tokenizers/        # Tokenization utilities
│   ├── vectors/           # Vector storage
│   ├── types/             # Type definitions
│   ├── validator/         # Validation schemas
│   ├── electron/          # Electron app wrapper
│   └── *.js               # Core server files
├── public/                # Client-side assets
│   ├── css/               # Stylesheets
│   ├── js/                # JavaScript modules (built)
│   ├── img/               # Images
│   ├── lib/               # Third-party libraries
│   ├── locales/           # Internationalization
│   ├── scripts/           # Utility scripts
│   └── *.html             # Main HTML files
├── default/               # Default content
│   └── content/           # Default characters/settings
├── memory-bank/           # Memory Bank documentation
├── docs/                  # Additional documentation
├── tests/                 # Test files
│   ├── util.test.js       # Unit tests
│   ├── mock-server.test.js
│   └── sample.e2e.js      # E2E tests
├── docker/                # Docker files
├── backups/               # Chat backups
├── plugins/               # Installed plugins
├── server.js              # Entry point
├── package.json           # Project metadata
├── webpack.config.js      # Webpack config
└── .clinerules/           # Cline instructions
```

## Build System

### Webpack Configuration
File: `webpack.config.js`

Features:
- Multiple entry points
- Output: `public/script.js`
- CSS handling
- Dev server support

### Build Commands

```bash
# No explicit build command in package.json
# Webpack runs automatically via dev flow

# Lint code
npm run lint

# Fix linting issues
npm run lint:fix
```

## Testing

### Test Framework
- **Jest**: Unit testing
- **Playwright**: E2E testing

### Test Configuration
- `tests/jest.config.json` - Jest config
- `tests/playwright.config.js` - Playwright config
- `tests/.eslintrc.cjs` - Test linting rules

### Running Tests

```bash
# Install test dependencies first
cd tests
npm install

# Run unit tests
npm test

# Run E2E tests (requires Playwright)
npx playwright test
```

## Deployment

### Local Development
```bash
npm start
```

### Docker
```bash
docker-compose up
```

Or using docker entrypoint:
```bash
docker build -t sillytavern .
docker run -p 8000:8000 sillytavern
```

### Electron
```bash
npm run start:electron
```

### Bun (Alternative Runtime)
```bash
npm run start:bun
```

### Deno (Alternative Runtime)
```bash
npm run start:deno
```

## Technical Constraints

### Node.js Version
- Minimum: Node.js 18
- ESM modules required (`"type": "module"`)

### Browser Support
- Modern browsers (ES6+)
- No IE support
- WebSocket required
- LocalStorage required

### File System
- Write access required for data storage
- Character/chat directories must be accessible

### Network
- Outbound HTTPS for API calls
- Optional proxy support
- CORS handling for cross-origin requests

## Dependencies Management

### Post-Install Hook
File: `post-install.js`

Runs after `npm install` to:
- Set up any required directories
- Perform initialization tasks
- Validate environment

### Plugin Management
```bash
# Update plugins
npm run plugins:update

# Install plugins
npm run plugins:install
```

## Tool Usage Patterns

### Git
- Version control for project
- `simple-git` package for programmatic operations
- Supports checking updates

### ESLint
- Linting with custom rules
- `no-var` rule disabled (legacy code)
- `no-path-concat` disabled for path operations

### Webpack
- Bundles frontend JavaScript
- Handles CSS imports
- Supports multiple entry points

## Development Notes

### Code Style
- ESM modules throughout
- Async/await preferred
- JSDoc comments for documentation
- ESLint enforced style

### Error Handling
- Try-catch around async operations
- Meaningful error messages
- Graceful degradation where possible

### Performance Considerations
- Streaming responses for LLM
- Web Workers for heavy computation
- Debouncing user inputs
- Lazy loading of resources

## External API Dependencies

### Required for Full Functionality
- OpenAI API key (for OpenAI backends)
- Anthropic API key (for Claude)
- Google AI key (for Gemini)
- Azure OpenAI credentials (for Azure)
- Other provider-specific keys as needed

### Optional Integrations
- Translation API keys (Google, Bing)
- Image generation API (Stable Diffusion)
- TTS/STT services
- Vector embedding service (OpenAI or compatible)
