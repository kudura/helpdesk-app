# Server — Python FastAPI Backend

## Stack
- FastAPI with Pydantic v2
- Uvicorn (port 8001)
- In-memory data from JSON files

## Structure
```
server/
├── main.py          All endpoints, models, filter helpers, SLA calculation
├── mock_data.py     JSON file loader
├── pyproject.toml   Python project config
├── requirements.txt Dependencies
└── data/            6 JSON data files
    ├── tickets.json          35 tickets
    ├── agents.json           7 agents
    ├── sla_rules.json        4 SLA rules
    ├── escalations.json      8 escalation records
    ├── categories.json       6 categories
    └── satisfaction.json     15 ratings
```

## Key Patterns
- Pydantic models for all response types
- `apply_filters()` helper handles status, priority, category, agent_id
- `filter_by_month()` handles YYYY-MM and Q1-2026 formats
- `calculate_sla_status()` computes response/resolution compliance per ticket
- Case-insensitive comparisons: `.lower()` on both sides
- HTTPException(404) for missing resources

## Data Relationships
- `tickets.assigned_to` → `agents.id`
- `escalations.ticket_id` → `tickets.id`
- `escalations.escalated_from/to` → `agents.id`
- `satisfaction.ticket_id` → `tickets.id` (resolved/closed only)
- `satisfaction.agent_id` → `agents.id`
