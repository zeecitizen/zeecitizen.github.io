---
layout: single
title: "How I Work"
permalink: /approach/
author_profile: true
---

{% include base_path %}

Eight years across banking, automotive, cybersecurity and cloud have left me with a handful of principles I hold to. They're the lens I bring to any codebase.

## Validate against reality, not simulation

The fastest way to ship something wrong is to trust a model of the world instead of the world. In my trading research platform this is a hard rule: a Python backtest is only ever a hypothesis; a setting is promoted only when it survives the real MetaTrader&nbsp;5 engine with live execution delay. I learned it the expensive way &mdash; a Python replay once predicted +$876 on 163 entries that really lost &minus;$657 &mdash; so I built a fast internal tester and validated it against 16 real MT5 runs (52,782 trades) before trusting it. The same instinct applies to any system: measure the real behaviour, don't assume it.

## Own it end to end

I don't stop at my function signature. At Huawei I took one tool from requirements through design, build, a flat-file-to-MongoDB storage migration, deployment and support after release. At Deloitte I owned the CI/CD pipeline for a 150-million-line migration across three countries and led five developers to go-live. I like holding the whole thread &mdash; from the domain logic to the screen to the deploy &mdash; and owning what happens when a dependency fails.

## Modernise without breaking

A lot of my work has been moving large, load-bearing systems to new ground: COBOL to Java, Hive to BigQuery, flat files to MongoDB, a Liferay monolith to microservices. The craft isn't the new technology &mdash; it's doing it with tests at the seams that matter and a release pipeline that makes each step reversible, so a migration is a series of safe, observable moves rather than one big leap.

## Build with AI, review like an engineer

I build software with agentic AI coding every day, and I treat it as a fast pair-programmer under human review, not an autopilot. A concrete example: the AI-written backtester once filled *moved* stops at the entry tick instead of when price reached them &mdash; a look-ahead bug that quietly flattered every result. The AI wrote the code in seconds; catching that it was *wrong* was the engineering. I give the model goals and guardrails in a precise spec, let it plan and write across a large codebase, and review every change &mdash; because the judgement about what is correct and what is safe has to stay with the engineer.

## Measure impact, not activity

Code is a means. The work that lasts is the work that moved a number someone cared about: release time from two weeks to one day, manual translation effort down 56%, classifier precision from 78% to 83%, 23,000+ transactions a day with zero rejected submissions. I try to know, before I start, which number I'm here to move.

---

*The specifics are in the [portfolio](/portfolio/); the full history is in the [CV](/cv/).*
