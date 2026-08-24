# Aevor — Development State

Last updated: 2026-08-24

## Current state (verified 2026-08-24)

- Platform Tasks 2a–2e and 3a–3b are ALL merged into `platform/main` (HEAD `bc95682`): repository discovery, selection, issue/PR/commit sync, workspace manager (PR #44), go-git cloner + codebase discovery. The full-feature branch `feature/github-repositories` merged as PR #45; the equivalent stacked slices merged as PRs #46 (Task 3a cloner) and #47 (Task 3b discovery) — identical content, no conflicts.
- Local `platform` branches cleaned: only `main` remains, fast-forwarded to `origin/main`. Deleted local-only branches were all verified fully contained in origin history beforehand (`fix/users-upsert-token-column`, `feat`, `backup/wip-feat-668fa85`, the three feature branches above).
- Remote stale branches still exist on origin (`feat`, `backup/wip-feat-668fa85`, `refactor/*`, `task2/*`, `test/*`, merged feature branches) — deletion intentionally deferred pending explicit approval.
- 4 local stashes preserved in `platform` (Aug 15–22 WIP snapshots on long-merged branches); dropping them is destructive and awaits an explicit decision.
- Known open items: GORM default-logger SQL logging in `pkg/database/postgres.go` still leaks bound values (incl. token ciphertext) to logs — tightening deferred as its own task; `docs/prd.md`, `decisions.md`, `roadmap.md`, `contributing.md` remain empty stubs.

## Completed work

- Four separate repositories scaffolded: `docs`, `platform`, `ai`, `infra` (each a Git repo; the Aevor root is NOT a Git repo).
- Docs-first foundation: architecture, database, api-spec, learning (through Day 10), interview-notes. `docs/prd.md`, `docs/decisions.md`, `docs/roadmap.md`, `docs/contributing.md` are empty stubs.
- Local PostgreSQL 16 via Docker Compose (`platform/deployments/docker/docker-compose.yml`): host port `5433` mapped to container `5432`, volume `postgres_data`, container `aevor-postgres`.
- Config loader centralized in `pkg/config` (`AppConfig.Load()` validates DB, GitHub OAuth, JWT, token-encryption-key; fail-fast); `pkg/database` consumes it.
- Users domain: `User` model (UUID PK via `BeforeCreate`, `github_id` unique alternate key, username, display_name, email, avatar_url, `GitHubAccessToken *string` `json:"-"`, timestamps), GORM `AutoMigrate`, Handler -> Service -> Repository layers, `UpsertByGitHubID` (`ON CONFLICT (github_id) DO UPDATE`), AES-256-GCM token encrypt/decrypt (`internal/users/crypto.go`).
- Security gate items resolved in the `platform` repo working tree: `.env` untracked, `.gitignore` (ignores `.env`/`.env.*`), `.env.example` created.
- `docs/GOW.md` (frontend brief for Developer 2) committed to the `docs` repo via PR #2, merged into `main` (merge commit `f6b8430`, 2026-08-11).
- `gh` CLI installed via Homebrew and authenticated as `knockknock10` (gist, read:org, repo, workflow scopes).

### Task 1 (GitHub OAuth end-to-end) — Parts 1–8 COMMITTED and merged

FACT (2026-08-15, verified): all Task 1 Parts 1–8 are committed in the `platform` repo and merged to `main`/`origin/main` via PRs (feature work was merged as #24–#28; `main` = `0fd2bea`, PR #28 "docs: update development state snapshot"). Commit history on the current branch includes `92fbbd9` (#24 users repository interface + harden jwt config), `913cacd` (#25 issue jwt on oauth callback), `f2856c9` (#26 find-or-create upsert tests), `66e7d7a` (#27 todo tracker), `0fd2bea` (#28 dev-state snapshot). The `feat` branch is STALE at `6e18c0f` and should not be treated as current.

- **Part 1 — OAuth login init:** `GET /auth/github/login` generates random `state` + random PKCE `code_verifier`, stores both in a short-lived HttpOnly cookie, redirects to GitHub with `code_challenge=S256`; `GenerateState`/`VerifyState` (crypto/rand + `subtle.ConstantTimeCompare`).
- **Part 2 — Callback:** `GET /auth/github/callback` verifies `state`, exchanges the code with PKCE `code_verifier` via `golang.org/x/oauth2` (`VerifierOption`); exchange is NEVER retried (single-use code); error mapping: `invalid_state`, `github_authorization_denied`, `invalid_code`, `github_unavailable`.
- **Part 3 — JWT + /users/me:** `JWTManager` (HS256 via golang-jwt v5, claims sub/iat/exp(7d)/iss/aud), `RequireAuth` middleware, `GET /users/me`; temporary public routes `POST /users`, `GET /users/:id`, `GET /users/github/:id` removed.
- **Part 4 — GitHub profile client:** `internal/github` client (`GetCurrentUser`), injected HTTP client (10s default timeout), injectable base URL (`WithBaseURL`), User-Agent, Bearer auth, redirects disabled, typed error taxonomy (github_api_unauthorized / github_rate_limited / github_unavailable / github_invalid_response / github_api_error).
- **Part 5 — user upsert + encrypted token persistence wiring:** `users.Repository` is an interface (concrete `gormRepository`) so service/callback logic is unit-testable with fakes; `users.Service.FindOrCreateByGitHubID` persists via `UpsertByGitHubID` (`ON CONFLICT (github_id) DO UPDATE ... RETURNING`); Aevor UUID preserved on re-login, token replaced.
- **Part 6 — Secure GitHub access-token storage:** AES-256-GCM encrypt/decrypt in `internal/users/crypto.go` (random nonce per call, authenticated, `base64(nonce||ct)`, key-length checks); `GitHubAccessToken *string` `gorm:"type:text" json:"-"` column (Go nil <-> NULL); key from `GITHUB_TOKEN_ENCRYPTION_KEY` env (32-byte hex, validated fail-fast in `pkg/config`); key never in DB/JSON; plaintext discarded after encryption; token never in API responses, JWT, URLs, cookies, localStorage, logs, or error messages. Test matrix in `crypto_test.go`.
- **Part 7 — Aevor JWT issuance:** `HandleCallback` signs a JWT (`jwtManager.Issue`, HS256, sub = Aevor user UUID, exp 7d) after a successful upsert; callback returns `{token, user}` per design D7 (JSON body — browser transport deferred to the frontend task, NOT a cookie). Claims: `sub`/`iss`("aevor")/`aud`("aevor-api")/`iat`/`exp`(7d); no `jti`; no GitHub token, OAuth state, PKCE verifier, or profile data in the JWT. `JWTManager` has a defensive ≥32-byte signing-secret guard (`ErrInvalidJWTSecret`) and an injectable clock (`WithClock`); `pkg/config` fails fast on a missing/short `JWT_SECRET`. Tests: `internal/auth/jwt_test.go`, `pkg/config/config_test.go`, callback integration tests.
- **Part 8 — JWT Authentication Middleware:** `RequireAuth(manager)` enforces `Authorization: Bearer <Aevor JWT>` (no cookie, no client-supplied identity). Extraction (`bearerToken` — exact `Bearer ` prefix, non-empty), validation (`manager.Verify`: HS256 whitelist via `WithValidMethods`, `WithIssuer`, `WithAudience`, `WithExpirationRequired`, `sub` parsed as UUID), then the verified `uuid.UUID` is stored in Gin context under the typed `UserIDKey`; a single `abortUnauthorized` helper returns a uniform `401 {"error":"unauthorized"}`. `GetAuthenticatedUserID(c)` helper returns `(uuid.Nil, false)` safely when no verified identity exists; handlers consume identity only from context, never parse JWTs. JWT never logged, never echoed in error bodies. Tests: `internal/auth/middleware_test.go` (12 test functions incl. algorithm-confusion RS256/ES256/HS384/HS512/none rejections; deterministic `tamperToken`; `-count=20` stable).

### Task 1 Part 9 — Authenticated Current User Endpoint (test suite; UNCOMMITTED)

- `GET /users/me` endpoint itself is committed (Part 8): proves identity via `GetAuthenticatedUserID` (verified JWT `sub` only) and returns the safe `users.UserResponse` DTO (id/github_id/username/display_name/email/avatar_url).
- Part 9 (2026-08-15) adds the comprehensive endpoint test suite `internal/auth/me_test.go` (13 test functions, covering 17 required cases), currently UNCOMMITTED/untracked in the working tree:
  - valid token → 200 and returned user matches the JWT `sub`;
  - missing / invalid (tampered) / expired tokens → uniform `401 {"error":"unauthorized"}`;
  - a token for another user returns only that user's data;
  - query param / request body / `X-User-Id` header cannot override identity;
  - authenticated UUID unknown → `404 {"error":"user_not_found"}`;
  - repository failure → sanitized `500 {"error":"internal"}` (no DB detail leaked);
  - no GitHub access token, encrypted token, JWT signing secret, OAuth state, or PKCE verifier material in any response (DTO field-set assertion + substring scan);
  - handler fails closed (401) even when mounted without middleware;
  - `/users/me` does not conflict with a `/users/:id` route (static beats param; both routes coexist).
- Verified 2026-08-15 on branch `task1/improve-gitignore`: `gofmt -l .` empty, `go build ./...`, `go vet ./...`, `go test ./...`, `go test -race ./...` all clean (79 test functions in `internal/auth`, 11 in `internal/users` crypto, etc.).

### Task 1 Part 10 — Authentication hardening & integration verification (complete, verified 2026-08-16; UNCOMMITTED)

- **Error surface reconciled with design §6.** Callback now exposes EXACTLY: `invalid_state` (400), `invalid_code` (400), `github_authorization_denied` (401), `github_unavailable` (500), `internal` (500). All GitHub client failures (401/403/429/5xx/unexpected/malformed) collapse to ONE external code `github_unavailable`; the `internal/github` error taxonomy is now internal-only and never leaks beyond the handler.
- **Trust-boundary validation.** `GetCurrentUser` rejects profiles with `id <= 0` or blank login → `ErrInvalidResponse`; `FindOrCreateByGitHubID` validates the profile (`ID > 0`, non-blank `Login`) → new sentinel `ErrInvalidProfile`; the auth service maps it to `github_unavailable`. Malformed GitHub profiles can never be persisted.
- **Safe logging.** Profile-fetch failures logged with reason only — never the GitHub token, OAuth state, PKCE verifier, client secret, or auth code. Gin logger `SkipQueryString: true` (no `code`/`state` in access logs). Verified by `TestCallback_GitHubFailureLogsCarryNoSecrets` (captures the standard logger).
- **Verified library-level defenses (no dead code added):** oauth2 v0.36.0 rejects a missing `access_token` at exchange time; pgx v5.6.0 connection-error text includes user/database but NOT password; HS256 signature verification defeats JWT payload tampering (`TestRequireAuth_ModifiedPayloadRejected`).
- **Dependencies:** `go mod tidy -diff` clean; `quic-go`, `mongo-driver/v2`, `sonic` are transitive deps of gin v1.12.0 (not ours, no action).
- **New tests:** 7-scenario error-collapse table; state-cookie consumed after the flow (replay without cookie → `invalid_state`, exactly one exchange); single-use code replay → `invalid_code` (no profile fetch); client-supplied `github_id`/`user_id` query params ignored (identity only from the verified profile); non-`access_denied` GitHub error → `github_unavailable`; missing `access_token` → `github_unavailable`; no-secret log capture; JWT payload-tamper rejection; negative-id/blank-login rejection (github client + users service).
- **Deferred/documented:** rate limiting on public auth endpoints (deferred decision — localhost stage, GitHub enforces its own limits); `oauthCookieSecure` hardcoded `false` (needs env wiring or default `true` before any non-localhost deployment); no server-side OAuth state store (replay protection relies on single-use code + cleared HttpOnly cookie); real-DB upsert integration tests still pending.
- Verified 2026-08-16: `gofmt -l .` empty; `go build ./...`, `go vet ./...`, `go test ./...`, `go test -race ./...` all pass. Test functions: `internal/auth` 86, `internal/github` 23, `internal/users` 24, `pkg/config` 3.

### Broken-functionality repairs (2026-08-22; committed on `platform` branch `fix/users-upsert-token-column`)

- **FACT (discrepancy resolved):** the OAuth re-login upsert column mismatch was LIVE on `main` even though it had earlier been reported as fixed — no commit on any branch ever contained the correction. `UpsertByGitHubID` referenced nonexistent column `github_access_token` while GORM maps `User.GitHubAccessToken` → `git_hub_access_token`; first-time logins inserted fine, every REPEAT login failed with `column excluded.github_access_token does not exist`.
- FIXED in `platform`: clause corrected to `git_hub_access_token` (`internal/users/repository.go`, columns extracted into package vars); explicit `gorm:"column:git_hub_access_token"` pin on the model (`internal/users/model.go`); schema-consistency regression tests (`repository_upsert_test.go`, proven to fail against the old code); opt-in real-Postgres integration test (`repository_integration_test.go`, gated by `AEVOR_TEST_DATABASE_DSN`) covering insert + conflict-update + UUID preservation + token rotation.
- FIXED: JWT test time bomb (`internal/auth/jwt_test.go`) — four tests issued tokens at fixed 2026-08-15 with 7-day TTL and parsed them with wall-clock validation, failing permanently from 2026-08-22. `parsedClaims` now uses `jwt.WithoutClaimsValidation()`; validity semantics remain covered by `Verify`/middleware tests.
- FACT: local development PostgreSQL is now native Homebrew PostgreSQL 16.15 on host port `5432` (db/user `aevor`); the Docker Compose stack on `5433` is NOT running. `.env` points at `localhost:5432`.
- Verified 2026-08-22: `gofmt -l .` clean; `go build ./...`, `go vet ./...`, `go test ./...`, `go test -race ./...` all pass; integration test passes against live Postgres on `5432`; server boots, `/health` 200, `/auth/github/login` 302, `/users/me` uniform `401` without JWT.

## Current task

- **Task 1: GitHub OAuth end-to-end** (login -> callback -> profile -> upsert -> encrypted token storage -> JWT -> protected API authentication -> `GET /users/me`). Parts 1–10 implemented and COMMITTED (PRs #24–#38). The 2026-08-22 repairs above are uncommitted in the working tree.
- Approved design: `docs/task-1-oauth-design.md` (D1–D13).
- **Task 1 close-out (remaining):** manual browser re-login verification of the live OAuth flow (conflict path now proven at DB level); commit the 2026-08-22 repairs; confirm the security gate (secret rotation + history purge) before shipping anything that depends on the new credentials.

## Current branch

- `platform` repo: branch `main` = `eb409ec` (PRs #29–#32 merged: readme setup guide, postgres healthcheck, env example defaults). Parts 9–10 uncommitted in the working tree. `feat` is STALE — do not treat it as current.
- `docs`, `ai`, `infra` repos: branch `main`.
- FACT: docs files `development-state.md`, `learning.md`, `interview-notes.md`, `task-1-oauth-design.md` exist on disk but are UNTRACKED in the `docs` repo (never committed). Only `GOW.md`, `prd.md`, `decisions.md`, `roadmap.md`, `contributing.md`, `README.md`, `architecture.md`, `api-spec.md`, `database.md` are tracked. The `platform` repo carries its own committed mirrors `docs/todo.md` and `docs/development-state.md` — keep the canonical `docs/` repo and the platform mirrors in sync.

## Known problems

- FACT (2026-08-11, historical): `platform/services/api/.env` was TRACKED in git and PUSHED to `origin` (`https://github.com/Aevor/platform.git`) in commits `b9784cd`/`fd9752c` — those secrets are COMPROMISED. Working tree has `.env` untracked, `.gitignore` + `.env.example` present; verify secret rotation + history purge are complete before production use.
- FACT: Task 1 Parts 1–10 are committed on `main`/`origin/main` (through PR #38, HEAD `1040ebc`).
- FACT (2026-08-22): the upsert column mismatch (`github_access_token` vs `git_hub_access_token`) was live on `main` and broke every repeat OAuth login; fixed on branch `fix/users-upsert-token-column` (commits `098dd4d`, `a8b4479`) with regression + opt-in integration coverage; awaiting push/PR/review by Sanjeev.
- FACT: `go test ./...` was RED on `main` as of 2026-08-22 (JWT time-bomb tests); fixed on the same branch.
- FACT (security gate, verified 2026-08-22): `.env` is untracked and ignored, but the leaked commits `b9784cd`/`fd9752c` are still ancestors of `origin/main` — history purge is NOT done; GitHub-side secret revocation remains unconfirmed. Do not treat the gate as closed.
- FACT: callback issues a JWT and returns `{token, user}` on success (Part 7); the GitHub token stays server-side (encrypted at rest) and is never in the response or the JWT.
- FACT: `RequireAuth` middleware protects `/users/me`; public routes stay public (`/health`, `/auth/github/login`, `/auth/github/callback`); all auth failures abort with a uniform `401 {"error":"unauthorized"}`.
- FACT: real-Postgres integration coverage for the upsert path now EXISTS as an opt-in test (`AEVOR_TEST_DATABASE_DSN`); it is skipped by default so hermetic unit runs are unaffected.
- FACT (Part 10, documented deferral): rate limiting on public auth endpoints is deferred; `oauthCookieSecure` is hardcoded `false` (dev convenience — must be env-driven or `true` before non-localhost deployment); there is no server-side OAuth state store.
- Empty frontend: `platform/apps/web` has no files.
- `infra/kubernetes` and `infra/terraform` exist as empty placeholders only — NOT adopted tech; do not use without a justified requirement.

### Documentation vs code discrepancies

- Port: `docs/learning.md` says host `5432:5432`; actual `docker-compose.yml` maps host `5433:5432`.
- `docs/api-spec.md` lists `GET /auth/github/callback` and `/users/me` (routes now exist) and skills/repositories/issues/recommendations endpoints that do not exist (future stubs).
- `docs/task-1-oauth-design.md` §8 says `GET /user` gets a single bounded retry on transient failures; Part 4 deliberately implements NO retries (per the Part 4 spec) — add the bounded retry in a later stage or confirm removal from the design. (The §6 error surface is now reconciled in Part 10.)
- `docs/learning.md` (Day 9) describes OAuth as "login or create via `GetUserByGitHubID`"; the approved Task 1 design supersedes this with upsert by `github_id` (`FindOrCreateByGitHubID`).
- `docs/database.md` lists users/skills/user_skills/repositories/issues/recommendations; only the `users` table exists in code.
- `docs/task-1-oauth-design.md` §8 says `GET /user` gets a single bounded retry on transient failures; Part 4 deliberately implements NO retries (per the Part 4 spec) — add the bounded retry in a later stage or confirm removal from the design.

## Pending decisions

- Frontend token-storage strategy (deliberately deferred to the frontend task).
- Frontend framework/scaffold (React assumed from team role; `apps/web` empty).
- Versioned migrations tool (GORM `AutoMigrate` retained for now; golang-migrate flagged as a separate foundation task).
- Token-encryption key management (env var for V1; KMS is a prod upgrade).
- PKCE/state cookie parameters and storage are finalized in the Task 1 design (D2).

## Next task

- **Task 1 close-out:** Sanjeev pushes `fix/users-upsert-token-column`, opens PR, reviews, approves, merges; then performs the manual browser OAuth re-login check (exercises the fixed conflict path live). Confirm security-gate completion (secret rotation + history purge) before anything that depends on the new credentials.
- **Then Task 2:** GitHub data synchronization (repositories, issues, commits, pull requests) using the stored encrypted GitHub access token via a server-side retrieve+decrypt path.

## Blocked tasks

- None currently. (Security-gate blocking conditions were resolved in the working tree: `.env` untracked, `.gitignore` + `.env.example` present. Verify secret rotation and history purge explicitly before anything that depends on the old credentials.)

## Important architectural decisions

- Modular monolith for V1; four separate repositories kept separate.
- Deterministic engine first; LLM is perception/interpretation only, never the decision maker.
- Auth: GitHub Authorization Code + PKCE S256 + random state; upsert user by `github_id`; GitHub access token encrypted at rest (AES-256-GCM), separate env key, never in responses/JWT/logs; Aevor JWT (HS256, 7d) returned by callback and sent as `Authorization: Bearer`; `RequireAuth` middleware enforces HS256/exp/iss/aud and derives identity ONLY from the verified `sub` — never from query/body/custom headers; uniform `401 {"error":"unauthorized"}` on any failure; JWT never logged.
- DECISION (Part 10): the callback exposes exactly the design §6 error set (`invalid_state`/`invalid_code`/`github_authorization_denied`/`github_unavailable`/`internal`); every GitHub client failure collapses to the single external code `github_unavailable`, and GitHub profile invariants (positive id, non-blank login) are validated at the trust boundary (`internal/github` + `internal/users`), so untrusted GitHub content can never inject identity or persist malformed data.
- Authorization-code exchange is NEVER retried (single-use code). Design calls for a bounded single retry on idempotent `GET /user` transient failures (network, timeout, HTTP 5xx); Part 4 implements NO retries yet (per the Part 4 spec) — see discrepancy note above.
- Email best-effort; `user:email` scope dropped; no email abstractions.
- `GitHubAccessToken` stored as `*string` `gorm:"type:text" json:"-"` (Go nil <-> PostgreSQL NULL).
- `LastSyncedAt` is NOT in Task 1 (belongs to ingestion/sync).
- Replace public user routes with `GET /users/me` (removes IDOR by construction).
- PostgreSQL is the only V1 database. No Kubernetes/Kafka/Redis/Terraform/vector DB unless a requirement justifies them.
