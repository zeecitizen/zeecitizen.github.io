---
title: "Software Architecture for Crowdsourced Idea Generation (M.Sc. Thesis)"
collection: publications
permalink: /publication/2017-masters-thesis
excerpt: 'My TU Munich master''s thesis: migrating a large-scale crowdsourced idea-generation platform from a monolith to microservices, researched at the MIT Center for Collective Intelligence.'
date: 2017-09-15
venue: 'M.Sc. Thesis, Technical University of Munich'
citation: 'Zeeshan, Mohammad. (2017). "Software Architecture for Crowdsourced Idea Generation." M.Sc. Thesis, Technical University of Munich.'
---

My master's thesis, researched on campus at the **MIT Center for Collective Intelligence**, studied the software architecture of **XCoLab** &mdash; a platform that lets thousands of experts and non-experts collaborate on proposals for hard problems such as climate change.

The work migrated the platform from a monolithic **Liferay Portal** application (tightly coupled Java portlets on Liferay's ServiceBuilder ORM) toward containerised **microservices**, removing the portal lock-in caused by Liferay's weak inter-portlet communication so that each domain feature could be deployed independently and scaled horizontally.
