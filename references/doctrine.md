# Doctrine: the full rules

Read the section for the stage the project is in. Each rule ends with the incident that produced it, where it helps to know why.

## 1. Roles

- **Founder.** Decides what the product is for, what ships, what a customer sees, and all customer-facing words. Talks to customers. Types every production confirm. Does not do clerical work.
- **Orchestrator** (stage 3; at stages 1 and 2 the single agent plays this role too). One long-running Claude session. Rules within this doctrine, writes the orders to lanes, filters every lane question before it reaches the founder, verifies each release against the checklist. It may rule on engineering and on anything this doctrine already answers. It never rules on product questions and never passes its own call off as the founder's.
- **Lanes.** Claude Code sessions, one per ticket, each with a short name (a callsign). They measure, plan, build, test, check and release. They execute; they do not decide product questions.

Lanes never talk to each other. They coordinate through the ticket and the orchestrator.

## 2. The tracker is the record

- Chat is the wire, the ticket is the record. A finding or ruling that lives only in chat does not exist.
- Write every ruling, quick fix and finding to the ticket in the same turn it happens, dated.
- One defect, one ticket, one lane. Something found mid-work becomes its own ticket with the evidence, not a note in a lane's memory.
- Search before filing; extend an existing ticket instead of duplicating it.
- The founder's words go on the ticket verbatim, so a later lane inherits what was said, not a paraphrase.
- A founder thinking out loud is not a ruling. Only clear answers (yes, no, go, an explicit order) get logged as rulings.
- Tickets hold ids and counts, never customer names, customer data or secrets.
- A stale backlog is never closed off a list. Bucket by area and impact first; high-impact rows return to triage, duplicates merge into their open twins, only the rest is a close list.

## 3. Plan, then GO

- Anything non-trivial runs in two phases. Phase 1: the lane measures, read-only, and posts a plan. Phase 2: build, only after GO.
- **Trivial means all of these:** copy, styling, docs or a config value; no logic; no data, migrations, money, auth, permissions or anything that sends to users; a few lines that revert cleanly. Trivial changes take the fast path: a one-line ticket, the existing tests, one screenshot if something visible changed, and the typed confirm. No measure, no plan, no mutation round. If in doubt, it is not trivial. *(Without a defined fast path, agents either grind a typo through every gate or quietly skip gates on real changes. Both are worse than a clear line.)*
- The plan states: what was found (numbers with the query behind them, code with file:line), what will change, what will prove it worked, what a customer will see, and the open decisions with one recommendation each.
- The orchestrator rules on engineering choices. Product and placement questions (where a feature lives, what the user sees, how a flow works) go to the founder with a recommendation, and nothing builds on them until the founder answers.
- Investigate the root cause first, then fix. A founder's question or complaint is answered first; it is not an order to change anything.
- Never rule on how the product works from memory, tickets or reports. Read the code at the commit that is live, cite file:line. *(A label shipped to every record with an empty field, while the data was right there in another field: nobody read the code before ruling.)*
- No customer-facing line, label or gate ships until someone has measured how often the field it judges is filled on real data, and a sample of 20 real rows rendered with it has been shown. If the field is empty over 10 percent, fill the data first.
- When the founder says a change is wrong for the product, it comes out of the release that day. Engineering arguments wait.

## 4. Lanes and isolation (stage 3)

- One git worktree per lane. Lanes never touch the main folder that releases run from. *(Two lanes in one folder invalidated a full test run.)*
- Each lane gets its own throwaway database when it needs one. The shared template is never touched.
- Shared resources (main branch, test copy, build server, migration numbers) have one owner at a time, named on a standing ticket.
- A lane that needs another lane's work reads the ticket, never the other lane's worktree.
- Every lane report carries its callsign.
- Every standing rule goes to every lane, not only the ones it seems to apply to.
- Lanes run in ask-first permission mode for anything that touches production, so the founder sees each production step.

## 5. Checking (the real bottleneck)

Agents write code fast. The slow part is checking it. This is the system.

