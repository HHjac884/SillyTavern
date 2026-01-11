# Active Context - SillyTavern

## Current Work Focus

As of **January 10, 2026**, the SillyTavern project is at version **1.15.0**. This is the initial Memory Bank initialization, capturing the current state of the codebase for future work.

## Recent Changes

### Current Repository State
- **Git Remote**: `https://github.com/HHjac884/SillyTavern.git` (fork)
- **Latest Commit**: `58df65f9a40d7c52a1978aae947b5cefd00bffd6`
- **Note**: This appears to be a fork of the main SillyTavern repository

### Version 1.15.0 Features
Based on the codebase analysis, version 1.15.0 includes:

- Multi-backend LLM support (OpenAI, Anthropic, Google, Azure, Claude, OpenRouter, Horde, local models)
- Character card management with PNG metadata support
- World Info system for context injection
- Quick Replies (macro) system
- Vector storage for long-term memory
- TTS/STT integration
- Image generation (Stable Diffusion, Horde)
- Translation support (multiple providers)
- User account system with authentication
- Plugin and UI extension system
- Responsive design with mobile support

## Next Steps

### Immediate Priorities (None Specified)
Since this is a fresh Memory Bank initialization, there are no active tasks in progress. Future work should focus on:

1. **Code Review**: Evaluate the fork's changes from the upstream repository
2. **Feature Development**: Address any specific requirements or issues
3. **Bug Fixes**: Resolve any reported issues
4. **Documentation**: Keep Memory Bank updated with any changes

## Active Decisions and Considerations

### Technology Stack
- **Backend**: Node.js 18+ with ESM modules
- **Server**: Express.js with WebSocket support
- **Frontend**: Vanilla JavaScript with jQuery (legacy)
- **Build Tool**: Webpack
- **Image Processing**: @jimp (WASM-based for performance)
- **Tokenization**: tiktoken, @agnai/web-tokenizers
- **Vector Storage**: vectra with OpenAI embeddings

### Important Patterns

#### Backend Module Pattern
All backend integrations live in `src/endpoints/backends/` and follow a consistent interface:
- Normalized request/response handling
- Streaming support via Server-Sent Events (SSE)
- Error handling with user-friendly messages

#### Character Card Format
Characters are stored as JSON with optional PNG metadata embedding. The parser (`src/character-card-parser.js`) handles multiple formats.

#### Configuration
- YAML-based (`config.yaml` in server directory)
- Command-line arguments can override config
- User-specific settings stored in browser localStorage

### Architectural Notes

1. **Separation of Concerns**:
   - `src/` contains server-side logic
   - `public/` contains client-side assets
   - `src/endpoints/` contains API route handlers
   - `src/endpoints/backends/` contains LLM provider integrations

2. **Data Storage**:
   - Characters and chats stored as JSON files
   - User data isolated by account
   - Optional vector embeddings for memory

3. **Communication**:
   - REST API for most operations
   - WebSocket for real-time updates
   - SSE for streaming responses

## Learnings and Project Insights

### Codebase Characteristics
- **Mature Project**: Well-structured with clear separation of concerns
- **Feature-Rich**: Extensive functionality beyond basic chat
- **Extensible**: Plugin system allows third-party additions
- **Privacy-Focused**: Local-first storage with optional cloud APIs

### Known Considerations
- **Legacy Code**: Some parts use older patterns (jQuery, vanilla JS)
- **Multi-Backend Complexity**: Abstraction layer adds complexity but enables flexibility
- **Security**: CSRF protection enabled by default, configurable via command line
- **Rate Limiting**: Built-in rate limiting for API calls

### Development Workflow
- **Testing**: Jest for unit tests, Playwright for E2E tests
- **Linting**: ESLint with custom rules
- **Docker Support**: Multi-stage builds for containerization
- **Documentation**: External docs at https://docs.sillytavern.app/

## Integration Points

### External Services
- OpenAI API
- Anthropic API
- Google Generative AI
- Azure OpenAI
- NovelAI
- OpenRouter
- AI Horde (community model pool)
- Stable Diffusion (image generation)
- Translation APIs (Google, Bing, etc.)
- TTS/STT services

### Local Services
- KoboldCPP (local LLM inference)
- Ollama (local model server)
- LM Studio (inference)

## Future Considerations

1. **Potential Improvements**:
   - Modernize frontend framework (Vue/React migration)
   - Improve TypeScript coverage
   - Enhance plugin system capabilities
   - Better mobile experience

2. **Areas Needing Attention**:
   - Legacy code modernization
   - Performance optimization for large character databases
   - Enhanced error recovery and retry logic
   - Better accessibility support

3. **Community Feedback**:
   - Monitor GitHub issues for user pain points
   - Engage with Discord community for feature requests
   - Track fork changes vs upstream
