# 🌊 OrderFlow Magic
### *Gunbot Strategy — I don't guess. No TA. Pure market structure action.*
> **Entries driven by real market pressure. Not candles. Not RSI. The actual order book.**  
> *Read what institutions are doing. Trade alongside them.*

**Current version: OrderFlow Magic 1.2.20** (Wick Magic Spot v2.9.v33)

---
Most bots look at candles — where price **was**. OrderFlow Magic looks at the live order book and real executed trades — where the market **is going**. It reads the same data professional traders use on trading floors and turns it into automated, self-calibrating entries.
No manual threshold tuning. No lagging indicators. No guessing.

---
## What it does

**🌊 Live Order Book Imbalance**  
Calculates buying vs selling pressure in real time as a normalized score from -1 (pure sell pressure) to +1 (pure buy pressure). This is the primary entry signal.

**🔬 Public Trade History (tape)**  
Cross-checks the order book against real executed trades from the last 90 seconds. If the book looks bullish but real trades confirm sellers are aggressive → skip. Both signals must agree. Wick Magic reads the tape **directly from the exchange** on Hyperliquid, AsterDex, Binance, Bybit, Bitget, Kraken, MEXC, Crypto.com, HTX, Coinbase, KuCoin and OKX, and from Gunbot everywhere else.

**🧬 Self-Calibrating via Genetic Algorithm**  
The GA evolves buy/sell thresholds against this pair's own recent market history. It recalibrates every 4 hours — or immediately if market volatility shifts. No manual tuning needed. USDT-BTC gets its own thresholds. USDT-ETH gets its own. They evolve independently.

**🏗️ Multi-Timeframe S/R Confluence**  
Entries only fire when price is near a level that multiple timeframes agree on. No buying in no-man's-land between supports.

**🐋 Institutional Absorption**  
Detects when large players are defending a level — high sell volume on tape, price barely moving, bid side holding. Used as extra confirmation or to override a bearish tape read.

**🪤 Bull & Bear Trap Detection**  
Cross-checks order book shape against actual trade flow. Suspicious divergences block the entry (bull trap) or the rebuy (bear trap).

**🏦 Institution Hours**  
Full power during London–NY overlap (7–21 UTC). Outside those hours: scout mode raises thresholds 1.4× and enforces absorption and volume checks, or block mode stops entries entirely. Exits are never affected. Weekend mode lets you treat Saturday and Sunday as dead zone, block all entries, or ignore the weekend entirely.

**💰 Compound Profits**  
Trade Size adjusts automatically in proportion to your realized return on Total Capital across completed cycles. Winners grow the next buys. Losers shrink them. Hard floor at 25% of base size.

**🧊 Dump Protection**  
During confirmed orderbook dumps, the profit floor for partial sells is temporarily lowered so consecutive chunks keep firing instead of getting stuck after the first one. And if a partial sell target is reached but the order book never turns bearish, **Stuck Gate Fallback** fires it after a few candles anyway.

**🛡️ Protections: ATR Strength, ROC Momentum & Chop Guard**  
Three optional entry filters in one editor tab. ATR Strength raises the bar in dead or violent markets, ROC Momentum skips entries at the tail end of a fading move, and Chop Guard holds back fresh buys while the market moves sideways.

**🔪 Kill Position**  
An optional hard stop for the full bag, in USD or in % below your average entry. A disaster stop on top of the normal exits.

**📡 Signal to Pair**  
Trade on illiquid bases — like USDC-BTC where MiCA regulations route your capital — while pulling OrderFlow signals from the deep, liquid USDT-BTC book. The signal pair can even live on another exchange (**Signal Exchange**). Configure your trading pair and signal pair independently. The bot reads the order book and PTH tape from the liquid pair, runs the GA calibration against that data, and executes orders on your actual trading pair. Full depth where it matters, execution where you need it.

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
🌀 Chop Guard     ✅ OK · ADX 27.4 / 18
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
## Tape everywhere, works without it too