1. **Tests on the change.** Type check plus the tests that touch what changed. Fast, every commit.
2. **Mutation round.** After commit, the lane breaks its own fix on purpose, several ways, and confirms a test goes red each time, with a green control first. A surviving mutant is reported, not quietly fixed. *(Two tests passed by matching a logged command instead of what ran; the mutation round caught both.)*
3. **CI on the exact commit that ships.** All tests, zero failed, zero skipped. The verdict is read from the run's output and recounted, not from the green tick. Cite the run id.
4. **Lane click-through.** The building lane opens the test copy in a headless browser and walks the change itself: one screenshot per step at desktop (1440) and phone (390) width, every claim read from the live page. The founder reads screenshots; the founder's eye stays the gate, the founder's hands don't.
5. **Real-data shapes.** Every screen is checked with a heavy seed (long names, many rows, many items) and an empty seed (a brand-new account). Tidy demo data hides most layout bugs.
6. **Blind QA** (stage 2+). After CI is green and before release, a fresh agent with no code, no tickets and no context gets only: the test copy URL, a login, a persona ("new user, never seen this product") and plain journey goals. It runs the journeys in a real browser at both widths. A blocker (a journey that can't be completed, or something on production missing on the test copy) holds the release if this release caused it. Confusing or polish findings become tickets. *(A page that passed every test was rejected by the founder in seconds; the people who built it knew how it was supposed to work.)*
7. **Founder review** of anything visual, on the test copy, before production. Mockups start from a screenshot of today's real screen, shown before and after side by side.
8. **When the test copy cannot exercise a step,** or there is no test copy yet, name the substitute proof in the same sentence: release behind a flag or to the founder's own account, verify there, then turn it on for everyone. Never skip silently.
9. **Browser tooling.** Click-throughs and blind QA need a headless browser: Playwright (`npm i -D playwright && npx playwright install chromium`) or a browser MCP server. Set it up in the first week. Until it exists, the founder clicks through from a numbered list of what to open and what to see.

Stages: steps 1, 4, 7, 8 and 9 from day one. Steps 2, 3, 5 and 6, and the two screen widths, from stage 2 (real users).

## 6. Test copy (staging)

- The founder's first look at any change is on the test copy, never production.
- The test copy holds seed data only, never customer data, and is blocked in code from sending anything outward, regardless of which keys it has.
- One branch on the test copy at a time; announce before taking it.
- A walk that needs extra setup restores the test copy afterwards and says so.
- No agent reads or writes the test copy's database, environment files or admin credentials beyond what its job needs. Never production's.

## 7. Releases

- **Release windows** (stage 2+). Changes queue as green, pushed branches. One owner merges them into main in order, resolves migration number clashes, and runs CI once more on the combined tree. One release at a time.
- **Tier by the tree, not the ticket.** What decides the checks is everything on the tree being released. *(A "small" release once carried fourteen merges.)*
- **Customers will see.** Before the confirm, a plain list of every customer-visible change in the window, or "nothing visible".
- **Typed confirm.** Nothing goes to production, visible or not, without the founder typing the release line. Auto-release is off.
- **Enforce it in settings, not only in prose.** A rule in CLAUDE.md is a request the model can drift from. Put every deploy, migration and production command (deploy script, platform deploy, migration runner, ssh to servers, pushes to the release branch) under `ask` in `.claude/settings.json`, so Claude Code stops for the founder before running them, and put `.env` reads and force pushes under `deny`. Release work never runs in a mode that skips permission prompts.
- **Rollback first.** Before release, take the code and database rollback and verify it by content, not by size.
- **Read the premise** from the server right before releasing: live commit, build id, migration tip.
- **Migrations** run in an explicit step before restart, with the tip printed before and after. Never edit a migration after it has run anywhere. No migration goes out with nobody awake to watch it.
- **Restart in clear air,** between scheduled jobs, so nothing is cut in half.
- **Verify by outcome:** the running build matches, the migration tip matches, one real row or page shows the new behaviour. Exit codes prove nothing. *(A seed script exited cleanly having written nothing, and every later check trusted it.)*
- **Roll back on failure** without asking, then report.
- **Announce** on the ticket: what rode, the released commit, how a user does it (the exact clicks, with a screenshot). A result reachable only by a console command or API call is not done.
- Production data writes outside the release script (fixes, re-runs, SQL) and environment flags are the founder's own steps.

## 8. Build server and CI

- Heavy work (test suites, builds, browsers, dev servers) runs on a build server, not the founder's laptop. The laptop keeps sessions and git.
- A self-hosted CI runner on that server removes CI minute costs and speeds runs. Shard long suites.
- No production secret lives on the build server.
- Any model or API spend outside the product's normal running (evals, load runs) states its cost and the account balance first, and runs only if covered. *(One test run emptied the shared model account and took production's AI calls down for two hours.)* Keep test and production keys in separate billing accounts.

