# Task: WebSocket & SSE with Serialization

Add real-time communication endpoints using WebSocket and Server-Sent Events (SSE), backed by a streaming AI travel assistant service.

## Message Protocol

In `AppRoutes.kt`, define a sealed interface for the message protocol using `kotlinx.serialization`:

- `PartialAnswer(token: String)` — a streaming token from the AI
- `AnswerEnd` — marks the end of a streaming response
- `Error(text: String)` — an error message

All message types should be `@Serializable` with a `type` discriminator.

## AI Service

Create `backend/src/main/kotlin/org/jetbrains/ai/TravelService.kt`:

- Define a `TravelService` interface with a method `answer(memoryId: Int, message: String): Flow<String>` that returns a streaming flow of text tokens.
- Include a system message that configures the AI as a travel assistant.

Create `backend/src/main/kotlin/org/jetbrains/ai/LangChain4J.kt`:

- A `LangChainFactory` that builds service implementations using LangChain4J's `AiServices.builder()`.

## Chat Memory Persistence

Create `backend/src/main/kotlin/org/jetbrains/ai/ChatMemories.kt`:

- An `ExposedChatMemoryStore` that implements LangChain4J's `ChatMemoryStore` interface
- Uses an Exposed `LongIdTable("chat_memories")` with `memoryId` and `messages` (JSON text) columns
- Serializes/deserializes chat messages to JSON for database persistence

## AI Module

Create `backend/src/main/kotlin/org/jetbrains/plugins/AiModule.kt`:

- Configure OpenAI streaming chat model (compatible with Ollama)
- Set up in-memory embedding store and content retriever for RAG
- Configure chat memory provider backed by `ExposedChatMemoryStore`
- Register `TravelService` and related components via Ktor's DI

## WebSocket and SSE Endpoints

In `AppRoutes.kt`, implement:

1. **WebSocket `/ws`** — Accept text frames as user messages, call `TravelService.answer()`, and send back `PartialAnswer` tokens as they stream in, followed by `AnswerEnd`.

2. **SSE `/sse`** — Accept a query parameter for the user message, call `TravelService.answer()`, and emit `PartialAnswer` events as server-sent events, followed by `AnswerEnd`.

Both endpoints should handle errors gracefully by sending an `Error` message.

## Database Migration

Create `backend/src/main/resources/db/migration/V3__create_chat_memories_table.sql` for the chat memory persistence table.

Use Ktor's WebSocket and SSE plugins. Use `kotlinx.serialization` for message encoding. Look at the existing dependency configuration in `gradle/libs.versions.toml` for LangChain4J and related libraries.