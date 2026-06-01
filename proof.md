---
layout: writing
title: "Six-Month Build Log"
permalink: /proof/
excerpt: "A git-mined record of the last six months: 11,000+ commits across 30+ repositories — control-plane infrastructure, Sitelayer, Hockeypedia, and the systems around them."
---

This is the long version of "what have you actually built." Every figure below is mined directly from git history — commits since December 2025 — across more than thirty active repositories. The short version of the work lives on the [home page](/); this page is the evidence behind it.

The throughline is a single operator running a self-hosted **control plane** that schedules, supervises, and audits a fleet of coding and research agents — and then using that control plane to ship real products on top of it.

<div class="proof-stats">
  <div class="proof-stat"><div class="proof-stat-num">11,000+</div><div class="proof-stat-label">Git commits</div></div>
  <div class="proof-stat"><div class="proof-stat-num">30+</div><div class="proof-stat-label">Active repositories</div></div>
  <div class="proof-stat"><div class="proof-stat-num">8+</div><div class="proof-stat-label">Live applications</div></div>
  <div class="proof-stat"><div class="proof-stat-num">6</div><div class="proof-stat-label">Self-hosted machines</div></div>
  <div class="proof-stat"><div class="proof-stat-num">1</div><div class="proof-stat-label">Operator + agent fleet</div></div>
</div>

Public and deployed work is described in full. Private systems are described at an architecture level — no secrets, hostnames, or credentials. Four cloned/forked repositories with no original engineering were checked and excluded.

## Flagship Systems

<div class="lab-grid">
  <div class="lab-card">
    <div class="lab-card-tag tag-violet">Agent Infrastructure</div>
    <h3>Control Plane</h3>
    <div class="proj-commits">~5,976 commits &middot; private</div>
    <p>A self-hosted AI orchestration monorepo: a Go/Postgres scheduler, a Chrome browser-automation extension, a multi-agent fleet manager, and a React operator dashboard that route work across Claude, Codex, Gemini, and Antigravity CLI runners. Ships a merge-queue lander with an atomic verify-gate, a counsel-of-models decision system, Ed25519 component auth, a Postgres-backed personal event log, 27 architecture decision records, and 324+ migrations.</p>
  </div>
  <a class="lab-card" href="https://sitelayer.sandolab.xyz" target="_blank" rel="noopener">
    <div class="lab-card-tag tag-green">Product &middot; live</div>
    <h3>Sitelayer</h3>
    <div class="proj-commits">~1,302 commits &middot; sitelayer.sandolab.xyz</div>
    <p>A full bid-to-closeout construction-operations platform for exterior cladding contractors, hardened from prototype to production in roughly two months: blueprint PDF takeoff on a PDFium canvas, AI quantity capture, 15 deterministic workflow state machines, row-level security, QuickBooks OAuth sync with HMAC webhooks, and immutable-image CI/CD with 136 sequential migrations.</p>
  </a>
  <a class="lab-card" href="https://hockeypedia.org" target="_blank" rel="noopener">
    <div class="lab-card-tag tag-green">Sports Analytics &middot; live</div>
    <h3>Hockeypedia</h3>
    <div class="proj-commits">~735 commits &middot; hockeypedia.org</div>
    <p>A production NHL intelligence platform built from scratch to live in one quarter: a 14-adapter Go ingest pipeline, six materialized views (one dropped a query from 3.7s to 1ms), a trade-lineage graph, a fantasy team-builder with live cap math, 10 systemd timers, and a Playwright visual-regression harness with a GPU review lane.</p>
  </a>
</div>

## Agent Infrastructure

The control plane was deliberately decomposed into standalone, versioned repositories in a May 2026 extraction and governance pass.

