# API Key Security in the Backend

API key exposure is a big security concern, therefore this page of the documentaiton aims to provide the developers with the necessary information on how to properly secure and implement api keys.

## Risks causing API key exposures
[Source](https://escape.tech/blog/how-to-secure-api-secret-keys/)

**Embedding API keys in code**\
Never embed API keys directly in the application code. Even though our repositories are private, there is always a risk of accidental exposure (e.g., pushing to a public repo, developer misconfiguration, or compromised GitHub accounts). The safest practice is to store API keys outside of source code, typically using environment variables or a secrets manager.

**Storing API keys in the application's source tree**\
API keys should never be stored within the application’s source files. While our repositories are private, keeping keys in code still makes them vulnerable if the repo access is ever compromised. Additionally, committing keys to Git makes it difficult to rotate or revoke them safely. For development, use a .env file (excluded from Git with .gitignore); for production, rely on environment variables or a dedicated secret management solution.

**API keys in frontend code**\
API keys must never be included in frontend (Svelte) code. Anything shipped to the client (JavaScript, HTML, CSS) is fully visible to end users, even if obfuscated. If the frontend needs access to a third-party service, it should always go through our backend, which securely manages and stores API keys.

**Sending API keys in plain text**\
All communication involving API keys must happen over HTTPS. Since our backend and future integrations will be deployed with Transport Layer Security, the likelihood of sending keys in plain text is low. However, it’s still important to keep this requirement explicit: API keys should never be logged, sent in query strings, or transmitted without encryption.

**Using the same API keys for multiple APIs or services**\
Each external service should use its own dedicated API key. Keys must be scoped with the principle of least privilege (only the permissions required for their purpose). This minimizes the damage of a single key being exposed.

## Configuration and management of envrionment variables in Go

[Source](https://nattrio.medium.com/streamlining-go-configuration-and-environment-variables-management-2f5ebacf66e3)

Configuration should be kept separate from code so that the same application can run in multiple environments (development, staging, production) without code changes. This includes database credentials, API keys, ports, and log levels. Keeping configuration in environment variables helps prevent sensitive values from ending up in version control.

**In our current approach:**\
We use a `.env` file locally during development, paired with a `.env.example` for reference.\
`.env` is listed in `.gitignore`, so sensitive values are not committed to GitHub.\
Our EnvConfig.go file is responsible for loading environment variables (using Go’s built-in `os.Getenv()`).\
In production (Dockerized backend), environment variables should be injected at container runtime (via `docker-compose.yml` or Docker secrets) instead of relying on `.env` files.

**Best practices for our project**\
Never hardcode secrets in `Go` code or commit them to the repo.\
Keep `.env` files for local development only.\
Use environment variables for production deployments, configured through Docker.\
Provide defaults (e.g., port = 8080) in code where sensible, but never for secrets.\
If future needs require more complex configuration management, libraries like `caarlos0/env` or `viper` can simplify struct-based configs, but our current setup is sufficient.

## Future proofing

**API Key Rotation**\
To reduce risk, API keys should be rotated periodically and scoped to the minimum required permissions. Each environment (dev/staging/prod) must use separate keys. When adding a new service, document its rotation requirements in our internal docs.

**Audit Logging**\
Our backend should log API usage without exposing keys. Logs should include the service, endpoint, method, and response status. API keys must never appear in logs. Over time, we may expand to structured audit logging stored in PostgreSQL or a logging system.

## Technical guidance for implementation

The API keys must always be stored securely and never exposed to the frontend. This means they reside in environment variables that are loaded by the `Go` backend. During development, values can be provided in a `.env` file, ignored by **Git**. In staging and production, environment variables are injected at **container runtime** through the Docker configuration. The application retrieves them at startup using our `EnvConfig.go` file, via Go’s `os.Getenv()`. This ensures that secrets are not hardcoded in the source code or inadvertently included in a frontend build.

```
apiKey := os.Getenv("STRIPE_API_KEY")
if apiKey == "" {
    log.Fatal("missing STRIPE_API_KEY")
}
```

If we were to implement user-specific keys in the future, middleware would be appropriate for enforcement.Middleware in Gorilla Mux sits between the request and the handler, allowing us to perform validation checks before a request reaches the main business logic. For API keys, middleware would extract the key from request headers, validate it, and reject the request if the key is missing or invalid. Centralizing this logic ensures consistent enforcement across all endpoints.

**Example of middleware for validating an internal API key:**

```
func APIKeyMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        apiKey := r.Header.Get("X-API-Key")
        if apiKey == "" {
            http.Error(w, "API key required", http.StatusUnauthorized)
            return
        }

        validKey := os.Getenv("INTERNAL_API_KEY")
        if apiKey != validKey {
            http.Error(w, "Invalid API key", http.StatusForbidden)
            return
        }

        // future reference: validate against keys stored in PostgreSQL (expiry, scopes)

        next.ServeHTTP(w, r)
    })
}
```

### Audit logic

Middleware is also the right place to integrate audit logging. When requests are validated, metadata such as the endpoint called, the outcome of the validation, and, if available, the identity of the requester should be logged. It is **critical** that logs never include the API key itself. Instead, logs should capture enough context to trace the flow of requests without exposing sensitive information.

**Example:**

```
time="2025-10-02T15:04:05Z" level=info msg="API call" endpoint="/api/v1/data" status=200 requester="user123"
```

### Key rotation

Key rotation is an important part of secure API key management. For third-party services, rotation policies vary by provider, but keys should always be replaced periodically and scoped narrowly. Each environment (development, staging, and production) should have its own dedicated key. If user-facing API keys are implemented in the future, they should include explicit expiry dates, which the middleware would enforce.