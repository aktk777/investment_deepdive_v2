# 銘柄ディープダイブシステム

A multi-agent Claude Code system for systematic equity investment analysis.
Information acquisition runs entirely on Claude Code's `WebSearch` and `WebFetch` tools — no API keys, no shell-based HTTP, nothing to install. Designed to work cleanly inside the **remote Claude Code sandbox** (where direct outbound HTTP from Bash is blocked by an egress allowlist).

## Quick Start

```bash
# In a Claude Code session at project root:
/deep-dive 6498        # KITZ
/deep-dive 7203        # Toyota
/deep-dive NVDA        # NVIDIA
```

That's it. No setup steps. The pipeline does its own information gathering.

## Architecture

A 4-stage, 14-agent pipeline coordinated by a master orchestrator. All information acquisition happens through Anthropic's `WebSearch` and `WebFetch` tools — which run via Anthropic's backend, bypassing the local egress proxy and requiring no keys.

```
                       ┌─────────────────────┐
USER ──▶ /deep-dive ──▶│   master-investor   │
                       │  (オーケストレーター)   │
                       └──────────┬──────────┘
                                  │
        ┌─────────────────────────┼─────────────────────────┐
        ▼                         ▼                         ▼
┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
│ ① 情報収集部       │    │ ② 情報最適化部     │    │ ③ 分析部 (6課並列) │
│ (parallel,        │    │                   │    │                   │
│  WebSearch +      │    │                   │    │                   │
│  WebFetch only)   │    │                   │    │                   │
│ ・primary-info    │───▶│ ・md-converter    │───▶│ ・overview/value  │
│ ・competitor-info │    │   (validate +     │    │ ・financial       │
│ ・macro-info      │    │    index)         │    │ ・strategy/comp   │
└────────┬─────────┘    │ ・reliability-    │    │ ・governance      │
         │              │   auditor *       │    │ ・technical       │
         │              │   (independent)   │    │ ・macro            │
         │              └──────────────────┘    └─────────┬─────────┘
         │                        │                        │
         │              [PASS] ─▶ next                     │
         │              [FAIL] ─▶ remand                   │
         │                                                  ▼
         │                                        ┌──────────────────┐
         │                                        │ ④ 投資判断部       │
         │                                        │ ・ringi-writer    │
         │                                        │ ・quality-auditor*│
         │                                        └─────────┬────────┘
         │                                                  ▼
         │                                             USER 稟議書
         │
         ▼
* = independent / adversarial gate
```

## Sub-Agents (14 total)

| Department | Agent | Role |
|-----------|-------|------|
| Orchestrator | `master-investor` | Coordinates the pipeline |
| 情報収集部 | `primary-info-collector` | Fetches IR / 短信 / 有報 / governance / mid-term plan via WebSearch+WebFetch |
| 情報収集部 | `competitor-info-collector` | Identifies 3 competitors, gathers their filings |
| 情報収集部 | `macro-info-collector` | Pulls rates / GDP / sector flows / theme exposure |
| 情報最適化部 | `md-converter` | Validates frontmatter + builds INDEX (no PDF re-parsing) |
| 情報最適化部 | `reliability-auditor` ⚖️ | Independent audit of information quality |
| 分析部 | `overview-valuation-analyst` | Phase 1 |
| 分析部 | `financial-analyst` | Phase 2 |
| 分析部 | `strategy-competition-analyst` | Phase 3 |
| 分析部 | `governance-analyst` | Phase 4 |
| 分析部 | `technical-analyst` | Phase 5 |
| 分析部 | `macro-analyst` | Phase 6 |
| 投資判断部 | `ringi-writer` | Synthesis + gap-check |
| 投資判断部 | `quality-auditor` ⚖️ | Independent final audit |

## Why WebSearch + WebFetch only

Earlier iterations of this system used local Python scripts that called `requests.get()` against company IR pages. In Anthropic's remote Claude Code sandbox an egress proxy with a tight allowlist returns 403 to every external host (Google included), so those scripts silently failed and the agents fell back to AI-knowledge hallucinations.

