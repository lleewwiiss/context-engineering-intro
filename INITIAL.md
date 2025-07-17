## FEATURE

Briefly describe the feature’s purpose and scope.

### EARS Requirements Table  
Fill in one row per discrete behaviour. Use the **EARS** syntax and label the pattern:

| ID       | Pattern           | Requirement (EARS syntax)                                        | Rationale / Linkbacks |
|----------|-------------------|------------------------------------------------------------------|-----------------------|
| REQ-U-1  | Ubiquitous        | `<SystemName> shall <response>.`                                 | (always true)         |
| REQ-E-1  | Event-Driven      | `When <trigger>, the <SystemName> shall <response>.`             | …                     |
| REQ-UWB-1| Unwanted Behaviour| `If <undesired condition>, the <SystemName> shall <response>.`   | …                     |
| REQ-S-1  | State-Driven      | `While <state>, the <SystemName> shall <response>.`              | …                     |
| REQ-O-1  | Optional          | `Where <feature> is present, the <SystemName> shall <response>.` | …                     |
| REQ-C-1  | Complex           | `When <trigger> and while <state>, if <condition> …`             | …                     |

*Rewrite any vague requirement into clear EARS form before continuing.*

---

## EXAMPLES

Provide and explain runnable or illustrative examples from your `examples/` folder.  
Link each example to the EARS IDs it demonstrates.

---

## DOCUMENTATION

List documentation that must be referenced during development:

- `[RFC 9110 – HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)` — relevant for request handling (`REQ-E-1`).  
- `path/to/internal/README.md` — coding conventions for error handling (`REQ-UWB-1`).  
- …

---

## OTHER CONSIDERATIONS

- _Gotchas_ the AI often misses (edge-case date parsing, locale issues, etc.).  
- Performance or security constraints (tie them to EARS IDs where possible).  
- Feature flags or deployment notes (map to **Optional** pattern rows).  
