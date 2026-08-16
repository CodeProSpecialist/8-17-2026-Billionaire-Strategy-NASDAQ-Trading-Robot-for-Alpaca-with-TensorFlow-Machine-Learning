Billionaire Strategy — NASDAQ Stock Trading Robot for Alpaca
Brand-New 2026 Automated Trading System — August 16, 2026

This is a new, fully integrated Alpaca stock-trading robot built around a disciplined “buy at the lowest reasonable price” philosophy. The robot searches the S&P 500 for technically strong stocks that are temporarily discounted or pulling back, confirms the setup across multiple timeframes, ranks the best opportunities, sizes positions according to risk, and manages exits automatically.

The system combines traditional technical analysis, market-regime awareness, adaptive trade statistics, a TensorFlow machine-learning brain, ATR-based risk management, automated profit-taking, persistent trade history, and scheduled historical backtesting into one Python program.

The entire trading system is designed to operate from a single program rather than a collection of separate scanner, trading, ML, and backtesting scripts.
What Makes This Program New
1. Integrated S&P 500 Stock Scanner

The stock scanner is built directly into the trading robot.

On startup, the program scans the S&P 500 and builds an in-memory candidate universe. It ranks stocks using a combination of:

    RSI

    MACD

    Real anchored VWAP

    Bollinger Bands

    Stochastic

    ADX

    OBV

    Seasonal performance

    Historical best-month performance

    Trend and momentum conditions

    Sector controls and exclusions

The resulting candidate list is stored in SYMBOLS_TO_BUY_LIST and remains available throughout the trading session.

The scanner refreshes automatically once per day at approximately 4:15 PM ET. Purchases do not remove symbols from the universe, allowing the same stock to be reconsidered as its technical conditions change.

No external scanner program or text-file symbol list is required.
2. Real VWAP-Based Price Analysis

The robot uses actual volume-weighted price calculations rather than a simple average of sampled prices.

The scanner calculates anchored VWAP using:

Σ(Typical Price × Volume) / Σ(Volume)

The live buy logic also calculates session-anchored intraday VWAP from 1-minute OHLCV data.

The intraday VWAP information helps determine whether a stock is trading meaningfully below its current session value while other reversal conditions are developing.

VWAP data is cached to reduce unnecessary market-data requests.
3. High-Quality Dip and Reversal Buying Strategy

The robot is not designed to simply chase stocks that are already rising.

Instead, it searches for stocks that remain structurally healthy while experiencing a potentially attractive pullback or reversal.

A candidate must satisfy the program's required entry filters before it can become a buy candidate, including:

    Price above the 200-day SMA.

    Daily RSI at or below the configured non-overbought threshold.

    Bullish multi-timeframe confirmation.

    No active earnings blackout condition.

    A bullish reversal candlestick pattern when required by the entry logic.

    A buy score meeting the current market-regime threshold.

Supported reversal patterns include patterns such as:

    Hammer

    Bullish Engulfing

    Morning Star

    Piercing Line

    Three White Soldiers

    Dragonfly Doji

    Inverted Hammer

    Tweezer Bottom

The robot then ranks qualified candidates instead of blindly buying every stock that passes the filters.
4. Multi-Timeframe Confirmation

The strategy combines multiple time horizons instead of relying on one chart.

The live entry system considers:

    Daily trend and momentum

    60-minute trend confirmation

    5-minute price-action confirmation

    Current-session intraday behavior

This allows the robot to distinguish a potentially healthy pullback from a stock that is simply breaking down.
5. Market-Regime Awareness

The robot recognizes different market environments, including:

    Bull

    Sideways

    Bear

    Panic

Market conditions are evaluated using broad-market information including the VIX and SPY trend relationships.

The market regime changes the buy-score requirements and signal weighting so that the robot does not treat every market environment identically.

A strong stock setup can therefore be evaluated differently during a healthy bullish market than during a broad market decline or panic environment.
6. Dynamic Buy-Score System

The buy score combines multiple independent signals, including:

    Bullish candlestick patterns

    RSI condition and direction

    Volume behavior

    MACD confirmation

    Daily price pullback

    Intraday pullback from the day's high

    Distance below session VWAP

    Pattern-specific confirmation

    Price stability

    Relative strength versus SPY

    ATR and volatility characteristics

    Machine-learning adjustment when available

The program does not simply buy the highest raw score.

Candidates are ranked using a reward-versus-risk concept, with expected reward compared against ATR-based risk.

When sufficient trade history exists, the robot can also use empirical expected returns from its own historical trade outcomes to improve candidate ranking.
7. Adaptive Trading Parameters

The robot records its own trading results and analyzes which conditions have historically produced better or worse outcomes.

Adaptive parameters can adjust within predefined safety limits.

The purpose is not uncontrolled self-modification. The program uses minimum sample requirements, bounded adjustments, hard limits, and persistent state so that the adaptive system cannot freely rewrite the strategy.

