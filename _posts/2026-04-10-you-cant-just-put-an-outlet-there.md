---
layout: writing
title: "You Can't Just Put an Outlet There"
date: 2026-04-10
---

You ask the electrician for an outlet on the south wall of the basement. Simple request. He looks at the wall, looks at the ceiling, walks upstairs, comes back down, and says no.

Not "no, it's hard." Just no. Not there.

You do not understand. It is a wall. You have seen outlets on walls. You have seen this exact type of outlet at Home Depot for $3. What is the problem?

The problem is everything you cannot see. That wall has no circuit run to it. The nearest panel is on the opposite side of the house. Running a new circuit means going through the floor joists, which means pulling permits, which means the inspector will see the knob-and-tube wiring in the east wall that has been grandfathered in for forty years. Pulling permits means bringing that up to code. Bringing that up to code means a $15,000 rewire of half the house.

The outlet costs $3. The constraint graph costs $15,000. And you cannot see the constraint graph because you have never had to. You just wanted an outlet.

Software engineering is like this, except the constraint graph is invisible, nobody has a license, and the building inspector only shows up after things catch fire.

## The Bottleneck Shifted

For decades, software development was bottlenecked by code production. You had expensive engineers, you rationed their time, you debated whether to build or buy. The code itself was the scarce resource.

That is over.

LLMs can generate mountains of syntactically correct, functionally plausible code on demand. The bottleneck has shifted, not disappeared. We have automated the craft layer. What remains is the constraint layer. And here is the uncomfortable truth: most organizations, and most engineers, do not know how to work at the constraint layer because they have never had to. The craft layer was always in the way.

Think about what a senior engineer actually does when they are earning their money. They are not typing faster. They are pattern-matching against invisible structure. "We cannot add that queue here because the downstream consumer cannot handle duplicates and we do not have idempotency keys." "That API design will work until you have more than one datacenter." "Caching that makes the UI fast but breaks the audit log." They are reading the electrical grid.

Junior engineers generate code. Senior engineers navigate constraints. LLMs just made everyone a junior engineer.

## The Type of What You Can Do Depends on What Exists

There is a beautiful idea in programming language theory called dependent types. In most type systems, `List<User>` is just a list of users -- the type does not know how many, or which ones. In a dependent type system, the type can encode values, not just shapes. `List[n, User]` is a list of exactly n users, and if you try to write code that assumes a different count, it will not compile.

This sounds academic until you realize: every real system already has dependent types. You just cannot see them. They live in your head, in your runbook, in the tribal knowledge of whoever has been around longest.

The outlet is a dependent type. Its validity depends on the value of everything beneath it -- the circuit, the panel, the service entrance. An outlet hanging off a 15-amp circuit serving six other outlets is syntactically an outlet. Semantically it is a fire waiting to happen.

Software systems are full of these invisible dependencies. A microservice is valid only if the services it calls are available, contract-compatible, and responding within its timeout budget. A database schema migration is valid only if all running application versions can tolerate both the old and new schema simultaneously -- which is a function of your deployment strategy, your rollback plan, and whether you actually tested that. An API endpoint is valid only if its callers have been updated to handle the new error codes you added.

None of this is in the type checker. None of this shows up in the tests, usually. It lives in the constraint graph, which is almost always undocumented, partially understood, and discovered through production incidents.

## Neither Waterfall Nor Vibes-Driven Iteration

Once you see this, you understand why both dominant approaches to software development fail in characteristic ways.

Waterfall fails because it assumes constraints can be fully specified upfront. They cannot. You discover constraints by building. You do not know that caching this field will invalidate that invariant until you have tried it. You do not know that the third-party API has a rate limit that interacts badly with your retry logic until you have hit it. The constraint graph is partially latent -- it only becomes visible when you poke the system.

Pure iteration fails because it treats every decision as reversible. Most are not. Architecture is the set of decisions that are expensive to change. When you iterate without constraint awareness, you accrete technical debt that is not really debt -- it is more like load-bearing walls you did not know you built. You cannot just put an outlet there because last sprint's "quick win" quietly became the thing everything else depends on.

The right approach is neither. It is something closer to constraint-first design, where you treat discovery of constraints as the primary work product. Not discovery before building -- discovery through building, but with deliberate attention to what you are learning, and with explicit documentation of what the system can and cannot accept.

The real artifact is not the code. The code is almost incidental. The real artifact is the emergent constraint graph.

## The Ensemble Problem

There is an insight I keep coming back to, originally from Ian Duncan: the unit of correctness is the deployment ensemble, not the program.

Your type system checks one version of your code. Production runs many. At any given moment you might have three versions of your API behind a load balancer, two versions of your mobile app in the wild, and a background job running code that was deployed last month. The correctness guarantee your compiler gives you is a guarantee about a single snapshot. The system is a film, not a photo.

This is why distributed systems are hard in a way that has nothing to do with distributed systems. Even a monolith deployed to a single server is a distributed system across time. The database schema has to be valid under every version of the application that might be running against it. That is a constraint that most type systems do not model at all.

The electrician understands this intuitively. The code he writes -- the wire he runs -- has to be valid under every subsequent modification anyone might make to the panel. He leaves slack in the wire. He labels circuits. He does not route things through cavities that might be opened later. He is writing code for the ensemble, not the snapshot.

## Rust Saw It First

The clearest evidence that the industry is waking up to this is Rust.

Memory management is the purest form of constraint engineering. When you allocate memory, you create a resource with a lifetime. Every subsequent operation on that memory is valid only if the resource is still alive, still uniquely owned, still not borrowed elsewhere. These are not runtime constraints, they are structural constraints -- properties of the shape of your program.

