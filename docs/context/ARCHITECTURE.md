# Architecture

<!-- Last pruned: [date] -->
<!-- Lines: ~150 target, 200 max -->
<!-- Relevance: All structural tasks -->

## System Overview

[High-level description - 2-3 sentences max]

```
┌─────────────────────────────────────────────────────┐
│                    [Your App]                       │
├─────────────────────────────────────────────────────┤
│  Frontend          │  Backend          │  Data     │
│  [tech]            │  [tech]           │  [tech]   │
└─────────────────────────────────────────────────────┘
```

## Tech Stack

| Layer    | Technology  | Notes        |
| -------- | ----------- | ------------ |
| Frontend | [framework] | [key detail] |
| Backend  | [framework] | [key detail] |
| Database | [type]      | [key detail] |
| Auth     | [approach]  | [key detail] |
| Hosting  | [platform]  | [key detail] |

## Project Structure

```
src/
├── components/     # UI components
├── pages/          # Routes
├── api/            # API handlers
├── lib/            # Shared utilities
├── types/          # TypeScript types
└── styles/         # CSS
```

## External Dependencies

| Service         | Purpose      | Setup Issue | Status |
| --------------- | ------------ | ----------- | ------ |
| [Database]      | Data storage | #[number]   | ⬜/✅  |
| [Auth provider] | User auth    | #[number]   | ⬜/✅  |
| [API service]   | [purpose]    | #[number]   | ⬜/✅  |

## Key Patterns

### [Pattern 1 Name]

[1-2 sentences] → See `src/[path]` for example

### [Pattern 2 Name]

[1-2 sentences] → See `src/[path]` for example

## API Structure

- `GET /api/[resource]` - List
- `POST /api/[resource]` - Create
- `GET /api/[resource]/:id` - Read
- `PUT /api/[resource]/:id` - Update
- `DELETE /api/[resource]/:id` - Delete

Error format: `{ error: string, code: number }`

## Environment Variables

| Variable       | Purpose       | Required |
| -------------- | ------------- | -------- |
| `DATABASE_URL` | DB connection | Yes      |
| `[OTHER]`      | [purpose]     | Yes/No   |

---

_Keep this file under 200 lines. Reference code files instead of including code._
