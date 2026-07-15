+++
title = "We Built a Digital Twin for the Part of Learning Nobody Has Time For"
date = 2026-07-15T10:00:00Z
slug = "stacktwin-nebius-serverless-challenge"
description = "How StackTwin turns a week's worth of technical noise into a personal learning track with finite Nebius Serverless AI Jobs."
draft = true

[taxonomies]
tags = ["NebiusServerlessChallenge", "Serverless AI", "MLOps", "Digital Twin", "Continuous Learning"]

[extra]
featured = false
author = "Zubin J"
+++

Every working developer has the same Sunday-night problem. Not a lack of things to learn, there's an infinite supply of that, but the exhausting job of triage: is this arXiv paper worth an hour, is that framework release worth a Tuesday, is the flashy conference talk actually useful or just well-edited. The feed doesn't care. It hands you a paper, a changelog, and a thumbnail in the same breath and expects you to sort it out. That sorting is the actual cost of continuous learning, and it's the part we set out to automate with StackTwin.

- Repository: [github.com/BifunctionalLabsCo/stacktwin](https://github.com/BifunctionalLabsCo/stacktwin)
- Challenge tag: `#NebiusServerlessChallenge`

## The motivation

StackTwin's promise is narrow on purpose: instead of another feed to scroll, you get a small, source-backed weekly learning track. A handful of lessons, picked for you, that you can actually finish.

To make "picked for you" mean something, the product needs a durable model of who you are as a learner, not just a keyword filter. You build that by creating a digital twin from a CV, a LinkedIn profile, or a manual form. It captures your stack, your goals, the domains you care about, your stated preferences, and the honest number of hours you actually have this week. That twin is what turns a firehose of technical content into a shortlist that fits your life.

We call it a twin because that's a more accurate description than "personalization settings." It's not a robotics or physical-systems twin, there's no simulated hardware here, it's a standing representation of a person's learning intent that the system consults every week instead of asking them to re-specify their interests every time. The demo ships with three seeded twins, Engineer, Creator, and Researcher, each with a distinct stack and set of goals, so you can see the difference the twin makes without setting one up yourself. A fourth path, New Twin, lets you build your own from scratch.

What the learner actually sees is deliberately unglamorous: focused launch cards for the week's lessons, a lesson player, a progress tracker scoped to the current track, visible source provenance for every claim, and an archive of past weeks you can revisit. No pipeline dashboard, no job queue in your face. The infrastructure's job is to disappear into a good weekly reading list.

## How our backend and refresh pipeline work

The product is a Next.js learning interface, compiled to static assets, served by a FastAPI backend that owns ingestion, profiling, scoring, and lesson generation.

Ingestion pulls from four keyless public sources, meaning no OAuth flow, no paid tier, no credential that expires while you're not looking: Hacker News, arXiv RSS across five CS categories, Dev.to, and a curated set of YouTube channel RSS feeds. Each source is isolated. If YouTube's feed times out or arXiv changes its RSS shape, that source reports a degraded or failed status and the run continues with whatever the others returned. A partial week beats a blocked week.

Here's the sequence that actually runs, in order:

1. The first authenticated visit after the Monday UTC boundary claims a durable lease in Nebius Object Storage and submits one shared content-prefetch job. That job fetches and normalizes the week's articles and videos, then builds a shared topic-tag snapshot, once for the week, not once per learner.
2. Anyone else who shows up while that's in flight sees a "preparing this week" state instead of triggering a second, redundant fetch. The lease exists specifically to prevent duplicate work.
3. When you click Generate, the API reuses that shared pool instead of re-fetching anything. It filters candidates against your twin's stack, goals, and domains using the tag snapshot, which in practice cuts per-learner LLM calls from around a hundred-plus articles down to twenty or thirty, ranks what's left, writes a personal digest, and authors source-backed learning modules from it. Generate is the explicit trigger for that expensive, profile-specific work, nothing runs it speculatively.

Every one of those model calls happens inside a finite Nebius Serverless AI Job, not a standing endpoint you pay for by the hour. A Job starts vLLM (the inference server that actually runs the model), waits for it to report healthy, does its one job, tagging, scoring, digesting, or lesson-authoring, writes the result to Object Storage, stops vLLM, and exits. GPU billing ends when the process does. The current worker runs `Qwen/Qwen3-0.6B` on a single L40S GPU (`gpu-l40s-a`, `1gpu-8vcpu-32gb`), and our own config comments call it out plainly as the economical tier. We tried leaning toward a heavier model early on and ran into exactly the provisioning and cost friction you'd expect from bigger GPU shapes, one worth avoiding rather than fighting. A small model doing tightly-scoped work beat a big model doing vague work, and finite jobs made that trade cheap to make.

Small also means the model never gets asked to "make me a course." Every stage gets a narrow prompt and a small batch: tagging runs eight articles at a time, comparative ranking runs four, digest preparation runs two, and lesson authoring runs two. Structured JSON output is validated at every stage, so a malformed response degrades to a sane fallback instead of breaking the run.

The artifact trail this produces is durable and, importantly, inspectable:

`sources -> shared weekly snapshot and tags -> profile-aware ranking -> digest -> learning modules -> StackTwin classroom`

Two things fall out of building it this way. First, each article gets checkpointed to storage the moment it's scored. If a run dies halfway through scoring forty articles, the next attempt loads what's already scored and only sends the remainder to the model, so a partial failure never re-pays for work it already did. Second, a completed weekly track isn't a disposable row in a digest table, it's a reusable learning object with its own id, so last week's track can still be shown to a learner while this week's is being prepared, and the archive stays genuinely browsable instead of decaying into dead links.

## Why this belongs in the Nebius Serverless Challenge

This is an AI & ML submission, closest in spirit to the challenge's data-processing and MLOps-style examples: an ingestion job, a scoring job, a generation job, and the storage contract that lets them hand off to each other cleanly.

Nebius isn't a single API call bolted onto the side of this project, it's both the control plane and the data plane. Serverless AI Jobs perform every actual stage of work, tagging, ranking, digesting, authoring, and Nebius Object Storage is what lets those stages coordinate: the shared snapshot, the prefetch lease, per-learner profiles, per-article scoring checkpoints, digests, and finished tracks all live there as plain, readable objects. That's also what makes the reproducibility story concrete rather than aspirational. The repository holds the Job's Dockerfile, the environment configuration, and the commands to build the image, push it to a Nebius Container Registry, and trigger a run:

```bash
curl -X POST "http://127.0.0.1:8000/api/digest/run?user_id=<learner-email>"
```

Another practitioner can follow the README end to end and land in the same place we did, same sources, same batch sizes, same finite-job shape, without needing us to explain a single undocumented step.

The educational payoff of writing this up is the design reasoning itself: share the expensive prefetch, make generation an explicit and billable action instead of an ambient background job, checkpoint intermediate state so failures are cheap, and keep every model call bounded and recoverable. None of that requires a large model, it requires discipline about when GPU time actually gets spent. And the real-world usefulness is plain: a learner gets a focused, personal plan for keeping up with a fast-moving field without spending their evenings doing the curation themselves, which was the entire point.

## Where this leaves us

The weekly-decision tax was never really about a shortage of good material, it was about who does the sorting. StackTwin does the sorting; you still choose what to do with the lesson. It's still evolving, and we'd rather have people poke at the design than take our word for it. Clone the repo, run the pipeline, argue with our batch sizes. #NebiusServerlessChallenge
