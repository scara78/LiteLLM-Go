# Tech Stack

- **Language:** Go 1.25 — all source code is Go; no frontend framework
- **Web Framework:** [GoFr](https://gofr.dev) (`gofr.dev v1.54.1`) — use GoFr for HTTP routing, middleware, dependency injection, and app bootstrap; do NOT use `gorilla/mux` directly (it is a transitive dep)
- **Database:** PostgreSQL via `github.com/lib/pq` — use GoFr's built-in DB container (`ctx.SQL`) for queries; raw `database/sql` only when GoFr container is unavailable
- **Cache:** Redis via `github.com/redis/go-redis/v9` — use GoFr's Redis container (`ctx.Redis`) for caching and rate limiting
- **Messaging:** Kafka via `github.com/segmentio/kafka-go` (transitive through GoFr) — use GoFr pub/sub abstractions, not kafka-go directly
- **Configuration:** YAML files (`gopkg.in/yaml.v3`) + `.env` via `github.com/joho/godotenv`; read config through GoFr's config system where possible
- **IDs:** `github.com/google/uuid` for UUID generation
- **Testing:** `github.com/stretchr/testify` for assertions; `github.com/DATA-DOG/go-sqlmock` for DB mocking; `go.uber.org/mock` for interface mocks
- **Observability:** OpenTelemetry (traces + metrics) and Prometheus are wired in via GoFr — do not add separate OTel setup manually
- **Module path:** `aryanmehrotra/litellm-go`

# Rules

- Always use GoFr's context (`*gofr.Context`) for DB, Redis, and logger access inside handlers.
- Route registration goes in `main.go`; handler logic goes in dedicated handler files.
- Do not import `gorilla/mux`, `database/sql`, or `go-redis` directly in application code — use GoFr abstractions.
- Keep handlers thin: delegate business logic to service/use-case layer.
- Use `testify/assert` (not `testify/require`) for non-fatal assertions in table-driven tests.
- Config values must come from environment variables or YAML config, never hard-coded.
- All new packages must be added to `go.mod` via `go get`; do not hand-edit `go.sum`.
