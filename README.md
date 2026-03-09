# 🏆 Rankings API

[![Fastify](https://img.shields.io/badge/Fastify-5-000000?logo=fastify&logoColor=white)](https://github.com/fastify/fastify)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript&logoColor=white)](https://github.com/microsoft/TypeScript)
[![Node.js](https://img.shields.io/badge/Node.js-24-5FA04E?logo=nodedotjs&logoColor=white)](https://github.com/nodejs/node)
[![Zod](https://img.shields.io/badge/Zod-3-3068B7?logo=zod&logoColor=white)](https://github.com/colinhacks/zod)
[![Pino](https://img.shields.io/badge/Pino-9-687634?logo=pino&logoColor=white)](https://github.com/pinojs/pino)
[![pnpm](https://img.shields.io/badge/pnpm-10-F69220?logo=pnpm&logoColor=white)](https://github.com/pnpm/pnpm)
[![ESLint](https://img.shields.io/badge/ESLint-10-4B32C3?logo=eslint&logoColor=white)](https://github.com/eslint/eslint)
[![Prettier](https://img.shields.io/badge/Prettier-3-F7B93E?logo=prettier&logoColor=black)](https://github.com/prettier/prettier)
[![Vitest](https://img.shields.io/badge/Vitest-3-6E9F18?logo=vitest&logoColor=white)](https://github.com/vitest-dev/vitest)
[![Docker](https://img.shields.io/badge/Docker-ready-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![All Rights Reserved](https://img.shields.io/badge/License-All%20Rights%20Reserved-red.svg)](LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-scottdreinhart%2Frankings-api-181717?logo=github&logoColor=white)](https://github.com/scottdreinhart/rankings-api)

Fastify API backend for the King of the Hill multiplayer ranking and leaderboard system

**⚠️ PROPRIETARY SOFTWARE — All Rights Reserved**

© 2026 Scott Reinhart. This software is proprietary and confidential.
Unauthorized reproduction, distribution, or use is strictly prohibited.
See [LICENSE](LICENSE) file for complete terms and conditions.

> [!CAUTION]
> **LICENSE TRANSITION PLANNED** — This project is currently proprietary. The license will change to open source once the project has reached a suitable state to allow for it.

[Project Structure](#project-structure) · [Getting Started](#getting-started) · [Tech Stack](#tech-stack) · [API Reference](#api-reference) · [Architecture](#architecture) · [Environment Variables](#environment-variables) · [Authentication](#authentication) · [Error Handling](#error-handling) · [Rate Limiting](#rate-limiting) · [Data Models](#data-models) · [Deployment](#deployment) · [Supported Clients](#supported-clients) · [Versioning](#versioning) · [Remaining Work](#remaining-work) · [Future Improvements](#future-improvements) · [Contributing](#contributing) · [Portfolio Services](#portfolio-services) · [Portfolio Games](#portfolio-games)

## Project Structure

```
src/
├── domain/                           # Pure, framework-agnostic logic
│   ├── types.ts                      # Central type definitions (players, matches, rankings, seasons, Elo ratings, streaks, and leaderboard snapshots)
│   ├── errors.ts                     # Domain error classes (NotFoundError, ValidationError, etc.)
│   ├── constants.ts                  # Domain constants (page sizes, timeouts)
│   ├── schemas.ts                    # Zod validation schemas (pagination, ID params)
│   └── index.ts                      # Barrel export — re-exports all domain modules
├── app/
│   ├── app.ts                        # Fastify instance builder (plugins, routes, middleware)
│   └── index.ts                      # Barrel export — re-exports app builder
├── infra/
│   ├── config.ts                     # Environment configuration (validates env vars)
│   ├── logger.ts                     # Pino structured logger (standalone, non-request)
│   └── index.ts                      # Barrel export — re-exports infra modules
├── routes/
│   ├── health.ts                     # GET /health + GET /ready system endpoints
│   └── index.ts                      # Barrel export — re-exports all route modules
├── __tests__/
│   └── health.test.ts                # Health endpoint integration tests
└── server.ts                         # Entrypoint — starts the Fastify server

Dockerfile                            # Multi-stage production Docker image
.dockerignore                         # Docker build exclusions
.env.example                          # Environment variable template
package.json                          # Dependencies & scripts
pnpm-lock.yaml                        # pnpm lockfile
pnpm-workspace.yaml                   # pnpm workspace config
LICENSE                               # Proprietary license terms

tsconfig.json                         # TypeScript config (strict mode + @/ path aliases)
tsconfig.build.json                   # TypeScript build config (emits to dist/)
eslint.config.js                      # ESLint flat config (TypeScript + Prettier + boundary enforcement)
vitest.config.ts                      # Vitest test runner config (path aliases + coverage)
.prettierrc                           # Prettier formatting rules
.gitignore                            # Git ignore rules
.nvmrc                                # Node.js version pin (v24)
```

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) v24+ (pin via [nvm](https://github.com/nvm-sh/nvm) — see `.nvmrc`)
- [pnpm](https://pnpm.io/) v10+

### Install & Run

```bash
# Install dependencies
pnpm install

# Start development server (auto-restart on file changes)
pnpm dev

# Start production server (no file watching)
pnpm start

# Build for production (compiles TypeScript to dist/)
pnpm build

# Run compiled production build
pnpm preview
```

### Code Quality

```bash
# Individual tools
pnpm lint           # ESLint — check for issues
pnpm lint:fix       # ESLint — auto-fix issues
pnpm format         # Prettier — format all source files
pnpm format:check   # Prettier — check formatting without writing
pnpm typecheck      # TypeScript type check (tsc --noEmit)

# Chains
pnpm check          # lint + format:check + typecheck in one pass (quality gate)
pnpm fix            # lint:fix + format in one pass (auto-fix everything)
pnpm validate       # check + build — full pre-push validation
```

### Testing

```bash
pnpm test           # Run all tests once
pnpm test:watch     # Watch mode — re-run on file changes
pnpm test:coverage  # Run with V8 coverage report
```

### Docker

```bash
# Build the image
docker build -t rankings-api .

# Run the container
docker run -p 3000:3000 rankings-api
```

### Cleanup & Maintenance

```bash
pnpm clean          # wipe dist/ build output
pnpm clean:node     # delete node_modules only
pnpm clean:all      # nuclear — dist/ + node_modules/
pnpm reinstall      # clean:all + pnpm install
```

## Tech Stack

| Technology | Version | Purpose |
|---|---|---|
| [Fastify](https://github.com/fastify/fastify) | 5 | High-performance web framework |
| [TypeScript](https://github.com/microsoft/TypeScript) | 5.9 | Static type checking (strict mode) |
| [Node.js](https://github.com/nodejs/node) | 24 | Runtime (pinned via `.nvmrc`) |
| [Zod](https://github.com/colinhacks/zod) | 3 | Runtime schema validation |
| [Pino](https://github.com/pinojs/pino) | 9 | Structured JSON logging |
| [@fastify/swagger](https://github.com/fastify/fastify-swagger) | 9 | OpenAPI spec generation |
| [@fastify/swagger-ui](https://github.com/fastify/fastify-swagger-ui) | 5 | Interactive API documentation (`/docs`) |
| [@fastify/cors](https://github.com/fastify/fastify-cors) | 11 | Cross-origin resource sharing |
| [@fastify/helmet](https://github.com/fastify/fastify-helmet) | 13 | Security headers |
| [@fastify/rate-limit](https://github.com/fastify/fastify-rate-limit) | 10 | Request rate limiting |
| [Vitest](https://github.com/vitest-dev/vitest) | 3 | Test runner (V8 coverage) |
| [ESLint](https://github.com/eslint/eslint) | 10 | Linting (flat config + boundary enforcement) |
| [Prettier](https://github.com/prettier/prettier) | 3 | Code formatting |
| [pnpm](https://github.com/pnpm/pnpm) | 10 | Fast, disk-efficient package manager |
| [Docker](https://www.docker.com/) | — | Containerized production deployment |

## API Reference

Once the server is running, interactive OpenAPI documentation is available at:

```
http://localhost:3000/docs
```

### Built-in Endpoints

| Method | Path | Description |
|---|---|---|
| `GET` | `/health` | Health check — returns status, uptime, timestamp |
| `GET` | `/ready` | Readiness check — returns `{ ready: true }` when all dependencies are available |
| `GET` | `/docs` | Swagger UI — interactive API documentation |

### Proposed Endpoints

#### Players

| Method | Path | Description |
|---|---|---|
| `POST` | `/players` | Register a new player profile |
| `GET` | `/players` | List players (paginated, searchable by display name) |
| `GET` | `/players/:id` | Get player profile, lifetime stats, and current rating |
| `PATCH` | `/players/:id` | Update player display name or avatar |
| `GET` | `/players/:id/stats` | Get detailed stats breakdown (wins, losses, draws, streaks per game) |
| `GET` | `/players/:id/history` | Get match history (paginated, filterable by game/season) |

#### Matches

| Method | Path | Description |
|---|---|---|
| `POST` | `/matches` | Submit a match result (game, players, outcome, duration, moves) |
| `GET` | `/matches` | List recent matches (paginated, filterable by game/player/season) |
| `GET` | `/matches/:id` | Get match details (players, result, rating changes, replay data) |
| `POST` | `/matches/:id/verify` | Verify/dispute a match result (anti-cheat) |

#### Rankings & Leaderboards

| Method | Path | Description |
|---|---|---|
| `GET` | `/leaderboards` | List available leaderboards (one per game per season) |
| `GET` | `/leaderboards/:gameId` | Get current leaderboard for a game (paginated, top-N) |
| `GET` | `/leaderboards/:gameId/around/:playerId` | Get leaderboard centered around a player’s rank |
| `GET` | `/leaderboards/:gameId/top` | Get top 10/25/50/100 players for a game |
| `GET` | `/rankings/:playerId` | Get a player’s rank and rating across all games |

#### Seasons

| Method | Path | Description |
|---|---|---|
| `POST` | `/seasons` | Create a new competitive season (start date, end date, rules) |
| `GET` | `/seasons` | List all seasons (past, current, upcoming) |
| `GET` | `/seasons/:id` | Get season details, rules, and final standings |
| `GET` | `/seasons/current` | Get the active season |
| `POST` | `/seasons/:id/finalize` | End a season and snapshot final leaderboard standings (admin) |

#### Elo Ratings

| Method | Path | Description |
|---|---|---|
| `GET` | `/ratings/:playerId` | Get player’s current Elo rating per game |
| `GET` | `/ratings/:playerId/history` | Get Elo rating history over time (for chart visualization) |
| `GET` | `/ratings/distribution` | Get rating distribution stats (bell curve data per game) |

#### Achievements & Streaks

| Method | Path | Description |
|---|---|---|
| `GET` | `/achievements` | List all available achievements/badges |
| `GET` | `/achievements/:playerId` | Get achievements earned by a player |
| `POST` | `/achievements` | Define a new achievement (admin) |
| `GET` | `/streaks/:playerId` | Get player’s current and best win/loss streaks per game |

#### King of the Hill

| Method | Path | Description |
|---|---|---|
| `GET` | `/king-of-the-hill/:gameId` | Get the current “King” (top-ranked player) and their reign duration |
| `GET` | `/king-of-the-hill/:gameId/history` | Get history of Kings (who held the top spot and for how long) |
| `POST` | `/king-of-the-hill/:gameId/challenge` | Submit a challenge attempt against the current King |

#### Tournaments

| Method | Path | Description |
|---|---|---|
| `POST` | `/tournaments` | Create a tournament (name, game, format, max participants, start date) |
| `GET` | `/tournaments` | List tournaments (paginated, filterable by game/status/season) |
| `GET` | `/tournaments/:id` | Get tournament details, bracket, and current round |
| `PATCH` | `/tournaments/:id` | Update tournament settings (admin) |
| `POST` | `/tournaments/:id/register` | Register a player for a tournament |
| `DELETE` | `/tournaments/:id/register/:playerId` | Withdraw a player from a tournament |
| `GET` | `/tournaments/:id/bracket` | Get full bracket with match results |
| `POST` | `/tournaments/:id/advance` | Advance the tournament to the next round (admin) |
| `POST` | `/tournaments/:id/finalize` | Finalize tournament and distribute rewards (admin) |

#### Friends & Social

| Method | Path | Description |
|---|---|---|
| `POST` | `/friends/request` | Send a friend request to another player |
| `GET` | `/friends` | List current friends |
| `GET` | `/friends/requests` | List pending friend requests (incoming + outgoing) |
| `POST` | `/friends/requests/:id/accept` | Accept a friend request |
| `POST` | `/friends/requests/:id/reject` | Reject a friend request |
| `DELETE` | `/friends/:playerId` | Remove a friend |
| `GET` | `/friends/leaderboard/:gameId` | Get friend-scoped leaderboard for a game |

#### Challenges (Direct)

| Method | Path | Description |
|---|---|---|
| `POST` | `/challenges` | Challenge a specific player to a match |
| `GET` | `/challenges` | List pending challenges (incoming + outgoing) |
| `GET` | `/challenges/:id` | Get challenge details |
| `POST` | `/challenges/:id/accept` | Accept a challenge |
| `POST` | `/challenges/:id/decline` | Decline a challenge |
| `DELETE` | `/challenges/:id` | Cancel an outgoing challenge |

#### Replays

| Method | Path | Description |
|---|---|---|
| `POST` | `/replays` | Store a move-by-move game replay |
| `GET` | `/replays/:matchId` | Get replay data for a match |
| `GET` | `/replays/featured` | Get featured/highlighted replays (best games of the week) |
| `DELETE` | `/replays/:matchId` | Remove replay data (admin) |

#### Reports & Moderation

| Method | Path | Description |
|---|---|---|
| `POST` | `/reports` | Report a player for cheating or unsportsmanlike behavior |
| `GET` | `/reports` | List reports (paginated, filterable by status — admin) |
| `GET` | `/reports/:id` | Get report details and evidence |
| `POST` | `/reports/:id/resolve` | Resolve a report with action taken (warn, suspend, ban — admin) |
| `GET` | `/bans` | List banned/suspended players (admin) |
| `POST` | `/bans` | Ban or suspend a player (admin) |
| `DELETE` | `/bans/:playerId` | Lift a ban or suspension (admin) |

#### Notifications

| Method | Path | Description |
|---|---|---|
| `GET` | `/notifications/:playerId` | List recent notifications for a player |
| `POST` | `/notifications/mark-read` | Mark notifications as read |
| `GET` | `/notifications/:playerId/preferences` | Get notification preferences (challenge alerts, rank changes, etc.) |
| `PATCH` | `/notifications/:playerId/preferences` | Update notification preferences |

### Client UI Pairings

Endpoints that pair directly with a visible UI element in game clients or admin panels:

| UI Element | Icon / Control | Endpoint | Surface |
|---|---|---|---|
| Player avatar | Circular avatar image + display name | `GET /players/:id` | Game client — profile card / match screen |
| Avatar upload button | 📷 camera icon overlay on avatar | `PATCH /players/:id` | Game client — profile edit |
| Stats dashboard | Win/loss/draw counters + pie chart | `GET /players/:id/stats` | Game client — player profile |
| Match history list | Scrollable list with W/L/D badges per row | `GET /players/:id/history` | Game client — profile detail |
| Leaderboard table | Ranked list with 🥇🥈🥉 position medals | `GET /leaderboards/:gameId` | Game client — leaderboard screen |
| "Your Rank" card | Rank badge with up/down arrow vs. previous | `GET /leaderboards/:gameId/around/:playerId` | Game client — leaderboard (auto-scrolls to player) |
| Top players podium | Podium graphic with top 3 avatars | `GET /leaderboards/:gameId/top` | Game client — leaderboard header |
| Friends tab toggle | 👥 "Friends" / 🌐 "Global" tab switch | `GET /friends/leaderboard/:gameId` | Game client — leaderboard tab bar |
| Elo rating badge | Numeric rating + rank tier icon (Bronze, Silver, Gold, etc.) | `GET /ratings/:playerId` | Game client — player card / match result |
| Rating history chart | 📈 line chart (Elo over time) | `GET /ratings/:playerId/history` | Game client — player profile detail |
| Rating distribution curve | Bell curve visualization | `GET /ratings/distribution` | Game client — leaderboard stats tab |
| Achievement badge grid | 🏆 trophy/badge icons in a grid (earned = color, unearned = gray) | `GET /achievements/:playerId` | Game client — achievements screen |
| Achievement unlock toast | 🏆 popup notification with badge icon + name | `POST /achievements` (triggers client event) | Game client — overlay after match |
| Win streak flame | 🔥 flame icon with streak count number | `GET /streaks/:playerId` | Game client — player card / match result |
| Crown icon (King of the Hill) | 👑 crown icon on top-ranked player | `GET /king-of-the-hill/:gameId` | Game client — leaderboard #1 row |
| "Challenge the King" button | ⚔ crossed swords + 👑 crown icon | `POST /king-of-the-hill/:gameId/challenge` | Game client — King profile card |
| King reign timer | ⏱ duration counter ("King for 3d 5h") | `GET /king-of-the-hill/:gameId` | Game client — leaderboard #1 row |
| King history timeline | 👑 timeline of past Kings with avatars | `GET /king-of-the-hill/:gameId/history` | Game client — King of the Hill detail |
| Season banner | 🏅 decorated banner with season name + dates | `GET /seasons/current` | Game client — leaderboard header |
| Season countdown | ⏳ countdown timer to season end | `GET /seasons/current` | Game client — leaderboard header |
| "Add Friend" button | 👤➕ person-plus icon | `POST /friends/request` | Game client — player profile / post-match |
| Accept friend button | ✅ checkmark icon on request row | `POST /friends/requests/:id/accept` | Game client — friend requests list |
| Reject friend button | ❌ X icon on request row | `POST /friends/requests/:id/reject` | Game client — friend requests list |
| Remove friend button | 👤➖ person-minus icon | `DELETE /friends/:playerId` | Game client — friend list row |
| Friend request badge | 🔴 numeric badge on friends icon | `GET /friends/requests` | Game client — navigation bar |
| "Challenge" button | ⚔ crossed swords icon | `POST /challenges` | Game client — friend list row / player profile |
| Accept challenge button | ✅ "Accept" button on challenge card | `POST /challenges/:id/accept` | Game client — challenge notification |
| Decline challenge button | ❌ "Decline" button on challenge card | `POST /challenges/:id/decline` | Game client — challenge notification |
| Cancel challenge button | ✕ close icon on outgoing challenge | `DELETE /challenges/:id` | Game client — sent challenges list |
| "Join Tournament" button | 🏟 tournament icon + "Join" CTA | `POST /tournaments/:id/register` | Game client — tournament detail |
| "Leave Tournament" button | 🚪 exit icon | `DELETE /tournaments/:id/register/:playerId` | Game client — registered tournament |
| Tournament bracket view | Bracket tree visualization | `GET /tournaments/:id/bracket` | Game client — tournament detail |
| Tournament status badge | 🟢 Open / 🟡 In Progress / 🔴 Complete pill | `GET /tournaments/:id` | Game client — tournament card |
| Replay play button | ▶ play icon on match history row | `GET /replays/:matchId` | Game client — match detail |
| Featured replay badge | ⭐ star badge on replay row | `GET /replays/featured` | Game client — replays section |
| "Report Player" button | 🚩 flag icon on player profile | `POST /reports` | Game client — player profile context menu |
| Match dispute button | ⚠ warning triangle on match row | `POST /matches/:id/verify` | Game client — match detail |
| Resolve report button | ⚖ gavel icon + action dropdown | `POST /reports/:id/resolve` | Admin panel — report queue |
| Ban player button | 🔨 ban hammer icon | `POST /bans` | Admin panel — player moderation |
| Unban button | 🔓 unlock icon | `DELETE /bans/:playerId` | Admin panel — banned players list |
| Notification bell | 🔔 bell icon with unread count badge | `GET /notifications/:playerId` | Game client — navigation bar |
| "Mark All Read" button | ✅ checkmark icon in notification dropdown | `POST /notifications/mark-read` | Game client — notification panel header |
| Notification preference toggles | Toggle switches per category | `PATCH /notifications/:playerId/preferences` | Game client — settings screen |

## Architecture

This project enforces seven complementary design principles:

1. **CLEAN Architecture** (Layer Separation)
   - `domain/` layer: Pure types, errors, constants, schemas (zero framework dependencies)
   - `app/` layer: Fastify instance builder, use-case services
   - `infra/` layer: External adapters (database, cache, third-party APIs)
   - `routes/` layer: HTTP route handlers (thin — delegates to app/domain)
   - **Benefit**: Domain logic is testable, reusable, and framework-independent

2. **SOLID Principles** (Code-Level Design)
   - Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion
   - **Benefit**: Code is maintainable, testable, and resistant to side effects

3. **DRY Principle** (No Duplication)
   - Shared schemas, constants, and error types eliminate duplication
   - **Benefit**: Changes propagate consistently; less code to maintain

4. **Import Boundary Enforcement** (`eslint-plugin-boundaries`)
   - `domain/` → may only import from `domain/` (zero framework deps)
   - `app/` → may import `domain/` + `app/` (never `infra/` or `routes/`)
   - `infra/` → may import `domain/`, `app/`, and `infra/`
   - `routes/` → may import `domain/`, `app/`, and `routes/`
   - **Benefit**: CLEAN layer violations are caught at lint time, not at code review

5. **Path Aliases** (`@/domain`, `@/app`, `@/infra`, `@/routes`)
   - Configured in `tsconfig.json` (`paths`) and `vitest.config.ts` (`resolve.alias`)
   - Eliminates fragile `../../` relative imports across layers
   - **Benefit**: Imports are self-documenting and resilient to file moves

6. **Barrel Exports** (`index.ts` per directory)
   - Each layer exposes a single public API via its barrel file
   - Internal module structure can change without breaking consumers
   - **Benefit**: Explicit public APIs; refactoring internals doesn't cascade import changes

7. **Schema-First Validation** (Zod + OpenAPI)
   - All inputs validated with Zod schemas in the domain layer
   - Fastify route schemas auto-generate OpenAPI docs via `@fastify/swagger`
   - **Benefit**: Single source of truth for validation, serialization, and documentation

## Environment Variables

Copy `.env.example` to `.env` and configure:

| Variable | Description | Type | Default | Required |
| -------- | ----------- | ---- | ------- | -------- |
| `PORT` | HTTP server port | `number` | `3000` | No |
| `HOST` | Bind address | `string` | `0.0.0.0` | No |
| `NODE_ENV` | Runtime environment (`development`, `staging`, `production`) | `string` | `development` | No |
| `LOG_LEVEL` | Pino log level (`fatal`, `error`, `warn`, `info`, `debug`, `trace`) | `string` | `info` | No |
| `DATABASE_URL` | PostgreSQL connection string | `string` | — | **Yes** (production) |
| `REDIS_URL` | Redis connection string for leaderboard caching and pub/sub | `string` | — | **Yes** (production) |
| `API_KEY` | Service-to-service API key for internal callers | `string` | — | **Yes** (production) |
| `JWT_SECRET` | Secret used to sign and verify JWT access tokens | `string` | — | **Yes** (production) |
| `BILLING_API_URL` | Internal Billing API base URL for entitlement checks | `string` | `http://localhost:3001` | No |
| `BILLING_API_KEY` | API key for authenticating with the Billing API | `string` | — | **Yes** (production) |
| `WS_ENABLED` | Enable WebSocket server for live leaderboard updates | `boolean` | `false` | No |
| `WS_PORT` | WebSocket server port (if separate from HTTP) | `number` | `3001` | No |
| `ELO_DEFAULT_K` | Default Elo K-factor for new players | `number` | `32` | No |
| `ELO_ESTABLISHED_K` | Elo K-factor for established players (30+ games) | `number` | `16` | No |
| `SEASON_AUTO_ROTATE` | Whether to automatically rotate seasons on schedule | `boolean` | `true` | No |
| `REPLAY_STORAGE_BUCKET` | S3-compatible bucket for game replay storage | `string` | — | No |
| `REPLAY_STORAGE_REGION` | Cloud storage region for replays | `string` | `us-east-1` | No |
| `CORS_ORIGIN` | Allowed CORS origin(s), comma-separated | `string` | `*` | No |
| `RATE_LIMIT_MAX` | Max requests per rate-limit window | `number` | `100` | No |
| `RATE_LIMIT_WINDOW_MS` | Rate-limit window duration in milliseconds | `number` | `60000` | No |

## Authentication

All non-public endpoints require authentication. The API supports two authentication methods:

### JWT Bearer Tokens (Game Clients & Admin Apps)

Game clients and admin apps authenticate by sending a JWT in the `Authorization` header:

```
Authorization: Bearer <token>
```

- Tokens are issued by the auth service upon successful login
- Tokens contain the user's `sub` (user ID) and `roles` array
- Tokens expire after a configurable TTL (default: 1 hour)
- Refresh tokens are used to obtain new access tokens without re-authentication

### API Keys (Service-to-Service)

Internal services authenticate with a static API key in the `X-API-Key` header:

```
X-API-Key: <key>
```

- Used by admin apps and other portfolio APIs for server-to-server calls
- Keys are configured via the `API_KEY` environment variable
- API key requests bypass user-scoped authorization checks

### Public Endpoints

The following endpoints do not require authentication:

| Endpoint | Purpose |
| -------- | ------- |
| `GET /health` | Health check |
| `GET /ready` | Readiness probe |
| `GET /docs` | Swagger UI |
| `GET /docs/json` | OpenAPI spec |
| `GET /leaderboards/:gameId` | Public leaderboard viewing |

## Error Handling

All error responses follow a consistent JSON structure:

```json
{
  "statusCode": 409,
  "error": "Conflict",
  "message": "Player 'alex42' is already enrolled in tournament 'summer-showdown'",
  "code": "TOURNAMENT_ALREADY_ENROLLED"
}
```

| Field | Type | Description |
| ----- | ---- | ----------- |
| `statusCode` | `number` | HTTP status code |
| `error` | `string` | HTTP status text |
| `message` | `string` | Human-readable error description |
| `code` | `string?` | Machine-readable domain error code (optional) |

### HTTP Status Codes

| Status | Meaning | Example |
| ------ | ------- | ------- |
| `400` | Bad Request | Malformed JSON body or invalid match result |
| `401` | Unauthorized | Missing or expired JWT / invalid API key |
| `403` | Forbidden | Valid auth but insufficient role (e.g., non-admin banning players) |
| `404` | Not Found | Player, match, season, or tournament not found |
| `409` | Conflict | Already enrolled in tournament, duplicate friend request |
| `422` | Unprocessable Entity | Business rule violation (e.g., match against self, invalid Elo) |
| `429` | Too Many Requests | Rate limit exceeded |
| `500` | Internal Server Error | Unexpected failure |

### Domain Error Codes

| Code | Description |
| ---- | ----------- |
| `PLAYER_NOT_FOUND` | No player profile exists with the given ID |
| `PLAYER_BANNED` | Player account is banned and cannot participate |
| `MATCH_NOT_FOUND` | No match exists with the given ID |
| `MATCH_ALREADY_RESOLVED` | Match result has already been submitted |
| `MATCH_SELF_PLAY` | Cannot submit a match where both players are the same |
| `SEASON_NOT_FOUND` | No season exists with the given ID |
| `SEASON_ENDED` | Season has ended; no new matches accepted |
| `SEASON_NOT_STARTED` | Season has not begun yet |
| `TOURNAMENT_NOT_FOUND` | No tournament exists with the given ID |
| `TOURNAMENT_ALREADY_ENROLLED` | Player is already enrolled in this tournament |
| `TOURNAMENT_FULL` | Tournament has reached its maximum participant count |
| `TOURNAMENT_REGISTRATION_CLOSED` | Registration period has ended |
| `CHALLENGE_NOT_FOUND` | No challenge exists with the given ID |
| `CHALLENGE_SELF_CHALLENGE` | Cannot challenge yourself |
| `CHALLENGE_ALREADY_PENDING` | An active challenge already exists between these players |
| `FRIEND_REQUEST_DUPLICATE` | Friend request already sent or friendship already exists |
| `REPLAY_NOT_FOUND` | No replay exists for the given match |
| `REPORT_DUPLICATE` | This player/match has already been reported by this user |

## Rate Limiting

Rate limiting is enforced via `@fastify/rate-limit` to protect against abuse:

| Scope | Limit | Window | Notes |
| ----- | ----- | ------ | ----- |
| **Global default** | 100 requests | 60 seconds | Per IP address |
| **Leaderboard reads** | 300 requests | 60 seconds | High-frequency public queries |
| **Match submissions** | 30 requests | 60 seconds | Prevents result flooding |
| **Player profile updates** | 20 requests | 60 seconds | Avatar, display name changes |
| **Friend / challenge requests** | 30 requests | 60 seconds | Social interactions |
| **Report submissions** | 5 requests | 60 seconds | Abuse prevention |
| **Health / readiness** | Unlimited | — | Excluded from rate limiting |

Rate-limited responses include standard headers:

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 47
X-RateLimit-Reset: 1719000060
Retry-After: 12
```

When the limit is exceeded, the API returns `429 Too Many Requests` with the error body above.

## Data Models

Planned domain entities for the rankings system. These will be implemented as Zod schemas in `src/domain/types.ts`:

### Player

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string (uuid)` | Unique player identifier |
| `userId` | `string (uuid)` | Auth system user ID |
| `displayName` | `string` | Public display name |
| `avatarUrl` | `string?` | Profile avatar URL |
| `eloRating` | `number` | Current Elo rating |
| `peakRating` | `number` | Highest Elo ever achieved |
| `gamesPlayed` | `number` | Total games played |
| `wins` | `number` | Total wins |
| `losses` | `number` | Total losses |
| `draws` | `number` | Total draws |
| `currentStreak` | `number` | Current win streak (negative = loss streak) |
| `longestStreak` | `number` | Longest win streak ever |
| `rank` | `number?` | Current leaderboard rank |
| `tier` | `'bronze' \| 'silver' \| 'gold' \| 'platinum' \| 'diamond' \| 'master'` | Rank tier |
| `isBanned` | `boolean` | Whether the player is banned |
| `createdAt` | `string (ISO 8601)` | Registration timestamp |
| `lastActiveAt` | `string (ISO 8601)` | Last activity timestamp |

### Match

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string (uuid)` | Unique match identifier |
| `gameId` | `string` | Which portfolio game was played |
| `seasonId` | `string (uuid)` | Season this match belongs to |
| `player1Id` | `string (uuid)` | First player |
| `player2Id` | `string (uuid)` | Second player |
| `winnerId` | `string (uuid)?` | Winner (`null` = draw) |
| `player1RatingBefore` | `number` | Player 1's Elo before the match |
| `player2RatingBefore` | `number` | Player 2's Elo before the match |
| `player1RatingAfter` | `number` | Player 1's Elo after the match |
| `player2RatingAfter` | `number` | Player 2's Elo after the match |
| `ratingDelta` | `number` | Absolute Elo change |
| `moves` | `number` | Total moves in the game |
| `durationMs` | `number` | Game duration in milliseconds |
| `replayId` | `string (uuid)?` | Associated replay (if stored) |
| `playedAt` | `string (ISO 8601)` | When the match occurred |

### Season

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string (uuid)` | Unique season identifier |
| `gameId` | `string` | Which game this season is for |
| `name` | `string` | Season name (e.g., `Season 3 — Summer 2026`) |
| `status` | `'upcoming' \| 'active' \| 'ended'` | Season state |
| `startDate` | `string (ISO 8601)` | Season start |
| `endDate` | `string (ISO 8601)` | Season end |
| `startingElo` | `number` | Default Elo for new players entering this season |
| `eloResetPolicy` | `'full' \| 'soft' \| 'none'` | How ratings carry over from the previous season |
| `matchCount` | `number` | Total matches played this season |
| `playerCount` | `number` | Total active players |

### Tournament

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string (uuid)` | Unique tournament identifier |
| `gameId` | `string` | Which game the tournament is for |
| `name` | `string` | Tournament name |
| `format` | `'single_elimination' \| 'double_elimination' \| 'round_robin' \| 'swiss'` | Bracket format |
| `status` | `'registration' \| 'in_progress' \| 'completed' \| 'canceled'` | Tournament state |
| `maxParticipants` | `number` | Maximum allowed participants |
| `currentParticipants` | `number` | Currently enrolled players |
| `minElo` | `number?` | Minimum Elo to enter |
| `maxElo` | `number?` | Maximum Elo to enter |
| `prizeDescription` | `string?` | Prize details |
| `registrationDeadline` | `string (ISO 8601)` | Enrollment cutoff |
| `startDate` | `string (ISO 8601)` | Tournament start |
| `endDate` | `string (ISO 8601)?` | Tournament end |
| `bracketData` | `object` | Serialized bracket structure |

### Achievement

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string` | Achievement identifier (e.g., `first_win`, `10_game_streak`) |
| `name` | `string` | Display name |
| `description` | `string` | How to earn this achievement |
| `iconUrl` | `string` | Badge icon URL |
| `category` | `'milestone' \| 'skill' \| 'social' \| 'event'` | Achievement type |
| `rarity` | `'common' \| 'uncommon' \| 'rare' \| 'epic' \| 'legendary'` | How rare this achievement is |
| `xpReward` | `number` | XP granted on unlock |

### Replay

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string (uuid)` | Unique replay identifier |
| `matchId` | `string (uuid)` | Associated match |
| `gameId` | `string` | Which game was played |
| `moveLog` | `object[]` | Array of timestamped moves |
| `format` | `string` | Replay format version |
| `sizeBytes` | `number` | Replay file size |
| `storageUrl` | `string` | Cloud storage URL |
| `viewCount` | `number` | Number of times viewed |
| `createdAt` | `string (ISO 8601)` | When the replay was stored |

### FriendRequest

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string (uuid)` | Request identifier |
| `fromPlayerId` | `string (uuid)` | Sender |
| `toPlayerId` | `string (uuid)` | Recipient |
| `status` | `'pending' \| 'accepted' \| 'rejected'` | Request state |
| `sentAt` | `string (ISO 8601)` | When the request was sent |
| `respondedAt` | `string (ISO 8601)?` | When the recipient responded |

## Deployment

### Docker (Recommended)

```bash
# Build the production image
docker build -t rankings-api .

# Run with environment variables
docker run -d \
  --name rankings-api \
  -p 3000:3000 \
  -e NODE_ENV=production \
  -e DATABASE_URL=postgresql://user:pass@db:5432/rankings \
  -e REDIS_URL=redis://redis:6379 \
  -e JWT_SECRET=your-jwt-secret \
  -e BILLING_API_URL=http://billing-api:3000 \
  -e BILLING_API_KEY=internal-key \
  -e WS_ENABLED=true \
  rankings-api
```

### Health Checks

Configure your orchestrator (Docker Compose, Kubernetes, ECS) to use the built-in probes:

| Probe | Endpoint | Interval | Timeout | Failure Threshold |
| ----- | -------- | -------- | ------- | ----------------- |
| Liveness | `GET /health` | 30s | 5s | 3 |
| Readiness | `GET /ready` | 10s | 5s | 3 |

### Production Checklist

- [ ] Set `NODE_ENV=production`
- [ ] Provide all **required** environment variables (see [Environment Variables](#environment-variables))
- [ ] Configure database connection pooling (recommended: 10–20 connections)
- [ ] Configure Redis for leaderboard caching and WebSocket pub/sub
- [ ] Enable TLS termination at the load balancer / reverse proxy
- [ ] Set up database migrations before first deploy
- [ ] Configure log aggregation (Pino outputs structured JSON)
- [ ] Set up monitoring alerts on `/health` and `/ready`
- [ ] Enable CORS for specific origins (do not use `*` in production)
- [ ] Configure WebSocket upgrade path at the load balancer
- [ ] Set up replay storage bucket with lifecycle policies (auto-archive old replays)
- [ ] Configure season auto-rotation schedule

## Supported Clients

This API is consumed by the following applications:

| Client | Type | Description |
| ------ | ---- | ----------- |
| **All portfolio games** | Game Clients | Leaderboard display, match result submission, player profiles, achievements |
| **[💳 Billing API](https://github.com/scottdreinhart/billing-api)** | API (internal) | Entitlement checks for premium ranking features (custom badges, expanded history) |
| **[📺 Ads API](https://github.com/scottdreinhart/ads-api)** | API (internal) | Player engagement data for ad-targeting audience segments |

## Versioning

The API uses **URL-prefix versioning**:

```
https://api.example.com/v1/leaderboards
https://api.example.com/v1/matches
```

- All current endpoints are under `/v1/`
- Breaking changes will be introduced under `/v2/` with a deprecation notice on `/v1/`
- Non-breaking additions (new fields, new endpoints) are added to the current version
- Deprecated versions will be supported for a minimum of **6 months** after the successor is released
- The OpenAPI spec at `/docs/json` includes the version in its `info.version` field

## Remaining Work

### Core Functionality

- [ ] **Domain model implementation** — define entities, value objects, and business rules for players, matches, rankings, seasons, Elo ratings, streaks, and leaderboard snapshots
- [ ] **Database integration** — connect to PostgreSQL/SQLite via Drizzle ORM or Prisma
- [ ] **CRUD route scaffolding** — RESTful endpoints for all domain entities
- [ ] **Authentication** — JWT or API key verification middleware
- [ ] **Authorization** — role-based access control for admin vs. client operations

### Code Quality & Testing

- [ ] **Unit tests** — domain functions and use-case services
- [ ] **Integration tests** — route handlers with database fixtures
- [ ] **E2E tests** — full request lifecycle with Supertest or Fastify inject

### DevOps & Deployment

- [ ] **CI/CD pipeline** — GitHub Actions workflow for lint → test → build → deploy
- [ ] **Database migrations** — version-controlled schema changes
- [ ] **Environment configs** — staging, production, and preview environments
- [ ] **Monitoring** — health-check alerts, error tracking, APM integration

## Future Improvements

- [ ] **WebSocket live leaderboard** — stream real-time ranking changes to connected game clients as matches complete
- [ ] **Elo variant support** — configurable rating algorithms per game (Glicko-2, TrueSkill, custom K-factor curves)
- [ ] **Anti-cheat validation** — server-side move validation and replay verification to detect impossible game states
- [ ] **Global cross-game ranking** — composite score aggregating a player’s performance across all portfolio games
- [ ] **Seasonal rewards** — integrate with Billing API to auto-grant entitlements (themes, badges) based on season-end placement
- [ ] **Matchmaking service** — skill-based matchmaking queue that pairs players of similar Elo for competitive games
- [ ] **Historical snapshots** — nightly leaderboard snapshots for trend analysis and “this day last year” comparisons
- [ ] **Social graph** — friend lists, follow/unfollow, and friend-scoped leaderboards
- [ ] **Replay storage** — store move-by-move game replays referenced from match records for spectating and review
- [ ] **Push notifications** — notify players when they’re dethroned from King of the Hill or drop in rank

## Portfolio Services

Infrastructure services supporting the game portfolio:

| Service | Description |
| ------- | ----------- |
| **[💳 Game Billing](https://github.com/scottdreinhart/game-billing)** | Payment processing & subscription management (Admin App) |
| **[🎨 Theme Store](https://github.com/scottdreinhart/theme-store)** | DLC theme downloader & manager (Admin App) |
| **[📺 Ad Network](https://github.com/scottdreinhart/ad-network)** | Ad serving & revenue management (Admin App) |
| **[💳 Billing API](https://github.com/scottdreinhart/billing-api)** | Fastify API backend for billing |
| **[🎨 Themes API](https://github.com/scottdreinhart/themes-api)** | Fastify API backend for themes |
| **[📺 Ads API](https://github.com/scottdreinhart/ads-api)** | Fastify API backend for ads |

## Portfolio Games

All games in this portfolio share the same React + Vite + TypeScript + CLEAN architecture stack:

| Game | Description | Complexity |
| ---- | ----------- | ---------- |
| **[Tic-Tac-Toe](https://github.com/scottdreinhart/tictactoe)** | Classic 3×3 grid game with 4 AI difficulty levels and series mode | Baseline — the reference architecture |
| **[Shut the Box](https://github.com/scottdreinhart/shut-the-box)** | Roll dice, flip numbered tiles to match the total; lowest remaining sum wins | Similar — grid UI + dice logic |
| **[Mancala (Kalah)](https://github.com/scottdreinhart/mancala)** | Two-row pit-and-stones capture game; simple rules, satisfying chain moves | Slightly higher — seed-sowing animation |
| **[Connect Four](https://github.com/scottdreinhart/connect-four)** | Drop discs into a 7×6 grid; first to four in a row wins | Similar — larger grid, same win-check pattern |
| **[Simon Says](https://github.com/scottdreinhart/simon-says)** | Repeat a growing sequence of colors/sounds; memory challenge | Similar — leverages existing Web Audio API |
| **[Lights Out](https://github.com/scottdreinhart/lights-out)** | Toggle a 5×5 grid of lights; goal is to turn them all off | Similar — grid + toggle logic |
| **[Nim](https://github.com/scottdreinhart/nim)** | Players take turns removing objects from piles; last to take loses | Simpler — minimal UI, pure strategy |
| **[Hangman](https://github.com/scottdreinhart/hangman)** | Guess letters to reveal a hidden word before the stick figure completes | Similar — alphabet grid + SVG drawing |
| **[Memory / Concentration](https://github.com/scottdreinhart/memory-game)** | Flip cards to find matching pairs on a grid | Similar — grid + flip animation |
| **[2048](https://github.com/scottdreinhart/2048)** | Slide numbered tiles on a 4×4 grid; merge matching tiles to reach 2048 | Slightly higher — swipe input + merge logic |
| **[Reversi (Othello)](https://github.com/scottdreinhart/reversi)** | Place discs to flip opponent's pieces; most discs wins | Moderately higher — flip-chain logic + AI |
| **[Checkers](https://github.com/scottdreinhart/checkers)** | Classic diagonal-move capture board game | Higher — move validation + multi-jump |
| **[Battleship](https://github.com/scottdreinhart/battleship)** | Place ships on a grid, take turns guessing opponent locations | Moderately higher — two-board UI + ship placement |
| **[Snake](https://github.com/scottdreinhart/snake)** | Steer a growing snake to eat food without hitting walls or itself | Different — real-time game loop instead of turn-based |
| **[Monchola](https://github.com/scottdreinhart/monchola)** | Traditional dice/board race game with capture mechanics | Similar — dice roll + board path + capture rules |
| **[Rock Paper Scissors](https://github.com/scottdreinhart/rock-paper-scissors)** | Best-of-N rounds against the CPU with hand animations | Simpler — minimal state, animation-focused |
| **[Minesweeper](https://github.com/scottdreinhart/minesweeper)** | Reveal cells on a minefield grid without detonating hidden mines | Moderately higher — flood-fill reveal + flag logic |

## Contributing

This is proprietary software. Contributions are accepted by invitation only.

If you have been granted contributor access:

1. Create a feature branch from `main`
2. Make focused, single-purpose commits with clear messages
3. Run `pnpm validate` before pushing (lint + format + build gate)
4. Submit a pull request with a description of the change

See the [LICENSE](LICENSE) file for usage restrictions.

## License

Copyright © 2026 Scott Reinhart. All Rights Reserved.

This project is proprietary software. No permission is granted to use, copy, modify, or distribute this software without the prior written consent of the owner. See the [LICENSE](LICENSE) file for full terms.

---
[⬆ Back to top](#-rankings-api)
