# Task: Authentication with OAuth2, Sessions, and JWT

Add a security layer to the backend with OAuth2 (Google), encrypted session cookies, and JWT token generation.

## Security Configuration

Create a new file `backend/src/main/kotlin/org/jetbrains/security/Auth.kt` that:

1. **Defines data classes:**
   - `AuthConfig` — holds `encryptionKey: String` and `signKey: String` read from application config
   - `UserSession` — a `@Serializable` data class with `token: String`

2. **Implements `configureSecurity(config: ApplicationConfig)`** that sets up:
   - **OAuth2 authentication** with Google provider using Ktor's `oauth` authentication:
     - Provider name: `"google"`
     - Authorization URL: `https://accounts.google.com/o/oauth2/auth`
     - Token URL: `https://accounts.google.com/o/oauth2/token`
     - Client ID and secret from config
     - Scopes: `openid`, `profile`, `email`
   - **JWT authentication** using Ktor's `jwt` authentication:
     - Configure with a JWK provider
     - Validate the audience claim
   - **Session cookies** using Ktor's Sessions plugin:
     - Cookie-based sessions with `UserSession`
     - Encryption using `SessionTransportTransformerEncrypt` with the keys from config

## Application Routes

Create `backend/src/main/kotlin/org/jetbrains/app/AppRoutes.kt` with:

- Static file serving from `/web` for the frontend assets
- Route `/` that redirects: if the user has a session, redirect to `/home`; otherwise redirect to `/login`
- Route `/home` that requires authentication (session-based)
- Login and OAuth callback routes

## Wiring

In `Application.kt`, call `configureSecurity(property("config.auth"))` before the routing block. Add the auth configuration section to `application.yaml` with `encryptionKey`, `signKey`, `clientId`, and `clientSecret` fields.

Use Ktor's built-in authentication plugins (`io.ktor.server.auth`). Look at the existing application.yaml for the configuration pattern.