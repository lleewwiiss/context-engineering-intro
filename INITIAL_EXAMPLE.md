## FEATURE

Build a **Pydantic-based “Research-and-Email” AI system** with a nested-agent design:

- **Primary agent** – *Research Agent*
    - Uses Brave Search API to gather information.
    - Exposes search results and citations to sub-agents and CLI users.

- **Sub-agent (tool)** – *Email Draft Agent*
    - Consumes the Research Agent as a tool.
    - Generates a polished email draft summarising findings.
    - Sends (or saves) the draft via Gmail API.

- **CLI interface** (`ai-assistant`) to orchestrate both agents.

---

### EARS Requirements Table

| ID        | Pattern           | Requirement (EARS syntax)                                                                                                                                                                                                                                                    | Rationale / Linkbacks    |
|-----------|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| REQ-U-1   | **Ubiquitous**    | The **CLI application** shall be written in Python ≥ 3.11 and use `typer` for command routing.                                                                                                                                                                               | Company CLI standard.    |
| REQ-E-1   | **Event-Driven**  | **When** a user issues `ai-assistant search "<query>"`, **the Research Agent** shall fetch the top 10 Brave hits and return structured JSON results.                                                                                                                         | Core user workflow.      |
| REQ-UWB-1 | **Unwanted Beh.** | **If** the Brave API returns an error or empty set, **the Research Agent** shall raise a `ResearchError` and log the failing URL & status code.                                                                                                                              | Robust failure handling. |
| REQ-S-1   | **State-Driven**  | **While** offline (no network) **the system** shall queue the search request locally and retry automatically when connectivity is restored.                                                                                                                                  | Offline-first UX.        |
| REQ-O-1   | **Optional**      | **Where** a `GMAIL_SEND=true` env flag is present, **the Email Draft Agent** shall send the drafted email via Gmail API; otherwise it shall output the draft to stdout only.                                                                                                 | Feature flag.            |
| REQ-C-1   | **Complex**       | **When** the command `ai-assistant send-report "<query>" "<recipient>"` is executed, **and** research results are available **but** Gmail auth fails, **then** the Email Draft Agent shall save the draft to `./drafts/<timestamp>.md` **and** surface a warning in the CLI. | Multi-condition flow.    |

*(Rewrite or split any vague requirement into clear EARS form before development.)*

---

## EXAMPLES

The `examples/` directory illustrates best practices (do **not** copy verbatim):

- `examples/cli.py` — Typer-based CLI skeleton; shows option parsing and async orchestration.
- `examples/agent/` — Reference Pydantic agent definitions supporting multiple LLM providers, dependency injection, and tool composition.
- See `examples/README.md` for folder conventions and how to document future examples.

---

## DOCUMENTATION

- **Pydantic AI** — <https://ai.pydantic.dev/> -– framework architecture, agent patterns.
- **Brave Search API** — <https://api.search.brave.com/app/documentation> — endpoints, rate limits (`REQ-E-1`).
- **Gmail API** — <https://developers.google.com/gmail/api> — OAuth setup, send-message mutation (`REQ-O-1`).
- **Typer** CLI — <https://typer.tiangolo.com/> — command structure guidance (`REQ-U-1`).

---

## OTHER CONSIDERATIONS

- Provide `.env.example` with keys: `BRAVE_API_KEY`, `GMAIL_CLIENT_ID`, `GMAIL_CLIENT_SECRET`, `GMAIL_REFRESH_TOKEN`, `GMAIL_SEND`.
- In `README.md`, include **project tree** and **setup steps** (virtual-env is pre-installed).
- Load environment variables with `python_dotenv.load_dotenv()` at app start.
- Follow repo linting (`ruff`, `mypy`) and test conventions; place unit tests under `tests/`.
- Common AI-assistant pitfalls to avoid:
    - Forgetting async I/O when calling external APIs (causes CLI hangs).
    - Neglecting token-limit handling when sending long research summaries to LLMs.
    - Hard-coding Gmail “From” address instead of deriving from OAuth credentials.
