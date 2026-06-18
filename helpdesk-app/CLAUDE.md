# CLAUDE.md

IT Help Desk & Ticket Tracker — Full-stack demo application with Vue 3 frontend, Python FastAPI backend, and in-memory mock data (no database).

## Critical Tool Usage Rules

### Subagents
Use the Task tool with these specialized subagents for appropriate tasks:

- **vue-expert**: Use for Vue 3 frontend features, UI components, styling, and client-side functionality
  - **MANDATORY: ANY time you need to create or significantly modify a .vue file, you MUST delegate to vue-expert**
- **code-reviewer**: Use after writing significant code to review quality and best practices
- **Explore**: Use for understanding codebase structure, searching for patterns, or answering questions about how components work
- **general-purpose**: Use for complex multi-step tasks or when other agents don't fit

### Skills
- **backend-api-test** skill: Use when writing or modifying tests in `tests/backend` directory with pytest and FastAPI TestClient
- **saas-redesign** skill: Redesign the Vue 3 UI with an alternative color scheme or layout variation

### MCP Tools
- **ALWAYS use GitHub MCP tools** (`mcp__github__*`) for ALL GitHub operations
  - Exception: Local branches only — use `git checkout -b` instead of `mcp__github__create_branch`
- **ALWAYS use Playwright MCP tools** (`mcp__playwright__*`) for browser testing
  - Test against: `http://localhost:3000` (frontend), `http://localhost:8001` (API)

## Stack
- **Frontend**: Vue 3 + Composition API + Vite (port 3000)
- **Backend**: Python FastAPI (port 8001)
- **Data**: JSON files in `server/data/` loaded via `server/mock_data.py`

## Quick Start

```bash
# Backend
cd server
uv run python main.py

# Frontend
cd client
npm install && npm run dev
```

## Key Patterns

**Filter System**: 5 filters (Status, Priority, Category, Agent, Period) apply to ticket data via query params
**Data Flow**: Vue filters → `client/src/api.js` → FastAPI → In-memory filtering → Pydantic validation → Computed properties
**Reactivity**: Raw data in refs, derived data in computed properties
**SLA Calculation**: Real-time compliance check comparing ticket timestamps against priority-based SLA rules

## API Endpoints
- `GET /api/tickets` — Filters: status, priority, category, agent_id, month
- `GET /api/tickets/{id}` — Single ticket with SLA status
- `POST /api/tickets` — Create new ticket
- `GET /api/agents` — Filters: status, role
- `GET /api/agents/{id}` — Agent with workload stats
- `GET /api/escalations` — Filters: status, priority
- `GET /api/sla/compliance` — SLA compliance per priority
- `GET /api/sla/breaches` — Tickets that breached SLA
- `GET /api/dashboard/summary` — All filters supported
- `GET /api/reports/monthly-trends` — Monthly aggregations
- `GET /api/reports/agent-performance` — Per-agent metrics
- `GET /api/satisfaction/summary` — CSAT stats
- `GET /api/categories` — All categories

## Common Issues
1. Use unique keys in v-for (`ticket.id`, `agent.id` — never `index`)
2. Validate dates before `.getMonth()` calls
3. Update Pydantic models when changing JSON data structure
4. Filter comparisons are case-insensitive on the backend
5. SLA times are in minutes (15, 60, 240, 480, 1440, 2880)

## File Locations
- Views: `client/src/views/*.vue`
- Components: `client/src/components/*.vue`
- API Client: `client/src/api.js`
- Composables: `client/src/composables/*.js`
- Backend: `server/main.py`, `server/mock_data.py`
- Data: `server/data/*.json`
- Tests: `tests/backend/*.py`

## Design System
- Colors: Slate/gray (#0f172a, #64748b, #e2e8f0)
- Status badges: success/warning/danger/info/neutral
- Sidebar: Dark (#0f172a), 240px, collapsible
- Charts: Custom SVG donut, CSS bar charts
- No emojis in UI
