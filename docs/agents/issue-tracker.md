# Issue Tracker: GitHub Issues

Agent workflow issues for this repo use GitHub Issues.

## Conventions

- Create issues with `gh issue create`.
- Read and search issues with `gh issue view` and `gh issue list`.
- Update labels, milestones, and status through `gh issue edit` or the GitHub UI.
- Keep implementation notes in the issue or PR, not only in local scratch files.

When an imported skill says "issue", "ticket", or "publish to the issue tracker", use GitHub Issues unless the user explicitly asks for local notes.

## Issue Body Shape

For autonomous work, write issue bodies as durable agent briefs:

```markdown
## Agent Brief

**Category:** bug / enhancement / maintenance / architecture
**Summary:** one-line behavior-level summary

**Current behavior:**
What happens now.

**Desired behavior:**
What should happen after the issue is complete.

**Acceptance criteria:**
- [ ] Concrete observable criterion

**Verification:**
- Command or deterministic feedback loop to run

**Out of scope:**
- Adjacent work the agent should not touch
```
