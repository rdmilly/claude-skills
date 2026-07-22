---
name: session-protocol
description: The mandatory open/close ritual for every Millyweb infrastructure work session (Mesh Controller, Helix, MemBrain, fleet, or any /opt/projects repo). Use this skill at the START of any session that touches infrastructure, code on the VPSes, or the controller — including casual openers like "let's work on the controller", "continue where we left off", "pick up the migration", or resuming after a crash. Also use it whenever the user says "checkpoint", "wrap up", "close the session", "save our work", "update the docs", or when the conversation is approaching context limits. If a session involves mesh tools and this skill hasn't been consulted yet, consult it.
---

# Session Protocol

The ritual that guarantees no work, decision, or lesson exists only inside a chat that can die. Born from real losses: 371 uncommitted lines shipping in a live image (2026-07-15), a roadmap designed in-chat and never persisted (2026-07-14).

## Prime directive

**If the chat ended right now, nothing would be lost.** Every rule below serves that sentence.

## 1. Session start (run in this order, before real work)

If `/api/session/briefing` exists (selfdoc card): one call returns state + git status + live containers + active scars + recent audit events. Read it, confirm the next-first-action with the user, go.

Until then, manual:
1. `workdocs_read projects/memory/handoff.md` — read the `## CURRENT STATE` section as truth; treat any UPDATE blocks below it as history only.
2. `helix_search_conversations "recent work active projects"` — what recent sessions did.
3. `scar_query(project)` — active high-severity scars for the repo you're touching. Also `scar_query(anchor)` before working any specific file or boundary.
4. If resuming interrupted work: `git status` on the repo FIRST — uncommitted work on a session branch is the most likely casualty of the last crash.

Do not skip steps because the user's opener sounds casual. "Let's fix the preheater" still gets the ritual — abbreviated is fine (one combined read pass), skipped is not.

## 2. During work

- **Commit the moment it's deployable.** Never leave a live image built from an uncommitted tree. Session branches: `session/YYYY-MM-DD-<repo>`.
- **Buffer intelligence as you go (2026-07-22).** `POST /api/v1/session/buffer` the moment a decision is made or a boundary bites — non-blocking, no downstream routing. A chat that dies mid-session then still leaves its intelligence server-side. See §3 step 4.
- **Checkpoint every ~15 tool calls** and before any complex multi-step operation: commit + one-line handoff append if state changed materially.
- **Docs ship with the change (R7).** Code change → SDD/views updated in the same commit or same session. Never "we'll document later."
- **Scar immediately.** Bleed on a non-obvious boundary (API that lies, tool limit, env quirk) → `scar_record` in the same turn, not at close.
- **Write before you defer.** About to ask the user a question or start a long operation? Persist what you already know first.
- **Verify writes by read-back.** Never trust a returned SHA or success flag (hollow-success scar, S16). mesh_file_write only for controller/provisioner bind-mounted paths; mesh_exec heredoc everywhere else. `mesh_file_edit` is NOT supported on vps1 (agent-local file verbs pending, ADR-002) — use mesh_exec there.
- **STOP on errors.** Explain, present options, wait — don't auto-pivot. One diagnostic retry is fine; a workaround spiral is not.

## 3. Session close (run when the user wraps up, OR proactively when context is filling)