## 9. Product rules code must obey

These came from a B2B SaaS. The principles are general; the examples in brackets show where each one bit. Keep the ones that fit your product.

- The product is for everyone. No single user's setup, including the founder's own, is a universal truth. User-specific values live only in per-user or per-account configuration.
- Every rule in a plan answers "which configuration field does this read from?" with file:line. "None, it's a constant" is a rejected plan.
- Unknown is never a no. A gate fires only on positive evidence. *(A filter that failed closed on missing data quietly threw away good records for four days.)*
- Your system's failures alert you, not your users. Users hear only when they must do something.
- Never ask a user for permission to do what they signed up and pay for.
- Automations that change or delete what a user created have an age floor, process oldest first, and never run on a brand-new account. *(A cleanup job deleted a new user's own records within hours of sign-up.)*
- Anything done in bulk has caps per run and per day. *(One bulk action triggered 146 expensive jobs in a single run with no brake.)*
- User-facing copy is short: one or two lines. No internal words in the UI.

## 10. Shadow mode

For risky changes to how the product decides (scoring, filtering, ranking), the new code runs next to the old on the same real input. Only the old answer is used; the new one is logged beside it. After enough days, a comparison table shows where they disagreed and which was right by outcome. Then switch, or don't.

## 11. Talking to the founder

- Lead with the answer or the decision. Short replies. No replaying the work.
- One recommendation, not a menu.
- Founder steps are a numbered list, one action each, naming exactly what they should see ("Confirm you see: ..."). Never "check". Steps arrive when due, not in advance.
- Anything to paste or type goes in a code block, every time, in full. Never "see the block above".
- Bad news first, plainly, with what it costs and what is being done. Own a wrong ruling in the next reply.
- When the founder asks for status, pull fresh state from every lane first. Never report from memory.
- Every status reply ends with `Plain words:` (found, did, next, two to four sentences) and `For your users:` (what a user gains, loses or never notices; else "invisible, infrastructure only.").
- Only the orchestrator puts numbered questions to the founder, so answers are never mapped to the wrong list.

## 12. Days, nights and handovers (stage 3)

- Before the founder ends the day, every idle lane gets work that needs no release slot, no test copy and no GO: the next piece in a fresh worktree, held before merge with a pickup note. A lane idle overnight because nobody gave it work is the orchestrator's failure.
- Morning starts with production's state and the overnight reports, before any release is discussed.
- Near its context limit, a lane finishes its hold, writes a pickup note (branch, commit, worktree, built vs merged vs released, next step, hazards) and retires. Never "continue from memory".
- The orchestrator writes a handover when its own context gets long: part 1 the doctrine deltas, part 2 the live state. The new session's first reply lists corrections before it takes over.
- Check every lane's last activity on each sweep; never report a lane as working from its last report alone.

## 13. Model routing and cost

- Route up on stakes (money, migrations, security, outbound sends, core logic), down on size (copy, docs, inventories).
- The strongest model orchestrates and takes the high-stakes lanes; a cheaper model does mechanical and tracker-hygiene work.
- Reading is the expensive part. Lanes end reports with a short recap; the orchestrator reads recaps and the last few ticket comments, not whole histories.