C and C++ left these constraints invisible. They existed -- you violated them, you got undefined behavior, you got a CVE. The constraints were always there. You just could not see them.

Rust made them visible. The borrow checker is a type system for lifetimes. It refused to let you put an outlet where there was no circuit. Programmers complained bitterly. They still complain. "Fighting the borrow checker" is a meme. But what they are actually fighting is the constraint graph being made explicit for the first time. The constraints were not new. The visibility was.

Every difficult type system is secretly just a constraint graph made legible. Every "fighting the type system" complaint is secretly a constraint that was previously invisible and is now surfaced. The anger is real. So is the correctness.

## The Functor Problem

Here is where it gets interesting for systems design.

Category theory has a concept called a functor. A functor is a mapping between categories that preserves structure. If A relates to B in one category, then F(A) relates to F(B) in another. The objects change; the relationships are preserved.

Why does this matter? Because the dream of every architecture rewrite is that you can change the implementation while preserving the behavior. "We will migrate from JavaScript to C++ when we hit the performance wall." "We will move from a monolith to microservices when we need to scale." These are claims about functoriality -- that there exists a mapping from the old system to the new one that preserves the relationships you care about.

The constraint graph is what makes a system transposable or not. If your JS system and your hypothetical C++ system have the same constraint graph -- the same invariants, the same valid sequences of operations, the same failure modes -- then the migration is a functor. You changed the objects, the implementation; you preserved the morphisms, the constraints.

If they do not have the same constraint graph, the migration is not a functor. You are not rewriting the system. You are building a different system that happens to have the same name.

This is why "just rewrite it in X" usually fails. The new system has different performance characteristics, yes, but it also has subtly different constraints -- different failure modes, different consistency guarantees, different operational behavior under load. And nobody documented the constraint graph of the old system, so nobody can check whether the new one preserves it.

## LLMs Can Generate Code. They Cannot Discover Constraints.

Here is the crux of it.

An LLM trained on code has seen millions of outlets. It knows what an outlet looks like. It knows how to wire one. It can write you the code for an outlet in seconds. What it cannot do -- what no LLM can currently do -- is know whether the circuit exists.

Code is the object level. The constraint graph is the morphism level. LLMs are extraordinary at generating objects. They are much weaker at discovering morphisms, because morphisms are defined by relationships, and relationships are discovered through observation of system behavior over time, not through pattern-matching on static text.

This creates a specific failure mode I am watching spread through the industry: teams adopt LLM-assisted development, velocity goes up dramatically, constraint debt accumulates invisibly, and then something breaks in production in a way that is confusing because all the code looks correct. All the outlets are wired beautifully. The grid is not there.

The solution is not to distrust LLMs. It is to be clear about what you are asking them to do. "Generate code that satisfies these constraints" is a very different prompt than "write me some code that does this thing." The first requires you to know your constraints. That is harder than it sounds. Most teams do not have them written down. Most engineers could not tell you what they are without a whiteboard and forty-five minutes.

## What Constraint Engineering Actually Looks Like

I am not arguing for big upfront design. I am arguing for constraint-first thinking, which is different.

**Make the constraint graph explicit.** Write down what your system cannot do, not just what it does. Document the valid orderings. Document what must be true before you can run a migration. Document what would break if you changed X. This is unglamorous work. It is also the most durable work, because it is the only thing that makes the system navigable to the next team -- or to your future self, six months from now, when you have forgotten everything.

**Treat production incidents as constraint discoveries.** Every incident is the system telling you about a constraint you did not know existed, or knew about but failed to enforce. The incident report that matters is not "what went wrong" but "what does this tell us about the shape of valid states the system can be in?"

**Evaluate decisions at the constraint level, not the code level.** "Should we use Redis or Postgres for this?" is a constraint question, not a code question. Redis gives you speed and gives up durability guarantees. Postgres gives you consistency and gives up flexibility at scale. The code is almost irrelevant. The question is which constraint envelope fits your actual problem.

**Use LLMs for code generation; use humans for constraint discovery.** This is the right division of labor. Not "LLMs do the easy stuff, humans do the hard stuff" -- that framing is backwards and condescending. More precisely: LLMs work at the object level, humans work at the morphism level. The electrician who can read the grid is not replaceable by someone who can wire faster. Faster wiring makes the grid problem more urgent, not less.

## The Grid Is Real

I want to close where I started.

The electrical grid is real. It exists. It was built by thousands of decisions over decades. The outlet you want either fits within it or it does not. This is not a limitation of your electrician's imagination. It is a fact about the structure of the world.

Software systems have grids too. The constraints are real. They were built by thousands of decisions, some intentional, most accidental. The feature your PM wants either fits within them or it does not. This is not a limitation of your engineers' skill. It is a fact about the structure of your system.

The difference between software and electrical work is that in software, the grid is mostly invisible. You can, in fact, put an outlet there. It will work, for a while. The wrongness takes longer to surface. But it surfaces.

LLMs make the outlet cheaper. They do not make the grid optional.

The engineers who thrive in the next decade are the ones who can read the grid -- who can look at a request and see not just the code it requires, but the constraints it touches, the invariants it must preserve, the prior decisions it depends on. They are not the fastest coders. They never were. They are the ones who know you cannot just put an outlet there, and can explain exactly why.

---

*Taylor Sando writes about systems, constraints, and the gap between how software is supposed to work and how it actually does.*

*If this landed, the best thing you can do is share it with the engineer on your team who has been trying to explain something like this for years and could not find the words.*
