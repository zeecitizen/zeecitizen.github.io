---
layout: single
title: "Open Source"
permalink: /opensource/
author_profile: true
excerpt: "A fix under review at NASA's OnAIR, and 56 commits on MIT's XCoLab platform."
---

{% include base_path %}

<div class="dexter-lab" markdown="1">

Two contributions to public codebases: a fix now under review at NASA, and my 2017 work on the platform behind MIT's Climate CoLab.

- [**NASA OnAIR** (2026)](#nasa-onair): an open pull request fixing telemetry data loss in the Redis adapter.
- [**MIT XCoLab** (2017)](#mit-xcolab): 56 commits on the collaboration platform of the MIT Center for Collective Intelligence, including a site-wide notifications feature.

## NASA OnAIR (2026) {#nasa-onair}

<span class="lab-badge">NASA OnAIR &middot; pull request open</span>

**A fix for a telemetry data-loss bug in NASA's OnAIR**, the open-source platform from NASA's Goddard Space Flight Center for running AI algorithms on spacecraft telemetry. The pull request was opened on 24&nbsp;November&nbsp;2026 and is awaiting review by the maintainers. It has not been merged.

<a class="lab-btn" href="https://github.com/nasa/OnAIR/pull/210">Pull request #210 on GitHub &rarr;</a>

<div class="lab-stats">
  <div class="lab-stat"><b>18 of 20 &rarr; 0</b><span>updates lost on real Redis, before &rarr; after</span></div>
  <div class="lab-stat"><b>374 tests</b><span>passing, 100% coverage</span></div>
  <div class="lab-stat"><b>Python 3.8&ndash;3.12</b><span>verified on Linux</span></div>
  <div class="lab-stat"><b>~10 lines</b><span>of source changed</span></div>
</div>

### The project

[OnAIR](https://github.com/nasa/OnAIR) (On-board Artificial Intelligence Research) is a Python pipeline that takes live telemetry from a spacecraft or a simulator and hands it, one frame at a time, to AI plugins that learn, plan and reason about it. NASA's [published account](https://doi.org/10.1002/aaai.70020) describes its use in research missions, including an experiment on a payload at the International Space Station.

The change described here is to the Redis data adapter in the open-source repository. It is not flight software, and nothing on this page has flown.

### The bug

OnAIR receives telemetry on a background thread and passes it to the pipeline in frames. The Redis adapter wiped its buffer every time a message arrived. So when two channels published between two frames, the second message erased the first before the pipeline ever saw it.

| | Channel A publishes | Channel B publishes | What the pipeline received |
|---|---|---|---|
| **Before** | x = 10 | x = 20 | A: *nothing*, B: 20 |
| **After** | x = 10 | x = 20 | A: 10, B: 20 |

A member of NASA's team had described the problem in [issue #189](https://github.com/nasa/OnAIR/issues/189) and suggested where the fix belonged: blank the buffer when the two buffers are swapped, not when a message arrives.

### The fix

Three lines in `get_next`, at the moment the buffers swap:

```python
with self.new_data_lock:
    self.new_data = False
    self.double_buffer_read_index = (self.double_buffer_read_index + 1) % 2
    read_index = self.double_buffer_read_index
    # turn all data points of the new write buffer to unknown
    write_buffer = self.current_data[(read_index + 1) % 2]
    write_buffer["data"] = ["-" for _ in write_buffer["data"]]
```

A frame now holds everything received since the previous frame, and the pipeline's existing logic keeps the older values. The per-message wipe is removed.

The second half is concurrency. The listener thread used to write its message without holding the lock. With blanking moved to the swap, a message caught half-way through a swap would have its second half written into a frame that had already been handed out, and lost. So the listener now holds the same lock for the whole message.

### How it was verified

- **Against real servers.** The project's own Redis example was run against two Redis 7.0 servers, with two channels publishing back to back 20 times. On the existing code the first channel's update was lost in 18 of the 20 pairs (20 of 20 in a second run). With the fix, none were lost in either run.
- **Tests that fail when they should.** One existing test encoded the bug and was corrected; three were added. All four fail against the old code. Three deliberately broken versions of the fix are each caught too: a nested lock, blanking in place, and setting the "new data" flag outside the lock.
- **A test for the deadlock nobody would see.** The existing tests replace the lock with a mock, so a version that takes the lock twice, and would freeze on the first real message, passes all of them. A new test asserts exactly one acquire, with the write inside it.
- **Under load.** With real threads and 20,000 messages under aggressive thread switching, the old code let 100 to 176 half-written messages into a frame per run. The fixed code let none. Those are stress conditions, not field rates.
- **The whole suite.** 374 tests pass with 100% statement and branch coverage, on Python 3.8 to 3.12 on Linux.

**Still pending:** NASA's own CI has not run on the pull request yet, because runs from first-time contributors wait for a maintainer's approval. The Linux results above are from my fork.

### Decisions that shaped it

- **The documentation settled the design.** The discussion around these adapters offered two approaches: blank the buffer, or carry old values forward inside the adapter. OnAIR's architecture document defines a frame as only the newly received data, and the pipeline already carries old values forward. Blanking at the swap is therefore the consistent fix, and it is the one NASA's team had suggested.
- **Small on purpose.** No refactoring, no new options, and the documentation page left alone. The pull request description is kept short.
- **One fix per pull request.** The same defect exists in mirror image in the adapter for NASA's cFS flight software ([issue #201](https://github.com/nasa/OnAIR/issues/201)): it never blanks, so consecutive frames alternate between two stale buffers. That fix is written and verified on my fork, with 374 tests passing on Python 3.8 to 3.12, but not submitted. The reporter of that issue prefers a different approach, so I [raised the design question there](https://github.com/nasa/OnAIR/issues/201#issuecomment-7146645822) first. It has not been run against cFS itself.
- **Found along the way.** NASA's CI has been failing on the main branch since 29 October 2026, because the hosted runner image no longer provides Python 3.8 or 3.9; the pull request notes it. One existing test also leaves a line uncovered about one run in ten, depending on a random number.

### How it was built

I work with an AI coding agent, and this contribution was built that way. The agent (Claude Code) did the code reading, wrote the change and its tests, and ran the verification. I chose the project and the issue, approved the approach, reviewed the change before it went out, signed NASA's contributor agreement and submitted the pull request myself. The commit lists Claude as co-author and the pull request says so, in line with the project's request that AI-assisted contributions are reviewed by a person.

### Status

- **Pull request [#210](https://github.com/nasa/OnAIR/pull/210):** open since 24 November 2026, awaiting maintainer review. NASA requires a signed contributor agreement, which I sent the same day.
- **Second fix (cFS/SBN adapter):** ready on my fork, to be submitted after the discussion on issue #201.

*Last updated 25 November 2026.*

### Screenshots

<div class="lab-gallery">
  <figure class="lab-shot"><a href="/images/opensource/onair-pr-210.png" target="_blank" rel="noopener"><img src="/images/opensource/onair-pr-210.png" width="1440" height="1046" alt="Pull request 210 on the nasa/OnAIR repository, open, with its description and one commit" loading="lazy"></a><figcaption>Pull request #210 on nasa/OnAIR &mdash; open, awaiting review &middot; click to enlarge</figcaption></figure>
  <figure class="lab-shot"><a href="/images/opensource/onair-issue-189.jpg" target="_blank" rel="noopener"><img src="/images/opensource/onair-issue-189.jpg" width="1600" height="1067" alt="Issue 189 on nasa/OnAIR with the note of intent and the linked pull request" loading="lazy"></a><figcaption>Issue #189: the problem as NASA's team described it, my note of intent, and the linked pull request</figcaption></figure>
  <figure class="lab-shot"><a href="/images/opensource/onair-checks-redis-fix.jpg" target="_blank" rel="noopener"><img src="/images/opensource/onair-checks-redis-fix.jpg" width="1600" height="977" alt="Eleven successful checks on the fork branch holding the Redis fix" loading="lazy"></a><figcaption>The fix on my fork: tests and lint on Python 3.8&ndash;3.12, plus the run against real Redis servers</figcaption></figure>
  <figure class="lab-shot"><a href="/images/opensource/onair-checks-baseline.jpg" target="_blank" rel="noopener"><img src="/images/opensource/onair-checks-baseline.jpg" width="1600" height="977" alt="The same checks on NASA's unmodified code" loading="lazy"></a><figcaption>Baseline: NASA's unmodified code under the same checks (the Redis run here is the one that shows the loss)</figcaption></figure>
  <figure class="lab-shot"><a href="/images/opensource/onair-checks-sbn-fix.jpg" target="_blank" rel="noopener"><img src="/images/opensource/onair-checks-sbn-fix.jpg" width="1600" height="977" alt="Ten successful checks on the fork branch holding the second fix" loading="lazy"></a><figcaption>The second fix (cFS/SBN adapter), not yet submitted: tests and lint on Python 3.8&ndash;3.12</figcaption></figure>
  <figure class="lab-shot"><a href="/images/opensource/onair-readme.jpg" target="_blank" rel="noopener"><img src="/images/opensource/onair-readme.jpg" width="1316" height="1163" alt="The OnAIR README with its badges" loading="lazy"></a><figcaption>OnAIR's README. The red &ldquo;failing&rdquo; badge is NASA's own CI, broken by the runner change noted above</figcaption></figure>
</div>

---

## MIT XCoLab (2017) {#mit-xcolab}

<span class="lab-badge">MIT XCoLab &middot; 56 commits</span>

**[XCoLab](https://github.com/CCI-MIT/XCoLab)** is the open-source platform behind MIT's [Climate CoLab](https://climatecolab.org), built to let large groups of people work together on proposals for complex problems. I worked on it in 2017 as a visiting student at the MIT Center for Collective Intelligence, where it was also the subject of my master's thesis.

<a class="lab-btn" href="https://github.com/CCI-MIT/XCoLab/commits?author=zeecitizen">All 56 commits on GitHub &rarr;</a>

<div class="lab-stats">
  <div class="lab-stat"><b>56 commits</b><span>March to June 2017</span></div>
  <div class="lab-stat"><b>11 files, 662 lines</b><span>the notifications feature</span></div>
  <div class="lab-stat"><b>Java &middot; Spring</b><span>microservices and web layer</span></div>
  <div class="lab-stat"><b>Arctic Code Vault</b><span>archived by GitHub, 2020</span></div>
</div>

### The strongest piece: site-wide notifications

A feature that lets an administrator publish a timed notice to everyone on the site (ticket COLAB-1701). It runs through the whole stack:

- **Admin microservice.** REST endpoints to create, list and delete notifications, each with a start and an end time. Expired ones are dropped.
- **Typed client.** A REST client for those endpoints in the `admin-client` module.
- **Web layer.** A controller that checks for administrator rights, and a screen in the admin tab to write and remove notices.
- **Browser.** A script that asks for the active notice every three minutes, shows it once, and remembers in a cookie that it was shown.

[Merged into the project's `develop` branch on 13 April 2017](https://github.com/CCI-MIT/XCoLab/commit/d006bf7b7a724f5643b205c04442c513ede2a86e): 11 files, 662 lines added.

### Fixes across the platform

A selection of the others, each tied to a ticket in the project's tracker:

- **Linked proposals showed generic names instead of their own** ([COLAB-1915](https://github.com/CCI-MIT/XCoLab/commit/35133966bc)).
- **A message sent to several people appeared twice in the sender's mailbox** ([COLAB-1605](https://github.com/CCI-MIT/XCoLab/commit/f685b3c67f)).
- **Members with more than one role were listed several times** ([COLAB-1339](https://github.com/CCI-MIT/XCoLab/commit/da34e91f43)).
- **Mass messaging:** certain accounts excluded from the recipient list ([COLAB-1881](https://github.com/CCI-MIT/XCoLab/commit/39cd47fba0)).
- **Facebook sign-in:** corrected conditions in the registration flow ([COLAB-192](https://github.com/CCI-MIT/XCoLab/commit/67791856c4)).

The remaining commits are small interface and wording fixes, and 16 of the 56 are merge commits.

### How this work was done

These commits went in through the team's normal workflow: feature branches named after tickets, merged into `develop`, with commit access as a member of the team. They are not outside pull requests. They date from 2017, well before AI coding agents.

### Preserved in the Arctic Code Vault

In 2020 GitHub's Archive Program stored a snapshot of public repositories in a vault in Svalbard. XCoLab was among them, and contributors to the archived repositories carry the Arctic Code Vault badge on their GitHub profile.

### Screenshots

<div class="lab-gallery">
  <figure class="lab-shot"><a href="/images/opensource/xcolab-notifications-commit.png" target="_blank" rel="noopener"><img src="/images/opensource/xcolab-notifications-commit.png" width="1440" height="1180" alt="The commit that merged the notifications feature into XCoLab: 11 files changed, 662 additions" loading="lazy"></a><figcaption>The notifications feature landing in XCoLab: 11 files across the admin microservice, its client and the web layer &middot; click to enlarge</figcaption></figure>
  <figure class="lab-shot"><a href="/images/opensource/arctic-code-vault-badge.png" target="_blank" rel="noopener"><img src="/images/opensource/arctic-code-vault-badge.png" width="940" height="490" alt="The Arctic Code Vault Contributor badge on the zeecitizen GitHub profile, listing CCI-MIT/XCoLab" loading="lazy"></a><figcaption>The Arctic Code Vault badge on my GitHub profile, for XCoLab</figcaption></figure>
</div>

</div>
