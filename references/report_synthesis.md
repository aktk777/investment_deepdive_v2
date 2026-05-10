# Final: Report Synthesis

## Objective
Integrate results from all 6 phases into a single report that directly supports investment decisions.
The Synthesizer's job is NOT to simply stack individual analyses — it's to find connections
between them, resolve contradictions, and build a coherent investment "story."

## Synthesis Process

### Step 1: Survey All Phase Summaries
Lay out Phase 1-6 summaries side by side. Get the full picture.

### Step 2: Cross-Check
Verify consistency and resolve contradictions:
- Does financial growth (Phase 2) align with market environment (Phase 3, 6)?
- Does valuation (Phase 1) match growth outlook (Phase 2, 3)?
- Do technicals (Phase 5) and fundamentals (Phase 1-3) point the same direction?
- If external evaluation (Phase 4) diverges from your analysis, what are you missing?

### Step 3: Overall Assessment
Revisit Phase 1's initial hypothesis and update it based on the full analysis.

Investment quality guideline (from the analysis framework):
> Ideal profile: PER ~10, high probability of compounding revenue growth,
> strong ROIC, and an identifiable catalyst for capital inflow.

This is an ideal — not every stock needs to meet it.
But always evaluate the stock on these 4 dimensions:
**Valuation × Growth × Capital Efficiency × Catalyst**

## Report Format

Output the report in **Japanese** by default (English if user specifies).

---

```markdown
# 投資分析レポート: [Company Name] ([Ticker])
**分析日**: [Date]
**株価**: ¥[Price] / 時価総額: [Market Cap]

---

## エグゼクティブサマリー
[3-5 sentences capturing the stock's investment appeal and risks.
Focus on the most important points only.]

## 総合評価

| 評価軸 | スコア (5段階) | コメント |
|--------|:---:|---------|
| バリュエーション | ★☆☆☆☆〜★★★★★ | [one-liner] |
| 成長性 | ★☆☆☆☆〜★★★★★ | [one-liner] |
| 収益性・資本効率 | ★☆☆☆☆〜★★★★★ | [one-liner] |
| 競合優位性 | ★☆☆☆☆〜★★★★★ | [one-liner] |
| ガバナンス | ★☆☆☆☆〜★★★★★ | [one-liner] |
| テクニカル | ★☆☆☆☆〜★★★★★ | [one-liner] |
| マクロ環境 | ★☆☆☆☆〜★★★★★ | [one-liner] |

**総合スコア**: ★☆☆☆☆〜★★★★★ ([numeric]/5.0)

## ブルケース（楽観シナリオ）
[Most optimistic scenario. What needs to go right for significant upside?]

## ベースケース（基本シナリオ）
[Most probable scenario.]

## ベアケース（悲観シナリオ）
[Maximum risk scenario. What could cause significant downside?]

## 注目カタリスト
- [Catalyst 1]: [Timing] — [Direction and magnitude of impact]
- [Catalyst 2]: [Timing] — [Direction and magnitude of impact]

## 要追加調査事項
Items this analysis could not fully cover:
- [Item 1]: [Reason + recommended action]
- [Item 2]: [Reason + recommended action]

## 分析プロセスの透明性
- 内部品質チェック: [No remands / Phase X re-investigated (Y times)]
- 情報制約: [Items where adequate data could not be obtained]

## ソース一覧
Key information sources used:
- [Source 1: URL or document name]
- [Source 2: URL or document name]
- ...

---
*本レポートはAIによる一次分析です。投資判断は自己責任でお願いします。*
```

## Output Guidelines

- Default language: **Japanese** (unless user specifies English)
- Always cite sources for specific numbers
- Do NOT make explicit "buy" / "sell" recommendations — present analysis material only
- Use hedging language for uncertain information ("estimated at," "possibly," "appears to")
- Always include disclaimer at end of report
