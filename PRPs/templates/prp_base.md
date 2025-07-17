name: "Base PRP Template v3 – Context-Rich, EARS-Aligned, with Validation Loops"
description: |

## Purpose
A template optimised for AI agents to ship features in one pass by supplying:
* Comprehensive context (docs, examples, code patterns, pitfalls)
* EARS-formatted requirements for clarity and testability
* Executable validation loops so the agent can self-correct

## Core Principles
1. Context is King – include *all* docs, code refs, and caveats.
2. Unambiguous Requirements – write every requirement with the **EARS** pattern set.
3. Validation Loops – provide tests & lints the AI can run and fix.
4. Information Density – surface keywords and patterns from the codebase.
5. Progressive Success – deliver an MVP, validate, then enhance.
6. Global Rules – follow every rule in CLAUDE.md.

---

## Goal
[Describe precisely what must exist when “done.”]

## Why
- [Business value & user impact]  
- [Integration points / dependencies]  
- [Problems solved and beneficiaries]

## What
Describe user-visible behaviour **and** technical requirements.

### EARS Requirements Table
List *every* discrete behaviour here; one row per requirement.

| ID        | Pattern (EARS)     | Requirement (EARS syntax)                                                                  | Rationale / Linkbacks |
|-----------|--------------------|--------------------------------------------------------------------------------------------|-----------------------|
| REQ-U-1   | Ubiquitous         | `<SystemName> shall <response>.`                                                           | …                     |
| REQ-E-1   | Event-Driven       | `When <trigger>, the <SystemName> shall <response>.`                                       | …                     |
| REQ-UWB-1 | Unwanted Behaviour | `If <undesired condition>, the <SystemName> shall <response>.`                             | …                     |
| REQ-S-1   | State-Driven       | `While <state>, the <SystemName> shall <response>.`                                        | …                     |
| REQ-O-1   | Optional           | `Where <feature> is present, the <SystemName> shall <response>.`                           | …                     |
| REQ-C-1   | Complex            | `When <trigger> and while <state>, if <condition> then the <SystemName> shall <response>.` | …                     |

*(Rewrite vague statements into clear EARS form before continuing.)*

### Success Criteria
- [ ] Measurable outcome 1  
- [ ] Measurable outcome 2  

---

## All Needed Context

### Documentation & References
```yaml
# Include **every** resource the AI must ingest
- url: https://api.example.com/docs
  why: Core endpoints used in REQ-E-1

- file: src/example_usage.py
  why: Shows pattern for async error handling (mirrors REQ-UWB-1)

- doc: https://pydantic.dev/usage/types
  section: "Custom data types"
  critical: Explains serialization caveat that often breaks tests

### Current Codebase tree (run `tree` in the root of the project) to get an overview of the codebase
```bash

```

### Desired Codebase tree with files to be added and responsibility of file
```bash

```

### Known Gotchas of our codebase & Library Quirks
```python
# CRITICAL: [Library name] requires [specific setup]
# Example: FastAPI requires async functions for endpoints
# Example: This ORM doesn't support batch inserts over 1000 records
# Example: We use pydantic v2 and  
```

## Implementation Blueprint

### Data models and structure

Create the core data models, we ensure type safety and consistency.
```python
# Outline only – keep code light; focus on shape & relationships
class ResearchResult(BaseModel):
    query: str
    top_hits: list[SearchHit]

```

### list of tasks to be completed to fullfill the PRP in the order they should be completed

```yaml
Task 1:
  MODIFY src/core/router.py
  - INSERT new route after pattern "router = APIRouter()"
  - FOLLOW pattern from src/core/health.py

Task 2:
  CREATE src/agents/research_agent.py
  - MIRROR agent scaffold in src/agents/base_agent.py
  - IMPLEMENT search() fulfilling REQ-E-1
...
```


### Per task pseudocode as needed added to each task
```python
# Task 2 – research_agent.search
async def search(self, query: str) -> ResearchResult:
    # VALIDATE input (<= 256 chars)  # REQ-UWB-1
    # CALL brave_api.query(query)    # asynchronous
    # HANDLE HTTP errors / rate-limits
    # RETURN ResearchResult(model)
```

### Integration Points
```yaml
DATABASE:
  - migration: "ALTER TABLE users ADD COLUMN feature_enabled BOOLEAN DEFAULT FALSE"

CONFIG:
  - settings.py → FEATURE_TIMEOUT = int(os.getenv("FEATURE_TIMEOUT", "30"))

ROUTES:
  - src/api/routes.py → include_router(research_router, prefix="/research")
```

## Validation Loop

### Level 1: Syntax & Style
```bash
# Run these FIRST - fix any errors before proceeding
ruff check src/new_feature.py --fix  # Auto-fix what's possible
mypy src/new_feature.py              # Type checking

# Expected: No errors. If errors, READ the error and fix.
```

### Level 2: Unit Tests each new feature/file/function use existing test patterns
```python
def test_event_search_happy_path():  # REQ-E-1
    ...

def test_unwanted_beh_rate_limit():  # REQ-UWB-1
    ...
```

```bash
# Run and iterate until passing:
uv run pytest test_new_feature.py -v
# If failing: Read error, understand root cause, fix code, re-run (never mock to pass)
```

### Level 3: Integration Test
```bash
# Start the service
uv run python -m src.main --dev

# Test the endpoint
curl -X POST http://localhost:8000/feature \
  -H "Content-Type: application/json" \
  -d '{"param": "test_value"}'

# Expected: {"status": "success", "data": {...}}
# If error: Check logs at logs/app.log for stack trace
```

## Final validation Checklist
- [ ] All EARS requirements satisfied & traceable in code/tests
- [ ] All tests pass: `uv run pytest tests/ -v`
- [ ] No linting errors: `uv run ruff check src/`
- [ ] No type errors: `uv run mypy src/`
- [ ] Manual test successful: [specific curl/command]
- [ ] Error cases handled gracefully
- [ ] Logs are informative but not verbose
- [ ] README / docs updated

---

## Anti-Patterns to Avoid
- ❌ Don't create new patterns when existing ones work
- ❌ Don't skip validation because "it should work"  
- ❌ Don't ignore failing tests - fix them
- ❌ Don't use sync functions in async context
- ❌ Don't hardcode values that should be config
- ❌ Don't catch all exceptions - be specific
