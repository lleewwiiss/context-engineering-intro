# Create PRP – EARS-aligned Edition

## Feature file: $ARGUMENTS
Generate a complete PRP for the requested feature **using the EARS template to express every requirement**.  
Read the feature file first; understand what must be built, how the examples guide you, and any other nuances.

The downstream AI agent only sees what you append to this PRP plus the training data.  
Assume it has repo access and the same knowledge cut-off as you, so every insight you dig up **must** be included or explicitly linked (URLs).

---

## Research Process

1. **Codebase Analysis**  
   - Scan for similar features / patterns.  
   - Record file paths and snippets you will cite in the PRP.  
   - Note conventions (naming, error handling, tests).

2. **External Research**  
   - Find comparable implementations, library docs, best-practice write-ups.  
   - Capture canonical URLs (RFC-like links, GitHub gists, blog posts, Stack Overflow answers).  
   - List pitfalls and version quirks.

3. **User Clarification** *(only if necessary)*  
   - Ask about missing triggers, states, or optional behaviours that block writing unambiguous EARS requirements.

---

## PRP Generation  *(use `PRPs/templates/prp_base.md` as the scaffold)*

### 1 – EARS Requirements
For **each** discrete behaviour, supply an EARS-formatted requirement and label its pattern:

| ID       | Pattern           | Requirement (EARS syntax)                                        | Rationale / Linkbacks |
|----------|-------------------|------------------------------------------------------------------|-----------------------|
| REQ-U-1  | Ubiquitous        | `<SystemName> shall <system-response>.`                          | (always true)         |
| REQ-E-1  | Event-Driven      | `When <trigger>, the <SystemName> shall <response>.`             | …                     |
| REQ-UWB-1| Unwanted Behaviour| `If <undesired condition>, the <SystemName> shall <response>.`   | …                     |
| REQ-S-1  | State-Driven      | `While <state>, the <SystemName> shall <response>.`              | …                     |
| REQ-O-1  | Optional          | `Where <feature> is present, the <SystemName> shall <response>.` | …                     |
| REQ-C-1  | Complex           | `When <trigger> and while <state>, if <condition> …`             | …                     |

*Rewrite or split any vague requirement from the feature file into clear EARS form before continuing.*

### 2 – Critical Context for the AI Agent
- **Documentation URLs** – deep links to API sections, standards.  
- **Code Examples** – real snippets (with path and line refs).  
- **Gotchas** – version incompatibilities, library quirks.  
- **Patterns** – existing repo approaches to mimic or extend.

### 3 – Implementation Blueprint
1. Pseudocode / algorithmic outline.  
2. File-by-file plan referencing existing modules.  
3. Error-handling & logging strategy.  
4. **Task List** – ordered, check-box style.

### 4 – Validation Gates *(must be executable)*
```bash
# Syntax & style
ruff check --fix && mypy .

# Unit tests
uv run pytest tests/ -v

*** CRITICAL AFTER YOU ARE DONE RESEARCHING AND EXPLORING THE CODEBASE BEFORE YOU START WRITING THE PRP ***

*** ULTRATHINK ABOUT THE PRP AND PLAN YOUR APPROACH THEN START WRITING THE PRP ***

## Output
Save as: `PRPs/{feature-name}.md`

## Quality Checklist
- [ ] All EARS requirements present, labelled, and unambiguous.
- [ ] Necessary context (docs, snippets, gotchas) included.
- [ ] Validation gates run cleanly from repo root.
- [ ] References to existing patterns / conventions.
- [ ] Clear, linear implementation path.
- [ ] Error handling documented.

Score the PRP on a scale of 1-10 (confidence level to succeed in one-pass implementation using claude codes)

Remember: The goal is one-pass implementation success through comprehensive context. Think first, write second.
