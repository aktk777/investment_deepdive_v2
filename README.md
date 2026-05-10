# 銘柄ディープダイブシステム

> **Claude Code drop-in for systematic equity investment analysis.**
> 14 specialist sub-agents coordinated through a file-based pipeline. Information acquisition runs entirely on `WebSearch` + `WebFetch` — no API keys, nothing to install. Designed to work cleanly inside the **remote Claude Code sandbox** where direct outbound HTTP is blocked.

## TL;DR — Run It

```bash
git clone https://github.com/{your-username}/deep-dive-system.git
cd deep-dive-system
claude
```

In the Claude Code session:

```
/deep-dive 6498        # KITZ
/deep-dive 7203        # Toyota
/deep-dive NVDA        # NVIDIA
```

That's the entire setup. No `pip install`, no API keys, no environment variables.

Works locally and on remote / cloud Claude Code instances identically — because all information acquisition uses `WebSearch` and `WebFetch`, which run via Anthropic's backend rather than your local network.

## Why this design

Earlier iterations used local Python scripts that called `requests.get()` against company IR pages. In Anthropic's remote Claude Code sandbox an egress proxy with a tight allowlist returns 403 to every external host (Google included), so those scripts silently failed and the agents fell back to AI-knowledge hallucinations.

This version replaces all `requests.get()` paths with **`WebSearch` + `WebFetch`**, which:
- Run via Anthropic's backend (the egress proxy doesn't apply)
- Work identically in remote sandboxes and locally
- Require no keys, no setup
- Handle PDF text extraction automatically — `WebFetch` against a PDF URL returns extracted text directly

**Trade-off**: PDF text extraction quality is whatever Anthropic's extractor produces. Good for narrative content and most numerical statements; less reliable for dense financial tables. The reliability-auditor and quality-auditor are tuned to catch the cases where this matters.

## Architecture

```
                       ┌─────────────────────┐
USER ──▶ /deep-dive ──▶│   master-investor   │
                       └──────────┬──────────┘
                                  │
        ┌─────────────────────────┼─────────────────────────┐
        ▼                         ▼                         ▼
┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
│ ① 情報収集部       │    │ ② 情報最適化部     │    │ ③ 分析部 (6課並列) │
│ (parallel,        │    │                   │    │                   │
│  WebSearch +      │    │ ・md-converter    │    │ ・overview/value  │
│  WebFetch only)   │    │   (validate +     │    │ ・financial       │
│                   │    │    index)         │    │ ・strategy/comp   │
│ ・primary-info    │───▶│ ・reliability-    │───▶│ ・governance      │
│ ・competitor-info │    │   auditor *       │    │ ・technical       │
│ ・macro-info      │    │   (independent)   │    │ ・macro            │
└──────────────────┘    └──────────────────┘    └─────────┬─────────┘
                                                            │
                                                            ▼
                                                  ┌──────────────────┐
                                                  │ ④ 投資判断部       │
                                                  │ ・ringi-writer    │
                                                  │ ・quality-auditor*│
                                                  └─────────┬────────┘
                                                            ▼
                                                       USER 稟議書

* = independent / adversarial gate
```

## Repository Layout

```
deep-dive-system/
├── README.md                       ← you are here
├── CLAUDE.md                       ← project context (auto-loaded by Claude Code)
├── LICENSE                         ← MIT
├── .gitignore                      ← excludes workspace/
├── .claude/
│   ├── agents/                     ← 14 sub-agent definitions
│   └── commands/
│       └── deep-dive.md            ← /deep-dive slash command
├── references/                     ← 株屋投資哲学 (analytical philosophy)
│   ├── phase1_overview.md ... phase6_macro.md
│   └── report_synthesis.md
├── assets/
│   └── megatrend_reference.json    ← curated thematic trends
└── workspace/                      ← .gitignored, auto-created per run
```

## Sub-Agents (14 total)

| Department | Agent | Role |
|-----------|-------|------|
| Orchestrator | `master-investor` | Coordinates the pipeline |
| 情報収集部 | `primary-info-collector` | IR / 短信 / 有報 / governance via WebSearch+WebFetch |
| 情報収集部 | `competitor-info-collector` | 3 competitors, IR pages |
| 情報収集部 | `macro-info-collector` | Rates / GDP / sector flows / themes |
| 情報最適化部 | `md-converter` | Validates frontmatter + builds INDEX |
| 情報最適化部 | `reliability-auditor` ⚖️ | Independent audit |
| 分析部 | `overview-valuation-analyst` | Phase 1 |
| 分析部 | `financial-analyst` | Phase 2 |
| 分析部 | `strategy-competition-analyst` | Phase 3 |
| 分析部 | `governance-analyst` | Phase 4 |
| 分析部 | `technical-analyst` | Phase 5 |
| 分析部 | `macro-analyst` | Phase 6 |
| 投資判断部 | `ringi-writer` | Synthesis + gap-check |
| 投資判断部 | `quality-auditor` ⚖️ | Independent final audit |

## Output

The final 稟議書 (per `references/report_synthesis.md`):

- **エグゼクティブサマリー**
- **総合評価テーブル** — 7-axis ★ scoring
- **ブル / ベース / ベアケース**
- **注目カタリスト** — what + when
- **要追加調査事項**
- **分析プロセスの透明性** — including any internal remands
- **ソース一覧**
- **Disclaimer**

Default language: **Japanese**.

## Limitations

- `WebFetch` against a PDF returns Anthropic's text extraction. It's good but heavy financial tables may lose some structure. The reliability-auditor flags this; the quality-auditor surfaces it if anything important got lost.
- TDnet's public archive only retains ~31 days. For older 適時開示 the system uses the company's IR archives.
- The system does NOT provide explicit buy/sell calls — by design, it produces decision material for the user to act on.
- Real-time chart data (一目均衡表 etc.) cannot be web-searched — `technical-analyst` provides user-verification checkpoints rather than pretending.
- Each deep-dive consumes meaningful tokens (14 agents, parallel where possible). Plan accordingly.

## Provenance

Built on the analytical philosophy of the **investment-analysis-en** skill. The `references/` folder and `assets/megatrend_reference.json` are direct carryovers — preserved verbatim per the design principle of separating philosophy (refs) from execution machinery.

## Disclaimer

このシステムが生成するレポートはAIによる一次分析です。投資判断は必ずご自身の責任で行ってください。

The reports produced by this system are AI-generated first-pass analyses. Final investment decisions are your own responsibility.

## License

MIT — see [LICENSE](./LICENSE).
