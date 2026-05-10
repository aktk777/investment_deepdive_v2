# Phase 5: Technical Analysis

## Objective
Assess price trend, momentum, and supply/demand to inform entry timing.
Even a fundamentally strong company may not be a buy *right now* if technicals are unfavorable.

## Important Caveat
Technical analysis requires real-time chart data. Web search has limitations here.
This phase takes a hybrid approach:
1. Directly analyze what's obtainable via search (volume, margin trading data)
2. For chart-dependent items, present specific checkpoints for the user to verify
3. Provide reference links to charting tools (TradingView, etc.)

## Researcher: Information Gathering

### Trend Data
- Recent price action (1D / 1W / 1M / 3M / 6M / 1Y)
- 52-week high/low and current position relative to range
- Moving average alignment (25D / 75D / 200D)
- Volume trends (increasing / decreasing)

### Margin Trading Data (Japanese stocks)
- Margin buying balance (信用買い残)
- Margin selling balance (信用売り残)
- Margin ratio (信用倍率)
- Short interest: is it less than ~3 days of average volume? (cover ease)
- Lending ratio trends

### Supply/Demand
- Daily volume vs. historical average
- Order book depth (thick / thin)

### Recommended Search Queries
- `"{company}" stock chart price` / `"{company}" 株価 チャート`
- `"{company}" 信用残 信用倍率` (JP stocks)
- `"{company}" volume trend`
- `"{ticker}" technical analysis moving average 52 week`
- `"{company}" site:kabutan.jp` (JP stock technicals)

### Priority Sources
- Yahoo Finance (charts, technical indicators)
- Kabutan (kabutan.jp) — strong JP stock technical data
- TradingView
- JPX (Japan Exchange Group) — margin trading data

## Analyst: Evaluation

### Trend Assessment
- **Long-term (weekly)**: uptrend / downtrend / range
- **Medium-term (daily)**: uptrend / downtrend / range
- Moving average alignment (perfect order, etc.)
- Position relative to 52-week high/low

### Ichimoku Cloud (recommend user verification)
Suggest the user check on TradingView or similar:
- Price position relative to cloud (above / below / inside)
- Tenkan-sen / Kijun-sen cross
- Chikou Span position
- Three-line bullish/bearish signal (三役好転/三役逆転)

### Margin Trading Assessment (JP stocks)
- Short interest ÷ avg daily volume = days to cover → ≤3 days is manageable
- Margin ratio > 5 → overweight on longs, potentially heavy overhead
- Margin ratio improving or deteriorating trend

### Volume Analysis
- Rising volume + rising price → strong (real demand buying)
- Declining volume + rising price → weak (buyer fatigue possible)
- Volume spike → event-driven; check for news catalyst

## Internal Summary Format

```
【Phase 5: Technical Analysis】
■ Trend: Long-term [up/down/range]、Medium-term [up/down/range]
■ Position: __% from 52wk high / [above/below] key MAs
■ Volume: [increasing/decreasing/stable] — __x vs average
■ Margin Data: Ratio __x、Days to cover __
■ Technical Judgment: [Bullish signal/Neutral/Bearish signal] — [basis]
■ User Verification Recommended: [Ichimoku status / order book depth / etc.]
```
