---
title: "Cyber Threat Intelligence NLP Pipeline (Huawei)"
excerpt: "An NLP pipeline that crawls, parses and classifies 100,000+ security articles into STIX objects &mdash; human-in-the-loop validation raised precision from 78% to 83%."
collection: portfolio
---

At Huawei's German Research Center, I built an end-to-end natural-language-processing pipeline for the cyber-security team that turns the firehose of public security writing into structured, queryable threat intelligence.

**What it does.** It crawls security articles, parses and annotates them, and classifies the content into **STIX** objects (the standard format for cyber-threat intelligence). A human-in-the-loop validation stage lets analysts correct the model, and that feedback raised classification precision from **78% to 83%** over **100,000+** articles.

**Beyond the model.** I migrated the pipeline's storage from flat files to **MongoDB**, making every article searchable in seconds, and turned the research prototype into production-ready, documented code integrated into Huawei's in-house systems &mdash; batch processing, scheduled jobs, failure monitoring and retry, RBAC and versioning included.

**Tech:** Python, Flask, NLP, MongoDB, scheduled batch processing.

*Research project at Huawei &mdash; summary only.*