Trade-history analysis also produces expectancy and parameter-performance information.
8. TensorFlow Machine-Learning Brain

The new program contains an integrated TensorFlow sequence-model brain.

There is one shared ML brain, not a separate neural network for every stock.

The model uses a 20-day sequence of daily market observations and combines:
Stock information

    RSI

    MACD

    MACD signal

    MACD crossover state

    ATR percentage

    Daily return

    Relative volume

    Distance from SMA20

    Distance from SMA50

    Short-term versus long-term trend

    Relative strength versus NASDAQ

NASDAQ market-state information

The model uses QQQ as the NASDAQ-100 market-state proxy and incorporates:

    NASDAQ RSI

    NASDAQ MACD

    NASDAQ MACD signal

    MACD state

    ATR

    Daily return

    5-day return

    20-day return

    60-day return

    Distance from SMA20/50/100/200

    SMA slopes

    SMA20 versus SMA50

    Realized volatility

    Volatility percentile

    Relative volume

    Rate of change

The model architecture contains a frozen technical-analysis foundation followed by:

Conv1D → BatchNorm → Dropout → LSTM → Dropout → LSTM → Dropout → Dense → Win Probability

The frozen foundation preserves basic technical-analysis priors such as:

    Oversold conditions

    MACD bullish confirmation

    Healthy uptrends

    Dip-buy setups

    Volatility

    Volume

    Short-term strength

    Bearish conditions

    NASDAQ bullish/bearish environment

    Stock-versus-NASDAQ relative strength

The trainable sequence head learns temporal patterns and interactions on top of those fixed technical foundations.
9. ML Training and Safety Controls

The ML system is deliberately prevented from becoming an unrestricted decision maker.

On first run, the model can bootstrap with approximately 2,500 historical examples.

The scheduled overnight training system can process approximately 15,000 historical examples per run, with a lifetime historical-pretraining cap of 20,000 examples.

After the historical-pretraining cap is reached, the scheduled training process switches to maintenance training based on recent live win/loss outcomes.

The ML adjustment:

    Does not become a hard buy gate.

    Does not activate for live scoring until at least 60 closed live trades exist.

    Is capped at approximately ±1.5 buy-score points.

    Uses a confidence threshold.

    Uses focal loss to concentrate learning on difficult examples.

    Gives additional weight to losing examples.

TensorFlow is loaded lazily. If TensorFlow is unavailable, the trading robot continues operating without the ML adjustment rather than failing at startup.
10. Risk-Based Position Sizing

Position size is based on the amount of capital the robot is willing to risk rather than simply buying a fixed number of shares.

The default design targets approximately 1% of account equity risk per trade, using the same ATR multiplier that the real hard stop enforces.

Additional controls include:

    Maximum allocation per symbol

    Maximum portfolio exposure

    Available buying power

    Margin health

    Cash buffer

    Minimum order size

    Fractional-share support

    Optional small-dollar testing

This keeps position sizing connected to the actual downside protection used by the exit system.
11. Real ATR Hard Stop-Loss

The program now has a genuine downside stop that operates independently of the profit-monitoring system.

The default hard stop is:

2 × ATR below the entry price

with a configured minimum percentage floor.

The stop is checked even if the position has never reached the profit-monitoring activation point.

This is important because a position can now be force-sold when it immediately moves against the trade rather than being allowed to continue falling simply because the profit monitor was never armed.

The same ATR multiplier is used by position sizing, so the risk calculation and actual hard-stop distance are aligned.
12. Peak-Following Profit Management

Once a position reaches its profit activation threshold, the robot tracks the position's highest price.

Instead of using one fixed trailing percentage for every trade, the give-back allowance can expand as the position's peak profit becomes larger.

This gives strong trends more room to continue while still protecting accumulated gains.

The system can also use ATR-based arming and give-back calculations.
13. Automatic Scale-Out Profit Taking

The robot can divide a winning position into multiple profit-taking stages.

The default scale-out configuration can sell portions of the original position as predefined profit levels are reached.

After a successful scale-out, the remaining position can receive stronger protection, including movement of the profit floor toward breakeven.

Importantly, the robot now confirms the actual order result before considering a scale-out stage complete.

A rejected, canceled, expired, or otherwise unsuccessful order is not silently recorded as a completed sale.
14. Automated Profit Sweeps

The robot performs scheduled profit sweeps around important market periods.
Pre-market sweep

Approximately 9:25 AM ET, using a tightly controlled market-on-open submission window.
Pre-close sweep

Approximately 3:45 PM ET.

The sweep system uses an escalation process that can move from an initial order to a fallback limit order and ultimately to a market order when necessary.

Before a sweep attempts to sell, it first cancels existing sell orders for the symbol so the sweep can control the full available position.
15. Reliable Order Handling

The program polls orders until they reach a terminal state such as:

    Filled

    Canceled

    Expired

    Rejected

This prevents the database and strategy state from assuming that an order filled when the broker actually rejected or canceled it.

