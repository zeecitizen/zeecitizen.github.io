---
title: "Financial Trading Software &mdash; Turtle"
excerpt: "A live algorithmic-trading research platform built with agentic AI: real-time tick processing, strategy backtesting validated against MetaTrader 5, MQL5 Expert Advisors, and web dashboards.<br/><img src='/images/turtle-tv-uhv.png'>"
collection: portfolio
---

**Turtle** is a live algorithmic-trading research platform I designed and built &mdash; and it is developed almost entirely through **agentic AI coding** (Claude Code). It ingests raw market tick data, builds candles, detects trading setups, backtests them against a **real MetaTrader&nbsp;5 execution engine**, and manages live orders with realistic bid/ask spread handling.

[View the code on GitHub &rarr;](https://github.com/zeecitizen/turtle)

## What it does
- **Real-time data pipeline** &mdash; ingests broker tick data, builds M1/M5/M15 candles, and keeps a synchronized view of the market across several instruments.
- **Strategy research &amp; backtesting** &mdash; a browser-based tester that replays years of tick data, scores strategies on win rate, profit factor and drawdown, and surfaces every trade so a setup can be inspected candle by candle.
- **Expert Advisors (MQL5)** &mdash; automated strategies that run inside MetaTrader&nbsp;5, versioned and tracked from hypothesis to live deployment.
- **Live trading engine** &mdash; a Python service that reads live quotes over a browser bridge and manages positions with correct bid/ask and spread accounting.

## Architecture
A tick ingester feeds a candle builder; a detection engine scans for setups; a tester replays history through the same detection code that runs live, so research and production never drift apart. A fleet of background workers (I call them "hawks") watch the market, collect fills, and report. Dashboards are served by lightweight Python web servers and driven live through the Chrome DevTools Protocol.

## Engineering discipline
The hard lesson baked into the project: **a naive Python backtest lies** &mdash; it fills at the price it wants, with no spread, slippage or latency. So Turtle treats a Python result only as a *hypothesis*, and promotes a strategy only when it survives **MetaTrader&nbsp;5's own strategy tester** (with random delay and the target held at the broker). An internal tester was then validated against 16 MT5 runs totalling 52,000+ trades with a parity check before it was trusted to stand in for MT5. That "validate against reality" rule is the backbone of the whole system.

### Turtle TV &mdash; the research &amp; backtesting dashboard
![Turtle TV research dashboard](/images/turtle-tv-uhv.png)

A single-page research cockpit: live chart, adjustable strategy parameters, a strategy tester, an optimizer, a trade-lifecycle view, risk and exit labs, machine-learning experiments, and one-click export to MetaTrader&nbsp;5.

### Golden Gravity &mdash; a trading game on real market data
![Golden Gravity trading game](/images/golden-gravity.png)

A browser game that turns live gold-price movements into gameplay &mdash; built on the same real-time market engine that powers the research tools.

## Tech
Python &middot; MQL5 / MetaTrader&nbsp;5 &middot; real-time tick processing &middot; NumPy / pandas &middot; Flask web dashboards &middot; WebSocket &amp; Chrome DevTools Protocol automation &middot; SQLite &middot; agentic AI development (Claude Code)