If `/api/session/close` exists (selfdoc card): call it with an intent paragraph (why + what's next); it auto-generates the facts (commits, deploys, files touched) from audit+git, replaces state, appends the log. Then update Band 2 views and stop.

Until then, manual — four writes, not six:
1. **STATE (replace, don't append)** — rewrite the `## CURRENT STATE` section of `projects/memory/handoff.md` in place (workdocs_update or targeted edit): ≤40 lines — live images, branch, uncommitted?, pending, next-first-action. History does NOT belong here.
2. **LOG (one append)** — `workdocs_append projects/<project>/build-tracker.md`: a `## SESSION <date>` block — findings, shipped (commit hashes), NEXT. This is the single narrative of record; do not also write a separate journal entry (Helix indexes workdocs).
3. **Band 2 views the session touched** (table below): new feature → feature-ledger card; scenario status changed → flip it; incident lesson → playbook; plan change → roadmap amendment. Plus Infra KB if infrastructure changed.
4. **INTELLIGENCE — author it, don't let it be inferred (added 2026-07-22).**

   Haiku is currently disconnected (`HELIX_LLM_ENRICH: "0"` in the helix compose, set during the bulk backfill). Nothing is inferring session intelligence from transcripts right now. Authoring is also strictly better: the model that was in the session knows which boundary bit, which near-miss never surfaced as an error, and which alternative was rejected and why.

   **Preferred — flush the buffer** (items appended during the session per §2):

       POST http://localhost:9050/api/v1/session/close   {"session_id": "<id>"}

   Add `"dry_run": true` to rehearse. Inspect any time with `GET /api/v1/session/buffer/{session_id}`.

   **Buffer append shape:**

       POST http://localhost:9050/api/v1/session/buffer
       {"session_id": "<id>", "project": "<repo>",
        "items": [{"item_type": "decision", "payload": {"text": "..."}},
                  {"item_type": "relationship",
                   "payload": {"source": "a", "target": "b", "relation_type": "depends_on"}}]}

   `item_type`: decision | failure | pattern | entity | relationship | atom | summary | action | file.
   `provenance` defaults to `authored`.

   **One-shot alternative:** `helix_exchange_post` with session_summary, what_happened, decision+reason, failure, pattern, entities_mentioned, relationships_found, actions_taken, files_changed, next_step.

   **Non-negotiable — verify the counters.** Both paths return `submitted` / `accepted|created` / `rejected[]`. Read them back against what you sent. A clean `status: recorded` is not proof: scar `_d07e` was five relationships vanishing with no error at all. `relationships_found` wants objects with `source`/`target`/`relation_type` — prose strings are now rejected loudly rather than dropped.

   **Atoms (corrected 2026-07-22):** atoms flush to **The Forge** (`the-forge:9095/api/forge/scan`), the LIVE language-agnostic extractor — python, javascript, typescript, go, rust, bash, dockerfile, docker-compose. `helix/services/scanner.py` is a LEGACY python-only AST path; never route atoms there, and note its "Tree-sitter + Haiku" tool description is wrong in both directions (scar `_e9aa`). A language with no registered patterns now returns `unsupported_language` with the registered list instead of a clean zero-atom scan.

   **When Haiku returns:** every record carries `provenance`. Give `haiku_reconciler` a skip-if-authored check before flipping `HELIX_LLM_ENRICH` back to `1`, or you get two versions of one truth with no rule for which wins.

Then tell the user what was saved and which commit hashes matter.

If the session is dying (context limit): run the close ritual EARLY and tell the user to start fresh. A short session that closed cleanly beats a long one that crashed rich.

## 4. The view system (what Band 2 maintenance means)

Controller docs live at workdocs `projects/mesh-controller/views/` — read `00-index.md` for the full map. Bands:

| Band | Views | Who updates |
|---|---|---|
| 1 — hand-written | 01-story, 02-scenarios, 03-dictionary, 04-rules | User + Claude, deliberately |
| 2 — session-updated | 05-feature-ledger, 06-flows, 10-playbooks, 11-roadmap | THIS SKILL, at every close |
| 3 — code-generated | 07-setup, 08-interfaces, 09-code-map | /api/docdrift (planned); hand-patch until then |

12-target-architecture holds the aspirational design (backplane, probes, planes) — graduate items out of it into views 1–11 as they ship.

## 5. Non-negotiables inherited from the constitution

R2 volumes never auto-deleted · R3 nothing destructive without express confirmation · R4 every mutation attributable · R7 docs with the change · R8 no secrets in code/manifests/chat. Cite rule IDs when they bind a decision.

**R8 in practice — redact by default, not by expectation (scar `_e3bc`).** Git remotes on vps1 embed GitHub PATs directly in the URL, so `git remote -v` leaks a live credential into a transcript that Helix captures. The failure mode is incidental exposure while inspecting something unrelated: secrets you EXPECT get handled carefully, secrets you don't get printed. Pipe any command whose output may contain config, env, or remote URLs through a redactor — e.g. `git remote get-url origin | sed -E 's#//[^@]*@#//<redacted>@#'` — and print names, never values.

Note on R4 in `/opt/projects/helix`: a Git Sync Daemon auto-commits within seconds as "Git Sync Daemon" with the message `sync: <path>`, so hand-written commit messages there are lost and mutations are attributed to the daemon (scar `_d60a`). Put rationale in Cicatrix and code comments, not the git log.
