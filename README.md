# 🌊 OrderFlow Magic
### *Gunbot Strategy — I don't guess. No TA. Pure market structure action.*
> **Entries driven by real market pressure. Not candles. Not RSI. The actual order book.**  
> *Read what institutions are doing. Trade alongside them.*
---
Most bots look at candles — where price **was**. OrderFlow Magic looks at the live order book and real executed trades — where the market **is going**. It reads the same data professional traders use on trading floors and turns it into automated, self-calibrating entries.
No manual threshold tuning. No lagging indicators. No guessing.
---
## What it does

**🌊 Live Order Book Imbalance**  
Calculates buying vs selling pressure in real time as a normalized score from -1 (pure sell pressure) to +1 (pure buy pressure). This is the primary entry signal.

**🔬 Public Trade History (tape)**  
Cross-checks the order book against real executed trades from the last 90 seconds. If the book looks bullish but real trades confirm sellers are aggressive → skip. Both signals must agree.

**🧬 Self-Calibrating via Genetic Algorithm**  
The GA evolves buy/sell thresholds against up to 1000 real candles of this pair's history, across 60 candidates and 220 generations. It recalibrates every 4 hours — or immediately if market volatility shifts. No manual tuning needed. USDT-BTC gets its own thresholds. USDT-ETH gets its own. They evolve independently.

**🏗️ Multi-Timeframe S/R Confluence**  
Entries only fire when price is near a level that multiple timeframes agree on. No buying in no-man's-land between supports.

**🐋 Institutional Absorption**  
Detects when large players are defending a level — high sell volume on tape, price barely moving, bid side holding. Used as extra confirmation or to override a bearish tape read.

**🪤 Bull & Bear Trap Detection**  
Cross-checks order book shape against actual trade flow. Suspicious divergences block the entry (bull trap) or the rebuy (bear trap).

**🏦 Institution Hours**  
Full power during London–NY overlap (7–21 UTC). Outside those hours: scout mode raises thresholds 1.4× and enforces absorption and volume checks, or block mode stops entries entirely. Exits are never affected. Weekend mode lets you treat Saturday and Sunday as dead zone, block all entries, or ignore the weekend entirely.

**💰 Compound Profits**  
Trade Size adjusts automatically based on cumulative realized PnL across completed cycles. Winners grow the next entry. Losers shrink it. Hard floor at 25% of base size.

**🧊 Dump Protection**  
During confirmed orderbook dumps, the profit floor for partial sells is temporarily lowered so consecutive chunks keep firing instead of getting stuck after the first one.

**📡 Signal to Pair**  
Trade on illiquid bases — like USDC-BTC where MiCA regulations route your capital — while pulling OrderFlow signals from the deep, liquid USDT-BTC book. Configure your trading pair and signal pair independently. The bot reads the order book and PTH tape from the liquid pair, runs the GA calibration against that data, and executes orders on your actual trading pair. Full depth where it matters, execution where you need it.

**🏗️ S/R Zone Exhaustion**  
Prevents the bot from stacking rebuys at the same support level. When price oscillates around a zone, OF signals can fire repeatedly at essentially the same price. Zone Exhaustion caps how many rebuys are allowed per S/R level — once exhausted, new rebuys are blocked until price reaches the next lower support. Partial sells reset the counter.

**🚫 Exit: Ignore All OF Filters**  
When enabled, bypasses all OrderFlow gates on exits — side, imbalance threshold, iceberg. Both full exits and partial sells fire based on profit math alone. When your configured MIN_PROFIT is reached, the sell executes regardless of order book conditions. Dump Protection will not lower the profit floor while this is active.

**📐 Auto-ATR DCA Spread**  
Instead of fixed percentage drops for re-buys, the spacing is computed from the Average True Range. High volatility widens the spread to catch deeper wicks; low volatility tightens it to keep accumulating. Each direction — down-rebuys and up-rebuys — can independently pull ATR from a different timeframe (entry period, long, or daily).

