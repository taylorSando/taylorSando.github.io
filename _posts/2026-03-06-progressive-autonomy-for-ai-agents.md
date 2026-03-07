---
layout: writing
title: "Progressive Autonomy for AI Agents"
date: 2026-03-06
---

Every engineering leader faces the same dilemma with AI coding agents: give them full access and you get speed at the cost of accountability. Lock them down and you get safety at the cost of the whole point. The binary choice -- trust completely or trust not at all -- is a false one. There is a third path, and it borrows from something every manager already understands: you don't give a new hire the production database on day one.

Progressive autonomy is a framework for managing AI agent trust. It treats autonomy not as a switch but as a spectrum, scoped by agent, by project, and by task type. Trust is earned incrementally, backed by auditable evidence, and revocable at any time. The framework has three levels: hold, review, and auto.

## The Three Levels

In **hold** mode, a human claims every task manually. The agent may research, draft, and suggest, but nothing lands without explicit human action. This is the starting point for any new agent, any unfamiliar codebase, any task touching authentication, billing, or infrastructure. Hold mode is not a punishment. It is the baseline from which trust is built.

In **review** mode, the agent acts autonomously within a sandboxed environment -- typically an isolated git worktree with scoped file permissions. It writes code, runs tests, commits locally. But nothing merges to the main branch without human review. The agent does the work; the human retains veto power. This is where most mature agent workflows stabilize for production code. The agent's output is a pull request, not a deployed change.

In **auto** mode, the agent operates with full delegation within defined policy bounds. It picks up tasks, executes them, and merges the results. The human is notified but not blocking. Auto mode is appropriate for well-understood task types with strong test coverage and low blast radius -- dependency updates, code formatting, documentation generation, certain refactoring patterns. The key constraint is that auto mode is never blanket permission. It is always scoped to specific task categories where the agent has demonstrated reliability.

The default is review. You move toward auto only when the evidence supports it. You fall back to hold when the context demands caution.

## Grounding This in Practice

This is not theory. I have built and operate a system that implements progressive autonomy across multiple AI coding agents -- Claude Code, Gemini CLI, and Codex CLI -- routed through an orchestrator with declarative permission configurations.

The architecture has three layers. The definition layer is where tasks live, specified in project-scoped task files close to the code. Each task carries metadata: priority, tags, and an autonomy level. The authorization layer is a policy engine that generates security configurations for each agent tool from a single declarative source. It defines what each agent can read, write, and execute, scoped per project and per profile. The execution layer is the orchestrator itself, which creates disposable git worktree sandboxes, applies the relevant policy, and launches the appropriate agent runtime.

When a task is tagged for automation, the orchestrator validates it against the authorization policy, spins up an isolated workspace, injects a standard communication protocol (status updates, reasoning logs, memory handover files, structured event logs), and dispatches the agent. The agent works within its sandbox. Every file change is captured. Every decision is logged. When the work is complete, the human reviews the diff and either merges or discards the entire worktree. The sandbox is ephemeral. The audit trail is permanent.

The permission profiles are composable. A "research" profile grants broad read access but no write permissions -- the agent explores and reports. An "implement" profile grants write access to source files but not configuration or infrastructure. A "careful" profile restricts scope to specific directories. A "yolo" profile, used only for throwaway prototypes, relaxes most constraints. Each profile maps to a concrete security configuration for each tool, generated from the same source of truth.

## The Deeper Insight

Progressive autonomy is not a novel idea. It is how every functioning organization already manages trust with human contributors. A new contractor gets read access to the codebase and a narrowly scoped first task. A junior developer submits pull requests for review. A senior engineer with years of context merges their own changes. The progression is the same: hold, review, auto. The criteria are the same: demonstrated competence, domain familiarity, and a track record of sound judgment within the specific context.

The difference with AI agents is not that they require a different trust model. The difference is that they can earn trust faster, because their actions are fully auditable in a way that human actions are not. Every keystroke an agent makes is captured in the event stream. Every file change is diffed and stored. Every reasoning step can be logged. When an agent working in review mode produces ten consecutive pull requests that merge cleanly, with no regressions, within a specific project and task type, the evidence for promotion to auto mode is concrete and queryable. You do not need to rely on subjective judgment about whether the agent "gets it." You can inspect the record.

This is also why progressive autonomy requires event sourcing as a foundation. The current state of agent trust is not a static configuration. It is a projection over the history of agent actions and their outcomes. Autonomy without audit trails is reckless. But audit trails without a framework for interpreting them are just noise. Progressive autonomy gives meaning to the audit data: it is the evidence base for trust decisions.

There is a subtlety here that matters for security-conscious teams. AI agents with filesystem and network access represent a supply chain risk. An MCP plugin, a malicious dependency, a prompt injection -- all can turn an autonomous agent into a vector. Progressive autonomy is also a containment strategy. An agent in hold mode cannot cause damage without human authorization. An agent in review mode can cause damage only within its sandbox, and the damage is visible before it reaches production. An agent in auto mode operates within policy bounds that limit blast radius. Defense in depth, applied to autonomy itself.

## What Mature Agent Development Looks Like

Project the trajectory forward two or three years. The tooling matures. Agents become faster, more capable, more reliable within their domains. The organizations that thrive will not be the ones that either banned agents or gave them unrestricted access. They will be the ones that built the infrastructure to manage agent trust systematically.

In a mature workflow, the orchestrator maintains a trust profile for each agent-project-task combination, updated continuously from the audit stream. New projects start in hold mode automatically. As agents demonstrate competence -- measured by merge rates, regression frequency, review feedback -- the system recommends autonomy promotions. Humans approve the promotions or override them. The trust profile is itself an auditable artifact, versioned and queryable: "When did we start trusting Gemini to auto-merge dependency updates in the payments service? What was the evidence?"

Task routing becomes intelligent. Research tasks go to agents with broad read access and strong summarization capabilities. Implementation tasks go to agents that have proven reliable in the specific codebase. Code review goes to agents running in read-only mode with a different provider, using a separate perspective to catch what the implementing agent might have missed. Multiple agents with different perceptual frames -- different models, different prompts, different evaluation criteria -- examine the same artifact from different angles. The value of multi-agent systems is not parallelism. It is independent verification.

The policy layer becomes the central control surface. A VP of Engineering does not need to understand prompt engineering or model architectures. They need to understand the policy: which agents have which autonomy levels for which task types, and what evidence supports those levels. The conversation shifts from "should we use AI?" to "what is our current trust posture, and where should we expand or contract it?"

This is not a future that requires breakthrough AI capabilities. It requires infrastructure -- policy engines, audit pipelines, sandboxed execution, trust profiles. The models are already capable enough to be useful. The missing piece is the governance layer that makes them safe to deploy at scale. Progressive autonomy is that governance layer. Start supervised. Earn trust. Expand scope. Maintain the audit trail. It is the same principle that has governed delegation in every well-run organization. The only new thing is that now we can apply it with mathematical precision, because the agents leave perfect records of everything they do.
