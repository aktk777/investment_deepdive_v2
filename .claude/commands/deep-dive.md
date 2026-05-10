---
description: Run a full multi-agent deep-dive investment analysis on a ticker. Coordinates 13 specialist sub-agents (information collection → optimization → 6-phase analysis → synthesis → quality audit) and produces a final 稟議書 report.
argument-hint: <ticker> (e.g., 6498, 7203, NVDA, AAPL)
---

You are now in **銘柄ディープダイブシステム** mode.

Target: **$ARGUMENTS**

Use the `master-investor` sub-agent (via the Task tool) to orchestrate the full pipeline. The master-investor will:

1. Validate the ticker and confirm market (JP / US)
2. Set up the workspace (`workspace/{ticker}_{timestamp}/`)
3. Run information collection (3 collectors in parallel)
4. Run MD conversion + independent reliability audit
5. Run 6-phase analysis in parallel (each analyst reads its own `references/phaseN_*.md`)
6. Run synthesis (with gap-check, max 2 remands)
7. Run final quality audit (max 1 remand)
8. Output the final 稟議書 to the user

While the pipeline runs, only emit brief stage-transition status lines. The full 稟議書 is the deliverable — no intermediate analysis output.

After the master-investor returns, present the final 稟議書 verbatim to the user, prefixed with a one-paragraph executive summary noting:
- Overall score (★__/5.0)
- Whether any internal remands occurred
- Whether information constraints exist

If `$ARGUMENTS` is empty or ambiguous, ask the user for the ticker once before proceeding.