<div class="lab-grid">
  <div class="lab-card">
    <div class="lab-card-tag tag-violet">Operator UI</div>
    <h3>console-ui</h3>
    <div class="proj-commits">741 commits &middot; private</div>
    <p>An operator dashboard grown from nothing to 48 tab views (~21k lines of TSX): dispatch, fleet telemetry, a live kanban board, research, quota ledger, and ontology — with a full TypeScript-strict rollout and automated boundary-doc enforcement.</p>
  </div>
  <div class="lab-card">
    <div class="lab-card-tag tag-violet">Runtime</div>
    <h3>agent-cli</h3>
    <div class="proj-commits">159 commits &middot; private</div>
    <p>The multi-agent runtime: steerer fan-out, a watchdog that detects and unsticks frozen terminal panes, a 10-hook lifecycle framework, OpenTelemetry across four CLI runtimes, and per-account credential isolation.</p>
  </div>
  <div class="lab-card">
    <div class="lab-card-tag tag-violet">Bridge</div>
    <h3>browser-bridge-sidecar</h3>
    <div class="proj-commits">127 commits &middot; semi-public</div>
    <p>A standalone Fastify sidecar bridging a Chrome extension to the control plane: tab-lease APIs, a capture pipeline with HMAC ingest, per-dispatch research witnesses, and a versioned compatibility matrix.</p>
  </div>
  <div class="lab-card">
    <div class="lab-card-tag tag-violet">Fleet</div>
    <h3>fleet-ops</h3>
    <div class="proj-commits">125 commits &middot; semi-public</div>
    <p>Deploy and topology orchestration for a six-box fleet: a canonical topology config, an eleven-repo release-train catalog, ordered multi-phase deploys, health monitoring with failover, and deploy-pairing gates.</p>
  </div>
</div>
<p class="also-note">Also in this cluster: control-plane-suite (67), llm-accounts (47), operator-types (22, a shared TypeScript contract package used by nine repos), capture (32), emacs-context (11), operator-sdk (9), research-pipeline (8).</p>

## Products

<div class="lab-grid">
  <a class="lab-card" href="https://sandolab.xyz" target="_blank" rel="noopener">
    <div class="lab-card-tag tag-green">Public &middot; live</div>
    <h3>Sandolab</h3>
    <div class="proj-commits">280 commits &middot; sandolab.xyz</div>
    <p>The public lab site: ten Canvas/WebGL ecosystem simulations (each on its own subdomain), a Spotify playlist manager, a live multi-model debate surface, an ops dashboard, and a visual-regression harness wired to the observability substrate.</p>
  </a>
  <a class="lab-card" href="https://chess.sandolab.xyz" target="_blank" rel="noopener">
    <div class="lab-card-tag tag-green">Self-hosted &middot; live</div>
    <h3>Chess Trainer</h3>
    <div class="proj-commits">185 commits &middot; chess.sandolab.xyz</div>
    <p>A chess trainer built in 18 days: a pool of four warm Stockfish engines, multi-model LLM move coaching with a response cache, an adaptive puzzle picker, and a sandboxed system service with backup hooks.</p>
  </a>
  <div class="lab-card">
    <div class="lab-card-tag tag-green">Pilot</div>
    <h3>chat-pool</h3>
    <div class="proj-commits">49 commits &middot; private</div>
    <p>A self-hosted multi-user AI chat service for a small trusted group, with per-user spend caps enforced at the proxy and a subscription-ledger / route-decision API.</p>
  </div>
  <div class="lab-card">
    <div class="lab-card-tag tag-green">Early pilot</div>
    <h3>community-platform</h3>
    <div class="proj-commits">10 commits &middot; private</div>
    <p>An early community-logistics co-op pilot with a price-intelligence module to aggregate household demand and negotiate wholesale pricing.</p>
  </div>
</div>

## Learning Systems

<div class="lab-grid">
  <div class="lab-card">
    <div class="lab-card-tag tag-cyan">Study coach</div>
    <h3>learning-overlay</h3>
    <div class="proj-commits">190 commits &middot; private</div>
    <p>A native desktop HUD and Go service (~33,600 lines) that turns activity into a real-time study coach, with a target-run lifecycle engine, a proof-capture pipeline, and importable schema packages.</p>
  </div>
  <div class="lab-card">
    <div class="lab-card-tag tag-cyan">Proof tooling</div>
    <h3>QEDviz</h3>
    <div class="proj-commits">171 commits &middot; public</div>
    <p>LLM-assisted math-proof visualization: parse, then Lean 4 formal verification, SymPy computation checking, and Manim rendering, with a PDFium reader and cloud annotation sync. Concluded with a clean, deliberate public deprecation.</p>
  </div>
  <a class="lab-card" href="https://learn.sandolab.xyz" target="_blank" rel="noopener">
    <div class="lab-card-tag tag-cyan">Workbench &middot; live</div>
    <h3>learn</h3>
    <div class="proj-commits">105 commits &middot; learn.sandolab.xyz</div>
    <p>A standalone learning workbench with eleven routes — a daily aggregator, cross-device PDF resume, spaced recall, a glossary dependency graph, and an ontology atlas — under strict state-machine discipline.</p>
  </a>
</div>

## Attention &amp; Observability

A personal event log that correlates what the operator says, sees, and does into one queryable stream.

