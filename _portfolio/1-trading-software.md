---
title: "Financial Trading Software &mdash; Turtle"
excerpt: "A live algorithmic-trading research platform built with agentic AI: real-time tick processing, strategy backtesting validated against MetaTrader 5, MQL5 Expert Advisors, and web dashboards.<br/><img src='/images/turtle-tv-uhv.png'>"
collection: portfolio
---

**Turtle** is a live algorithmic-trading research platform I designed and built &mdash; and it is developed almost entirely through **agentic AI coding** (Claude Code). It ingests raw market tick data, builds candles, detects trading setups, backtests them against a **real MetaTrader 5 execution engine**, and manages live orders with realistic bid/ask spread handling.

[View the code on GitHub &rarr;](https://github.com/zeecitizen/turtle)

## What it does
- **Real-time data pipeline** &mdash; ingests broker tick data, builds M1/M5/M15 candles, and keeps a synchronized view of the market.
- **Strategy research &amp; backtesting** &mdash; a browser-based tester that replays years of data, scores strategies on win rate, profit factor and drawdown, and validates every result against MetaTrader 5 so backtests reflect real execution.
- **Expert Advisors (MQL5)** &mdash; automated strategies that run inside MetaTrader 5.
- **Live trading engine** &mdash; a Python service that reads live quotes and manages positions.

### Turtle TV &mdash; the research &amp; backtesting dashboard
![Turtle TV research dashboard](/images/turtle-tv-uhv.png)

A single-page research cockpit: live chart, adjustable strategy parameters, a strategy tester, an optimizer, risk and exit labs, and one-click export to MetaTrader 5.

### Golden Gravity &mdash; a trading game on real market data
![Golden Gravity trading game](/images/golden-gravity.png)

A browser game that turns live gold-price movements into gameplay &mdash; built on the same real-time market engine.

## Tech
Python &middot; MQL5 / MetaTrader 5 &middot; real-time tick processing &middot; Flask web dashboards &middot; WebSocket / CDP browser automation &middot; pandas / NumPy &middot; agentic AI development (Claude Code)
