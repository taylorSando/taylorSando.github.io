---
layout: writing
title: "Tests Are Agents With Beliefs"
date: 2026-03-06
---

A passing test tells you nothing. Or rather, it tells you something so narrow that mistaking it for safety is one of the most expensive errors in software engineering. A passing test means: one agent's belief about the system was not contradicted *this time*, under *these conditions*, against *this version* of reality. That is not the same as "the system works." Silence is not the same as consent.

We talk about tests as if they are objective arbiters. The test suite is green, so the code is correct. But correctness relative to what? The test encodes an assumption someone made at some point in time about what the system should do. That assumption might have been right when it was written. It might still be right now. But the test cannot tell you whether it is right. It can only tell you whether the system still conforms to the belief it encodes.

This distinction matters more than it sounds like it should.

## Three Agents, No Ground Truth

Consider the three artifacts that are supposed to keep a software system honest: the spec (what someone intends), the implementation (what the code does), and the tests (what we check). Standard CI assumes a tidy relationship: the implementation satisfies the spec, the tests verify the implementation, and therefore the tests transitively confirm the spec. If the tests pass, everything is aligned.

In practice, these three things are independent agents with independent models of reality, and they can all diverge from each other without anyone noticing.

The spec lives in someone's head, or in a document that stopped being updated two sprints ago. The implementation evolved under pressure, accumulating workarounds and incidental behaviors that nobody wrote down. The tests were written against an older version of both and have been copy-pasted forward ever since.

When a test fails, there are at least three possible explanations. The implementation broke and the test correctly caught it. The implementation intentionally changed and the test is now stale. Or the test was always wrong and only appeared to work because the old implementation happened to satisfy a badly-specified assertion. You cannot distinguish these cases by looking at the test. You need to know *intent*, and intent lives outside the code.

A passing test has the same ambiguity, just quieter. The test passes. Is it because the system is correct? Or because the test is not checking the thing that changed? A test that passes when it should fail is strictly more dangerous than a test that fails, because failure at least demands attention. A hollow pass is invisible.

## Belief Drift Is the Default

This is not just a testing problem. It is the fundamental problem of distributed systems, restated.

Every service in a distributed architecture operates with a partial, possibly stale model of its neighbors. Service A believes Service B returns JSON with a field called `id`. Service A believes B responds in under 200 milliseconds. Service A believes B is idempotent. These are not facts about B. They are beliefs that A holds, encoded in A's client code, retry policies, and timeout configurations.

When B deploys a new version that renames `id` to `identifier`, A's belief becomes wrong. If the schema change is caught by a type system or contract test, the drift is surfaced before it causes damage. But semantic drift -- where the types still match but the meaning changes -- is invisible to every tool in the standard pipeline. B's endpoint still returns an integer, but it now represents cents instead of dollars. A's belief about B was never written down as a formal contract. It was implicit in the code. And now it is wrong.

Production bugs are not, in the general case, "code errors." They are divergences between one agent's model of another agent and that agent's actual behavior. The circuit breaker that trips is not a network failure. It is the moment when A's confidence in its model of B drops to zero. Configuration is not a static file. It is an agent's initial belief state. A deployment is not copying files to a server. It is replacing one agent with another agent that has a different model of the world.

Mark Burgess's Promise Theory captures this precisely: agents do not control each other. They make promises (interfaces, SLAs, contracts) and assess whether those promises were kept. A service call is not an RPC. It is a request for another agent to fulfill a promise, evaluated against the caller's belief about what that promise entails.

## The LLM Inherits Your Stale Beliefs

This framing becomes urgent when AI agents enter the picture. When an LLM reads your codebase to make changes, it inherits every implicit belief embedded in the existing code. It pattern-matches: every other service uses this retry policy, so I will too. Every other handler validates input this way, so I will too. The LLM cannot distinguish between intentional design decisions that must be preserved, incidental implementation details that could be anything, workarounds for bugs that have since been fixed, and dead assumptions that nobody remembers making.

This is not a flaw in the LLM. This is exactly what a new hire does. The existing codebase is simultaneously the best documentation of the system and a source of systematically misleading signals. The LLM, like the new hire, builds a model of the system from the artifacts it can observe. That model is partial and possibly wrong in ways that are not detectable from the artifacts alone.

The N-to-N+1 transition is where this bites. You know what the code does (read it). You know what you want it to do (new requirement). What you do not know is the full blast radius of bridging one to the other. Regressions are not caused by a lack of specs. They are caused by incomplete models of dependencies. You change function A, not realizing module B depends on a side effect of A's old behavior. The test for B still passes, because the test was checking B's output format, not B's dependency on A's side effect. The belief encoded in the test was always narrower than the actual contract.

## What Changes If You Take This Seriously

If tests are agents with beliefs, then "the tests pass" is not a meaningful quality signal on its own. What you actually want to know is: are the beliefs encoded in the tests still coherent with the beliefs encoded in the spec and the beliefs encoded in the implementation? Coherence across agents, not truth within any single agent.

This suggests several concrete shifts.

First, tests should be ordered by drift resistance. A property-based test that asserts "for all inputs, the output is sorted" survives almost any implementation change. A unit test that asserts "given input 7, the output is 42" breaks the moment someone changes the constant. Invariant beliefs drift slowly. Example beliefs drift fast. Invest accordingly.

Second, co-evolution should be tracked. If a module has high code churn but its tests have zero edits in the same changeset, something is wrong. Either the tests are robust enough to cover the changes (possible but unlikely for targeted unit tests), or the tests are stale and nobody noticed because they kept passing. The ratio of implementation change to test change is a measurable proxy for belief drift.

Third, a passing test against a changed dependency should be treated as suspect, not reassuring. When Service B deploys a new version, every test that encodes beliefs about B is potentially invalid regardless of whether it passes or fails. The pass/fail result is only meaningful after someone confirms that the test's beliefs are still calibrated against the current version of reality.

Finally, the test suite should not be the sole source of confidence. Runtime evidence -- production traffic patterns, error rates, latency distributions -- is another agent with its own beliefs, and those beliefs are calibrated against reality in a way that pre-deployment tests never can be. The test says "I believe B responds in under 200 milliseconds." Production says "B responded in 850 milliseconds fourteen times in the last hour." When these agents disagree, production wins.

None of this means tests are useless. It means they are partial. A test is one agent's snapshot of one belief at one point in time. Treating that snapshot as ground truth is the error. The system is not correct because the tests pass. The system is *coherent* when the spec, the implementation, the tests, and production all agree. And coherence, unlike a green CI badge, is something you have to continuously earn.
