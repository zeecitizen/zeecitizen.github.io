---
title: "HD Maps for Autonomous Driving (Samsung HARMAN / BMW)"
excerpt: "Built the navigation-database compiler that generates HD maps for BMW's autonomous-driving programme &mdash; lane rules, internationalised signs, deterministic output.<br/><img src='/images/proj-maps.svg'>"
collection: portfolio
---

**Context.** A self-driving car depends on **HD maps** &mdash; the high-definition geo-data it uses to understand the road ahead. At Samsung HARMAN's Connected Car division I worked on the compiler that builds those maps for BMW's autonomous-driving programme.

**What I did.** Developed features for the navigation-database (NDB) compiler across a multi-million-line Java codebase: implemented **lane-restriction** support (working with traffic simulations), **internationalised** the warning signs, and fixed **non-determinism** bugs so the compiler produced identical, reproducible output on every run &mdash; essential for a safety-critical pipeline where the same input must always yield the same map.

**Tech:** Java, PostgreSQL, JUnit, Mockito, Maven, Jenkins, SonarQube.

*Corporate project &mdash; sanitized overview.*