The program also handles partial fills and uses retry/error-handling logic around broker API operations.
16. Portfolio-Level Risk Controls

The robot monitors the account as a whole rather than treating every position independently.

Controls include:

    Margin health floor

    Portfolio exposure limit

    Leverage/buying-power limitations

    Cash buffer

    Broker account restrictions

    Position reconciliation

    Order ownership

    Thread-safe position management

New purchases are suspended when the account violates configured margin-health conditions.
17. Persistent SQLite Trading Database

The program maintains a local SQLite database containing information such as:

    Positions

    Trade history

    Entry/exit information

    Feature snapshots

    Learned/adaptive parameters

The database uses locking/WAL behavior appropriate for the multi-threaded program.

On startup, the local database is reconciled against the actual Alpaca account.

That means stale positions can be removed and missing or incorrect position quantities and average prices can be corrected from the broker's live position data.
18. Weekly Automated Backtesting

The program now contains an integrated Backtrader-based historical testing system.

Backtesting is enabled by default and runs automatically once per week on:

Sunday at approximately 10:00 PM ET

The backtest normally uses the same candidate universe produced by the live scanner and evaluates approximately 2 years of daily market history.

The backtest reuses the live program's trading constants, including:

    ATR hard-stop settings

    Profit-arm settings

    Give-back settings

    Scale-out stages

    Risk-per-trade settings

    Maximum allocation

    Buy-score threshold

This greatly reduces the chance of the backtest using a different set of parameters from the live strategy.

The backtest reports statistics such as:

    Final portfolio value

    Total trades

    Sharpe ratio

    Maximum drawdown

    Annualized return

    SQN

The backtest is an approximation rather than a tick-by-tick recreation of live execution. Daily-bar ordering, real execution latency, live order escalation, and exact intraday slippage are not fully reproduced.

Backtrader is optional and loaded lazily, so the live robot can still run if the backtesting package is unavailable.
19. One Unified Program

The major architectural goal of this new version is consolidation.

The program contains the major components in one Python application:

S&P 500 Scanner
→ Candidate Universe
→ Market-Regime Analysis
→ Technical Entry Filters
→ Buy Scoring
→ Adaptive Ranking
→ ML Adjustment
→ Risk-Based Position Sizing
→ Alpaca Execution
→ Hard Stop / Profit Monitor
→ Scale-Outs / Profit Sweeps
→ Trade Database
→ Adaptive Learning
→ ML Training
→ Weekly Backtesting

There is no requirement for a separate scanner process, separate ML server, RunPod server, WebSocket server, or cryptocurrency data source.
20. Reliability and Performance

The program uses:

    Shared market-data rate limiting

    Batched yfinance downloads

    In-memory caching

    Thread-safe locks

    Per-symbol position claims

    Lazy loading of TensorFlow

    Lazy loading of Backtrader

    API retries

    Order timeouts

    Database locking

    Position reconciliation

    Persistent ML state

    Persistent adaptive-parameter state

    Detailed logging

    CSV trade logging

The live trading loop normally checks market conditions approximately once per minute.

The overnight ML process is scheduled outside the primary trading period, and the weekly backtest runs separately on Sunday night.
Installation

Ubuntu 24.04 LTS is recommended.

The unified installation process installs the system dependencies, TA-Lib, TensorFlow, and required Python packages.

sudo bash install.sh

Configure Alpaca credentials:

export APCA_API_KEY_ID='your_key'
export APCA_API_SECRET_KEY='your_secret'
export APCA_API_BASE_URL='https://paper-api.alpaca.markets'

Paper trading is strongly recommended before connecting the program to a live account.

Start the robot:

python3 billionaire-strategy-buy-lowest-price-stock-market-robot.py

Important Operating Characteristics

    The robot is designed for Alpaca stock trading.

    It is not a cryptocurrency trading bot.

    It does not require a separate stock-scanner program.

    The candidate universe is maintained in memory.

    The program supports same-day position entry and exit.

    Manual broker-side position changes can be reconciled with the local database.

    Machine learning supplements the buy score rather than completely controlling the decision.

    Adaptive parameters operate within predefined safety boundaries.

    Historical backtesting is automated weekly.

    Live trading and backtesting use the same core strategy constants where applicable.

Risk Disclaimer

This software is not affiliated with or endorsed by Alpaca Securities, LLC.

Automated trading involves substantial financial risk. Past performance, backtest results, machine-learning predictions, technical indicators, or historical expectancy do not guarantee future results.

Backtests are approximations and cannot reproduce every aspect of live execution, including market impact, liquidity changes, slippage, latency, order rejection, gaps, and intraday price sequencing.

Use paper trading and thorough testing before risking real capital. Never risk money you cannot afford to lose.

This software is provided on an “as-is” and “use-at-your-own-risk” basis without guarantees of profitability or uninterrupted operation. The developer is not responsible for financial losses or damages resulting from use of the software, to the fullest extent permitted by law.

Trade with discipline. Protect capital first.
