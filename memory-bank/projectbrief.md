# Project Brief - SillyTavern

## Project Name
SillyTavern

## Version
1.15.0

## Core Purpose
A powerful LLM frontend designed for power users to interact with Large Language Models through a web-based interface.

## Primary Goals

1. **Universal LLM Interface**: Provide a unified interface for connecting to multiple LLM providers (OpenAI, Anthropic, Google, Azure, Claude, OpenRouter, Horde, local models via KoboldCPP/Ollama, and more)

2. **Character Management**: Enable creation and management of AI characters with rich metadata, personality definitions, and relationship tracking

3. **Chat System**: Build a sophisticated chat interface supporting group chats, branching conversations, and message editing

4. **Advanced Features**: Implement power-user features including:
   - World Info (context injection)
   - Presets (prompt templates, settings)
   - Quick Replies (macro system)
   - TTS/STT integration
   - Image generation integration
   - Vector storage for long-term memory
   - Translation support
   - Content management and data tools

5. **Extensibility**: Support plugins, UI extensions, and custom scripts for customization

## Key Requirements

- **Multi-Backend Support**: Must connect to diverse LLM APIs with consistent abstraction
- **Real-time Communication**: WebSocket-based updates for streaming responses
- **Privacy-First**: Run locally or connect to external APIs based on user preference
- **User Accounts**: Multi-user support with authentication and isolation
- **Responsive Design**: Mobile-friendly interface
- **Configurable**: YAML-based configuration system

## Scope
- Frontend: Web interface (HTML/CSS/JS) served via Express
- Backend: Node.js (ESM modules) with Express server
- Integrations: Multiple AI/ML service APIs
- Deployment: Can run locally, via Docker, or on cloud platforms

## Out of Scope
- LLM model training or fine-tuning (only inference via external APIs)
- Mobile native apps (web-based only)
- Enterprise SaaS features (self-hosted focus)

## Target Users
- AI/ML enthusiasts and researchers
- Role-playing and creative writing users
- Developers building AI applications
- Privacy-conscious users wanting local control
