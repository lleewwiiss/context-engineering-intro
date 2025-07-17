# Execute BASE PRP – EARS-aligned Edition

## PRP File: $ARGUMENTS
Implement the feature described in the specified PRP.  
**Every requirement in the PRP is expressed in EARS format**; treat each row in the EARS table as an acceptance criterion you must satisfy.

---

## Execution Process

1. **Load PRP**  
   - Read the PRP file in full.  
   - Parse the **EARS Requirements** table; note each `ID`, `Pattern`, and expected behaviour.  
   - Review all *Critical Context*, *Implementation Blueprint*, and *Validation Gates*.  
   - Extend research (web or codebase) **only if** additional clarity is needed to implement an EARS requirement unambiguously.

2. **ULTRATHINK**  
   - Think deeply before writing code.  
   - Map each EARS `ID` to one or more concrete implementation tasks.  
   - Break complex tasks into smaller steps.  
   - Use your Todos toolset (`TodoWrite`, etc.) to create and track this execution plan, tagging tasks with the corresponding EARS IDs (e.g., `REQ-E-1`).  
   - Identify existing patterns in the repo to mirror—you must follow them unless the PRP explicitly diverges.

3. **Execute the Plan**  
   - Implement code exactly as outlined in your task list.  
   - Cross-reference code against the EARS specs to ensure each trigger, state, optional path, and unwanted-behaviour handler is covered.  
   - Keep commits and PR comments traceable back to EARS IDs.

4. **Validate**  
   - Run the **Validation Gates** from the PRP.  
   - If tests fail, locate which EARS requirement is unmet; refactor accordingly.  
   - Iterate until **all gates pass**.

5. **Complete**  
   - Re-read the PRP. Confirm every checklist item is satisfied and every EARS requirement has corresponding logic & tests.  
   - Run the full validation suite one final time; ensure green.  
   - Report status, citing the highest-level EARS IDs covered (e.g., “All REQ-* rows satisfied”).

6. **Reference the PRP Anytime**  
   - Keep the PRP open; jump back to clarify triggers, states, or optional paths as you code.

> **If any validation fails:**  
>  • Use failure output to identify the relevant EARS requirement.  
>  • Fix, rerun, repeat until success.

---

## Completion Checklist
- [ ] All EARS requirements implemented and traceable in code/tests.  
- [ ] Validation gates (`ruff`, `mypy`, `pytest`, etc.) pass without manual intervention.  
- [ ] Implementation follows existing repo conventions unless PRP overrides.  
- [ ] Unwanted-behaviour handlers tested (negative paths).  
- [ ] Optional features gated correctly (`feature flag`, compile-time check, etc.).  
- [ ] Code comments or PR description link back to EARS IDs for future maintainers.

> **Final Reminder:** EARS rows are your single source of truth for “done.”  
>  Confirm *every* pattern (Ubiquitous, Event-Driven, Unwanted Behaviour, State-Driven, Optional, Complex) is respected in both implementation and tests before calling it complete.
