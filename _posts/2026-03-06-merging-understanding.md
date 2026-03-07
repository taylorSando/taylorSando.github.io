---
layout: writing
title: "Merging Understanding Is Harder Than Merging Code"
date: 2026-03-06
---

Git solves code convergence. CRDTs solve data convergence. But when two agents -- human or AI -- work on the same system with divergent mental models, there is no merge algorithm. We have extraordinary tools for reconciling textual differences and resolving conflicting edits to shared data structures, yet we have almost nothing for the problem that actually causes production failures: two participants in a system believing different things about how it works.

This is not an engineering problem waiting for a better tool. It is an epistemic problem, and it has been with us far longer than software.

## The Partial Model Problem

Every participant in a software system -- every developer, every AI coding assistant, every service, every test suite -- operates with a partial model of the whole. Nobody holds the complete picture. This is not a bug in how teams are organized. It is a fundamental consequence of the fact that complex systems exceed any single agent's capacity to represent them.

The trouble starts when these partial models drift apart. Two developers refactor the same module, each carrying a different assumption about why the code exists in its current form. One believes the convoluted error handler is a mess left by a previous team. The other knows it is a deliberate workaround for a vendor API that occasionally returns malformed responses under load. Both developers write clean, well-tested code. The git merge succeeds without a conflict. The CI pipeline passes. And the system breaks in production three weeks later, on a Saturday, when that vendor API misbehaves and the workaround is no longer there.

The merge algorithm did its job perfectly. The problem was never in the code. It was in the space between two people's understanding of the code.

## Belief Drift as the Root Cause

I have come to think that most production incidents are not code errors in the traditional sense. They are manifestations of belief drift -- a gradual divergence between what one part of the system assumes about another. A service expects idempotent behavior from its dependency, but a recent refactor introduced a subtle side effect. A test suite asserts that a particular endpoint returns a specific shape, but nobody updated the test when the contract evolved, so the test passes against behavior that no longer reflects intent. The test is not wrong, exactly. It is stale. It encodes a belief about a version of reality that no longer exists.

This framing -- agents with beliefs rather than components with specifications -- comes from epistemic logic, a branch of philosophy concerned with who knows what and who knows that others know. In distributed systems terms, we might represent an agent's model of its neighbor as a tuple of schema expectations, behavioral promises, and a confidence score. When the confidence is high and the model is current, things work. When the model drifts -- because deployments happened, because teams rotated, because an AI agent modified something without understanding the implicit contract -- you get the kind of failure that no amount of type checking can prevent.

The formal term for this gap is the "variety gap," borrowed from control theory: the point where the complexity of the running system exceeds the complexity of the controller's model of it. In software, the controller is whoever or whatever is making the next change.

## Why This Is Not a Metaphor

My background is in psychology, not computer science. I spent years studying how humans form models of other minds -- what developmental psychologists call theory of mind, and what epistemologists formalize as orders of intentionality. I am thinking about what you are thinking about what I am thinking. There are limits to this recursion. For some people it bottoms out at the first order. For others it extends to four or five. The capacity to model other minds is not just a social skill; it is the cognitive substrate for coordinating action in any multi-agent system.

When I moved into software engineering, I did not leave this behind. I found the same structures everywhere. A service calling another service over HTTP is not making a remote procedure call in any meaningful sense. It is acting on a belief about what the other service will do. A timeout is not a connection error; it is the moment an agent's belief in another agent's availability drops to zero. Configuration is not a static file; it is an agent's initial belief state. A deployment is not copying files; it is replacing an agent with one that holds a different model.

This is not analogy. It is the literal application of the same formal frameworks -- BDI (Belief-Desire-Intention) architectures, promise theory, epistemic logic -- to a domain where they apply directly. Mark Burgess's promise theory, developed for infrastructure management, makes this explicit: an agent cannot control another agent. It can only assess whether the other agent kept its promise. Every microservice interaction is an exercise in belief management under uncertainty.

## The AI Agent Amplification

The rise of AI coding agents makes this problem dramatically worse. An LLM reading your codebase treats existing code as authoritative guidance, the same way a new hire would. But it cannot distinguish between an intentional design decision that must be preserved, an incidental implementation detail that could be anything, a workaround for a bug that has since been fixed, and a dead assumption that nobody remembers making. It pattern-matches: every other service uses this pattern, so I will too -- even if the pattern is wrong. The existing code is simultaneously the best documentation available and a source of systematically misleading signals.

When two AI agents work on the same codebase in parallel, each builds its own model of the system from the code it reads. These models diverge immediately. The git merge might succeed, but the underlying assumptions are incompatible. How do you merge divergent agent models? I do not have an answer. Nobody does. But naming the problem precisely is the first step toward solving it.

## What Would a Solution Look Like?

If merging understanding is the hard problem, then any serious approach must make understanding explicit and comparable -- not just code.

First, decision records need to be captured as event streams, not written after the fact as ceremony. Manual architecture decision records fail because nobody writes them. The alternative is to capture raw material -- session transcripts, commit messages, chat logs, meeting recordings -- and derive structured decisions on query. You never write a decision record. You ask "why did we choose polling over WebSockets?" and the system synthesizes an answer from raw evidence. This is event sourcing applied to organizational knowledge: state derived from events, not stored directly.

Second, we need something like a belief registry. Not just a service registry that lists addresses, but a registry where each component publishes its expectations of its dependencies. When a new version deploys, the system checks: does the new version of service B satisfy the recorded beliefs of service A? If not, the deployment is rejected before drift enters production. Consumer-driven contract testing approaches this, but only at the schema level. The deeper challenge is capturing semantic expectations -- not just "this endpoint returns JSON with a field called id" but "this endpoint is idempotent" or "this error code means retry is safe."

Third, and most difficult, we need to treat every participant in the system -- human, AI agent, test suite, monitoring system -- as an epistemic agent with an explicit, queryable model. When models disagree, that disagreement should be surfaced as a first-class artifact, not discovered as a production incident. The gap between what agent A believes about agent B and what agent B actually does is not a bug to be fixed. It is the central quantity to be measured, tracked, and minimized over time.

None of this exists yet in a usable form. We have pieces -- contract testing, observability, decision logs, CRDTs -- but no unified framework for treating understanding as a convergence problem on par with code or data. The tools we have built for merging text are extraordinary. The tools we have built for merging minds are almost nonexistent. Given that the latter is where the actual failures live, this seems like a gap worth closing.
