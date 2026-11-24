---
layout: single
title: "How I Work"
permalink: /approach/
author_profile: true
---

{% include base_path %}

Eight years across banking, automotive, cybersecurity and cloud have left me with a handful of principles I actually hold to. They're the lens I bring to any codebase.

## Validate against reality, not simulation

The fastest way to ship something wrong is to trust a model of the world instead of the world. In my trading research platform this is a hard rule: a Python backtest is only ever a *hypothesis*; a strategy is promoted only when it survives the real MetaTrader&nbsp;5 engine with live execution delay. I built a fast internal tester, then validated it against 16 real runs and 52,000+ trades before trusting it. The same instinct applies to any system &mdash; measure the real behaviour, don't assume it.

## Own it end to end

The most useful engineers I've worked with don't stop at their function signature. At Huawei I took one tool from requirements through design, build, storage migration, deployment and post-release support to HQ. At Deloitte I owned the CI/CD pipeline for a 150-million-line migration across three countries. I like holding the whole thread &mdash; from the domain logic to the screen to the deploy &mdash; and owning what happens when a dependency fails.

## Modernise without breaking

A lot of my work has been moving large, load-bearing systems to new ground: COBOL to Java, Hive to BigQuery, flat files to MongoDB, a Liferay monolith to microservices. The craft there isn't the new technology &mdash; it's doing it with tests at the seams that matter and a release pipeline that makes each step reversible, so a migration is a series of safe, observable moves rather than one big leap.

## Build with AI, review like an engineer

I build software with agentic AI coding every day, and I treat it as a fast pair-programmer under human review, not an autopilot. I give it goals and guardrails in a precise spec, let it plan, write and test across a large codebase, and I review every change. It's a force multiplier precisely *because* the judgement &mdash; what's correct, what's safe, what's worth doing &mdash; stays with the engineer.

## Measure impact, not activity

Code is a means. The work I'm proudest of is the work that moved a number someone cared about: release time from two weeks to one day, manual translation effort down 56%, classifier precision from 78% to 83%, 23,000+ transactions a day with zero rejected submissions. I try to know, before I start, what number I'm here to move.

---

*Want the specifics? The [portfolio](/portfolio/) has the projects, and the [CV](/cv/) has the full history.*