---
## Sidebar — what you see live
```
🌊 Signal         BULLISH
⚖️  Imbalance      0.6134
🟢 Buy threshold  0.5801 ✓
🔴 Sell threshold -0.4920
🧬 GA samples     1031 / 200 ✓
🪤 Trap guard     CLEAR
🔬 PTH trades     1000 (3s ago)
🏗️  S/R gate       ON (±1.0%)
🏗️  S/R zone       2 / 3
🐋 Absorption     ON
💰 Compound       +3.20 → 53.20
🏦 Inst. Hours    FULL POWER (UTC 14h)
📡 Signal Pair    USDT-BTC ✅
📉 Entry if drops ~59,800 · READY TO FIRE
📈 Entry if rises ~61,200 · OFF
```
---
## Two ready-to-use presets

| | `orderflow_scalp` | `orderflow_institutional` |
|---|---|---|
| Candles | 5m | 15m |
| Schedule | 24/7 | Institution Hours (7–21 UTC, scout off-hours) |
| Rebuy cooldown | 2 candles (10 min) | 3 candles (45 min) |
| Absorption | Off (speed) | On (precision) |
| Dump Protection | ✅ | ✅ |
| Best for | BTC, ETH, liquid pairs | ETH, SOL, higher conviction setups |

Apply a preset from the GUI and everything configures itself.

---
## Works without PTH

Exchanges that don't expose public trade history (e.g. Hyperliquid) are fully supported. The order book imbalance is the core signal — PTH is an optional confirmation layer. You'll see `🔬 PTH trades 0 (n/a)` in the sidebar and everything else runs normally.

---
## Hybrid mode — TA entries, OF exits

Don't want to commit to full OrderFlow entries? You can run classic TA entries (Heikin Ashi, Pullback, Breakout) while using OrderFlow for partial exits. Enable `Partial Sell: Orderflow Gate` with Orderflow Mode off. The OF tape reads when to take profits — the TA decides when to enter. Test the exit engine independently before going full OF.

---
## 📖 Full Documentation

Everything is in the [Wiki](https://github.com/truitaman/OrderFlow-Magic/wiki):
- [What is OrderFlow Trading?](https://github.com/truitaman/OrderFlow-Magic/wiki#-1-what-is-orderflow-trading)
- [The Machine Learning Engine](https://github.com/truitaman/OrderFlow-Magic/wiki#-2-the-machine-learning-engine)
- [Support & Resistance Confluence](https://github.com/truitaman/OrderFlow-Magic/wiki#-3-support--resistance-confluence)
- [Trap Detection, Absorption, Institution Hours](https://github.com/truitaman/OrderFlow-Magic/wiki#-4-bull--bear-trap-detection)
- [Compound Profits & Dump Protection](https://github.com/truitaman/OrderFlow-Magic/wiki#-7-compound-profits)
- [Signal to Pair](https://github.com/truitaman/OrderFlow-Magic/wiki#-signal-to-pair)
- [All Settings Reference](https://github.com/truitaman/OrderFlow-Magic/wiki#-11-orderflow-settings-reference)
- [OrderFlow vs Classic Branch](https://github.com/truitaman/OrderFlow-Magic/wiki#-14-orderflow-vs-classic-branch)
- [Tips & Best Practices](https://github.com/truitaman/OrderFlow-Magic/wiki#-15-tips--best-practices)

---
## 🛍️ Get the Strategy

Requires a valid **Gunbot Defi license** to run.

> 🛒 [Purchase OrderFlow Magic](https://www.gunbot.com/devcommunity/wickmagic)

No Defi license yet?  
→ Upgrade from Standard — $190: [Purchase here](https://checkout.gunbot.com/crazymop/promoStandardToDefi)  
→ Upgrade from Pro — $100: [Purchase here](https://checkout.gunbot.com/crazymop/promoProToDefi)  
→ New to Gunbot: [gunbot.com](https://checkout.gunbot.com/crazymop/ultimate)
