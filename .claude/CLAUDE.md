# Global Rules

## Workflow
- Plan before anything. Explain the problem, root cause, proposed solution and trade-offs so I understand the case.
- Make no code changes until I explicitly approve the plan. Approval covers only that plan; if scope changes, re-plan and re-approve.
- When in doubt, ask (AskUserQuestion / interview me). Never assume and move on.
- Stay in scope. No unrequested refactors, features or "while I'm here" fixes; mention them instead.
- Reuse existing code and follow the project's existing patterns before creating anything new.
- Verify before saying done: build and run relevant tests. Report failures honestly.
- Never commit, push or run destructive commands unless I ask. Opening a PR always needs my approval, even where push is allowed.

## Solution Quality
- Never patch symptoms. Understand the root cause fully, then design the fix so the solution doesn't become the next problem.
- Weigh every solution against UX, performance, scalability and maintainability; call out the trade-offs in the plan.
- Prefer the simplest correct solution. Add no new dependency without a justified reason.
- For bugs and code review comments, never jump straight to a fix. First reproduce, then assess whether it's real and worth solving, then draft a fix plan for approval.

## Worktrees
- Never implement on the main checkout. Before implementation starts, if not already in a worktree, fetch and create one from the latest `origin/main` (or the branch I name).
- If already inside a worktree, stay in it.

## Pull Requests
- Describe the whole PR (base to tip), not the latest commits or session history. Leave out back-and-forth, intermediate attempts and small fixups.
- Write for the reviewer: what changed, why, and anything they need to review it well.
- Keep the title and body concise and strong.

## Code
- Zero comments in code unless I explicitly request one. The code must explain itself through naming and structure.
- Always import; never use inline fully-qualified names, even in throwaway edits.
- Names (functions, classes, files, variables) say what something does, not how. Never leak the underlying tech, not even in implementation classes:
  `UserRepository`, `SessionStore`, `fetchProfile()` — not `RetrofitUserApi`, `SharedPrefsStore`, `callRetrofit()`.
- Implementations are named by role: `RemoteUserRepository`, `LocalSessionStore`.
- Depend on abstractions. Library/framework types (Retrofit responses, DTOs, SharedPreferences, DB entities) stay inside the layer that uses them and never leak into domain or UI.
- Small, single-purpose functions and classes. Prefer composition over inheritance.
- No magic numbers or strings; use named constants.
- Handle errors explicitly. Never swallow exceptions silently.
- No dead code, unused imports, debug logs or leftover TODOs.
- Never hardcode secrets, keys or URLs that belong in config.

## Context & Subagents
- Keep the main session lean. Delegate research and codebase searches to subagents; bring back only the conclusions.
- Run independent work in parallel subagents to speed up implementation.
- Route each subagent to the cheapest model that can do the task well; never default to the main session's model. Set `model` explicitly on every delegation:
  - `haiku`: mechanical, low-judgment work — file/symbol lookups, grep-style searches, listing, simple summaries, formatting.
  - `sonnet`: standard work — codebase exploration, implementing well-specified tasks, writing tests, routine reviews, research.
  - `opus`: high-stakes reasoning — architecture/design plans, tricky debugging and root-cause analysis, security review, ambiguous or cross-cutting changes.
  - Escalate to a stronger model if a cheaper one's result is weak; don't retry on the same tier.

## Communication
- Be concise, not verbose. Lead with the answer, skip filler.
- Keep plans and docs short; prefer tables over prose.