`WebSearch` and `WebFetch` go through Anthropic's backend instead of the local network — they bypass that egress proxy entirely. They work in remote sandboxes, work locally, require no API keys, and `WebFetch` against a PDF URL even returns the text already extracted (Anthropic's backend handles the parsing). So a single retrieval tool covers what previously needed three layers (HTTP client + PDF parser + key management).

The trade-off is that PDF text extraction quality is whatever Anthropic's extractor produces — good for narrative + most numerical statements, less reliable for dense financial tables. The reliability auditor (Stage 2) and quality auditor (Stage 5) are tuned to catch the cases where this matters.

## File-Based Pipeline

Sub-agents communicate via the workspace filesystem:

```
workspace/{ticker}_{timestamp}/
├── manifest.json                        # source registry
├── md/                                  # all retrieved content
│   ├── INDEX.md                         # human-readable index
│   ├── md_index.json                    # machine-readable index
│   ├── validation_issues.json           # md-converter findings
│   ├── gap_report.md                    # collector-reported gaps
│   ├── yuho_FY2024_*.md                 # target company docs
│   ├── tanshin_2025Q3_*.md
│   ├── setsumei_2025Q3_*.md
│   ├── ...
│   ├── competitors/{slug}/*.md          # per-competitor docs
│   ├── industry/*.md                    # industry data
│   └── macro/*.md                       # macro context
├── audit/
│   ├── reliability_report.md
│   └── quality_audit_report.md
├── analysis/
│   └── phase1_summary.md ... phase6_summary.md
├── ringi_draft.md
└── reports/
    └── ringi_{ticker}_{date}.md         # final delivered report
```

There is **no `raw/` directory** — collectors save WebFetch'd text directly as `.md` because no separate parsing step is needed.

`workspace/` is `.gitignored`.

## Investment Philosophy (株屋投資哲学)

The analytical perspectives in `references/` are **canonical philosophy files** carried over from the parent investment-analysis-en skill. **Analyst agents must read their corresponding reference and follow it strictly.**

```
references/
├── phase1_overview.md
├── phase2_financials.md
├── phase3_strategy.md
├── phase4_governance_external.md
├── phase5_technical.md
├── phase6_macro.md
└── report_synthesis.md
```

```
assets/
└── megatrend_reference.json
```

## Quality Gates & Loop Limits

Two **independent adversarial gates**:

1. **reliability-auditor** (after MD optimization) — refuses to let analysts work on bad data
2. **quality-auditor** (after synthesis) — refuses to let bad reports reach the user

Loop limits:
- Reliability remand: max 1 cycle
- Synthesis (Layer 2 → Layer 1) remand: max 2 cycles
- Quality (Layer 3 → Layer 1/2) remand: max 1 cycle

When limits are reached, unresolved issues surface as **要追加調査事項** in the report.

## Output Defaults

- **Language**: Japanese (English on user request)
- **Format**: Per `references/report_synthesis.md`
- **No buy/sell recommendation** — analysis material only
- **Disclaimer always included** at the end

## Notes for Claude (when running in this project)

- When the user invokes `/deep-dive <ticker>`, dispatch `master-investor` and let it orchestrate.
- For light queries ("what's the PER of Toyota right now?"), a quick search is fine — don't over-engineer.
- For substantive questions ("should I buy Toyota?", "analyze 7203"), recommend or trigger the full deep-dive.
- **Information acquisition is via `WebSearch` + `WebFetch` only.** Don't call `curl`, `wget`, or any other shell-based HTTP client (they're blocked by the sandbox anyway).
- **Never read files outside the project tree** (`~/.ssh/`, `~/.aws/`, `.env`, etc.).
- **Never output environment variables.**
- **Never write outside `$WORKSPACE/`.**
- If `WebFetch` against a URL fails, **record the gap honestly** in `gap_report.md`. Do NOT fall back to AI knowledge of the company — that's the failure mode this rebuild exists to prevent.