<div class="lab-grid">
  <div class="lab-card">
    <div class="lab-card-tag tag-cyan">Voice</div>
    <h3>voice-tools</h3>
    <div class="proj-commits">83 commits &middot; private</div>
    <p>An always-on speech-to-text daemon writing signed observations, an attention-window rendezvous protocol with a race-safe guard, and a voice-command reactor that fires local handlers.</p>
  </div>
  <div class="lab-card">
    <div class="lab-card-tag tag-cyan">Screen</div>
    <h3>screen-capture</h3>
    <div class="proj-commits">34 commits &middot; private</div>
    <p>Continuous multi-monitor capture with hot/cold tracks, focus tracking, a local API, and a nightly AI archive-captioner running on a local GPU.</p>
  </div>
  <div class="lab-card">
    <div class="lab-card-tag tag-cyan">Space</div>
    <h3>sando-tracker</h3>
    <div class="proj-commits">11 commits &middot; private</div>
    <p>Physical-space tracking that fuses UWB positioning, IMU head orientation, and webcam face detection to infer which monitor has the operator's attention.</p>
  </div>
</div>

## Game Systems

<div class="lab-grid">
  <a class="lab-card" href="https://winwar.sandolab.xyz" target="_blank" rel="noopener">
    <div class="lab-card-tag tag-rose">Engine &middot; live</div>
    <h3>WinWar</h3>
    <div class="proj-commits">285 commits &middot; winwar.sandolab.xyz</div>
    <p>An engine-first strategy game where the engine owns all gameplay semantics and the UI is a pure projection: four rules profiles, fog-of-war commute invariants enforced in CI, an AI tournament balancer, and ten watchable AI-vs-AI scenarios — plus a 451-error type-strictness overhaul and a full accessibility pass.</p>
  </a>
</div>

## Research &amp; Ontology

<div class="lab-grid">
  <div class="lab-card">
    <div class="lab-card-tag tag-purple">Knowledge base</div>
    <h3>org</h3>
    <div class="proj-commits">105 commits &middot; private</div>
    <p>The operator knowledge substrate: a goal registry, a four-layer decision hierarchy, structured project briefs, and a research flywheel that evaluated 500+ topics in the window.</p>
  </div>
  <div class="lab-card">
    <div class="lab-card-tag tag-purple">Ontology</div>
    <h3>digital-ontology</h3>
    <div class="proj-commits">47 commits &middot; private</div>
    <p>A 149-document corpus defining a layered ontology of digital life, with a typed abstraction backbone, a CLI validator, and a formal category-theoretic fragment typing the event pipeline.</p>
  </div>
</div>

## Tooling &amp; Fleet Backbone

<div class="lab-grid">
  <div class="lab-card">
    <div class="lab-card-tag tag-violet">Dotfiles</div>
    <h3>dotfiles</h3>
    <div class="proj-commits">164 commits &middot; private</div>
    <p>The operational backbone of the fleet: profile-isolation-aware agent hooks, steerer autolaunch, capture/transcription system units, git push guards, and cross-agent session markers.</p>
  </div>
  <div class="lab-card">
    <div class="lab-card-tag tag-violet">Editor</div>
    <h3>spacemacs-config</h3>
    <div class="proj-commits">107 commits &middot; private</div>
    <p>Custom editor layers integrating the control plane: custom org-link types, shell lifecycle event forwarding, attention-window binding, and a live key-coach HUD.</p>
  </div>
  <div class="lab-card">
    <div class="lab-card-tag tag-violet">Extension</div>
    <h3>misc-plugin</h3>
    <div class="proj-commits">11 commits &middot; private</div>
    <p>A clean, security-conscious first-party Chrome extension (playback speed, voice-repair EQ, per-site hotkeys, per-machine identity) with zero external network calls.</p>
  </div>
</div>

<div class="focus-note">
  <p><strong>How these numbers were produced.</strong> Each repository was analyzed with <code>git log --since=2025-12-01</code> for commit counts, date ranges, and the subsystems that actually shipped. Most repositories are solo work or work on the operator's own branches reviewed and merged by him. Some private repositories — financial records and scraping tools — are intentionally omitted entirely.</p>
</div>

<div class="links" style="margin-top:2rem;">
  <a href="mailto:consulting@taylorsando.com?subject=Control-plane%20review" class="btn btn-cta">Start a control-plane review</a>
  <a href="{{ "/work/" | prepend: site.baseurl }}" class="btn">Working with me</a>
  <a href="https://sandolab.xyz/proof" class="btn btn-outline">The same log on Sandolab</a>
</div>