Twelve exchanges have their own live tape feed (Hyperliquid, AsterDex, Binance, Bybit, Bitget, Kraken, MEXC, Crypto.com, HTX, Coinbase, KuCoin, OKX), and every other exchange uses Gunbot's trade history. The source is chosen automatically. If an exchange has no tape at all, OrderFlow still works: the order book imbalance is the core signal, and you'll see `🔬 PTH trades 0 (n/a)` in the sidebar.

---
## Hybrid mode — TA entries, OF exits

Don't want to commit to full OrderFlow entries? You can run classic TA entries (Heikin Ashi, Pullback, Breakout) while using OrderFlow for partial exits. Enable `Partial Sell: Orderflow Gate` with Orderflow Mode off. The OF tape reads when to take profits — the TA decides when to enter. Test the exit engine independently before going full OF.

Or the other way round: OrderFlow entries with the classic TA partial sells, with `Partial Sell: TA Exit Mode`.

---
## 🔥 What's New in 1.2.20
- **Compound Profits grows with your capital** — proportional to your real return on Total Capital, on every buy
- **Less log noise** on S/R zone changes

**Also in 1.2.19**
- **Own tape feed on 12 exchanges**, and Binance tape that keeps working when the core returns an API-key error
- **Signal Exchange** — read the Signal Pair from another exchange
- **Chop Guard** and the new **Protections** editor tab
- **Stuck Gate Fallback** and **TA Exit Mode** for partial sells
- **Kill Position** hard stop (USD or %)
- **Order safety** — no buy without free funds, failing orders pause instead of looping
- Cleaner GA calibration, trap guard direction fix, Compound cycle fixes
- Readable sidebar decimals, Futures chart layout with the next OF re-buy line, new **Fear & Greed** tab

---
## 📖 Full Documentation

Everything is in the [Wiki](https://github.com/truitaman/OrderFlow-Magic/wiki):
- [What's New in 1.2.20](https://github.com/truitaman/OrderFlow-Magic/wiki#-whats-new-in-orderflow-magic-1220)
- [What is OrderFlow Trading?](https://github.com/truitaman/OrderFlow-Magic/wiki#-1-what-is-orderflow-trading)
- [The Machine Learning Engine](https://github.com/truitaman/OrderFlow-Magic/wiki#-2-the-machine-learning-engine)
- [Support & Resistance Confluence](https://github.com/truitaman/OrderFlow-Magic/wiki#-3-support--resistance-confluence)
- [Trap Detection, Absorption, Institution Hours](https://github.com/truitaman/OrderFlow-Magic/wiki#-4-bull--bear-trap-detection)
- [Compound Profits](https://github.com/truitaman/OrderFlow-Magic/wiki#-7-compound-profits)
- [ATR Strength, ROC & Chop Guard](https://github.com/truitaman/OrderFlow-Magic/wiki#-8-atr-strength-classifier--roc-acceleration-filter)
- [Dump Protection & Stuck Gate Fallback](https://github.com/truitaman/OrderFlow-Magic/wiki#-9-dump-protection)
- [Kill Position](https://github.com/truitaman/OrderFlow-Magic/wiki#-kill-position-hard-stop)
- [Signal to Pair](https://github.com/truitaman/OrderFlow-Magic/wiki#-11-signal-to-pair-cross-pair-orderflow)
- [All Settings Reference](https://github.com/truitaman/OrderFlow-Magic/wiki#-13-orderflow-settings-reference)
- [OrderFlow vs Classic Branch](https://github.com/truitaman/OrderFlow-Magic/wiki#-16-orderflow-vs-classic-branch)
- [Tips & Best Practices](https://github.com/truitaman/OrderFlow-Magic/wiki#-17-tips--best-practices)

---
## 🛍️ Get the Strategy

Requires a valid **Gunbot Defi license** to run.

> 🛒 [Purchase OrderFlow Magic](https://www.gunbot.com/devcommunity/wickmagic)

No Defi license yet?  
→ Upgrade from Standard — $190: [Purchase here](https://checkout.gunbot.com/crazymop/promoStandardToDefi)  
→ Upgrade from Pro — $100: [Purchase here](https://checkout.gunbot.com/crazymop/promoProToDefi)  
→ New to Gunbot: [gunbot.com](https://checkout.gunbot.com/crazymop/ultimate)
