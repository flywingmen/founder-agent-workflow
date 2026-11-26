# Workflow rules

<!-- Written by the founder-agent-workflow skill. Keep only the sections for this project's stage. Fill every {placeholder}. -->

Product: {one line: what it is and who uses it}
Tracker: {Linear / GitHub Issues / Jira / docs/tickets.md}
Test copy: {staging URL or preview command, or "none: production only, use substitute proofs"}
Release command: {deploy script or platform step}
Rollback command: {how to undo a release}
Founder: {name}. Decides product questions and types every production confirm.

## Every change (stage 1+)

1. Ticket first. One problem per ticket. The founder's words go on it as written.
2. Measure before believing the ticket: read the code at the live commit (cite file:line) and the data (post the query). If the premise is wrong, say so first.
3. Post a plan on the ticket (see the plan template), then HOLD. Do not build before the founder says GO.
4. Build on a branch. Run the type check and the tests that touch the change.
5. Click through the change yourself (on the test copy, or with the substitute proof if there is none) and post what you saw.
6. Write the "Customers will see" list, or "nothing visible".
7. Wait for the founder's typed confirm. Take and verify the rollback. Release.
8. Verify by outcome (live version, one real example of the new behaviour). On failure, roll back first, report second.
9. Close the ticket with the released commit and what rode with it.

## Fast path (small changes)

Small means: copy, styling, docs or a config value; no logic; no data, migrations, money, auth, permissions or anything that sends to users; a few lines that revert cleanly. If in doubt, it is not small.

Small changes skip steps 2 and 3, not the founder's say on wording: user-facing words still need the founder's yes. They keep a one-line ticket, the existing tests, one screenshot if anything visible changed, and the typed confirm. Say "fast path" on the ticket.

## No test copy yet

When a check needs a test copy and there is none, name the substitute proof in the same sentence: release behind a flag or to the founder's own account, verify there, then turn it on for everyone. Never skip a check silently.

## Never

- Decide a product question (placement, flow, what a user sees, user-facing words). Bring it to the founder with one recommendation.
- Build a new user-visible screen, button or copy without the founder's yes.
- Treat a founder's question or complaint as an order. Answer it, then wait.
- Release to production without the founder's typed confirm. You hold no production credentials; your GitHub token cannot trigger or edit workflows, and only the founder runs the deploy ({deploy workflow name / host promote step}). `.claude/settings.json` asks before deploy, migration and production commands; never work around a permission prompt or reword a command to avoid one.
- Trust an exit code. Check the outcome.
- Estimate a number you could measure. Write "unknown" instead.
- Put secrets, production credentials or user data in tickets, chat or the test copy.
- Ask the founder for something you can get yourself (docs, logs, public APIs).
- Edit a migration after it has run anywhere.

## Live product (stage 2+)

- Every ruling, finding and quick fix goes on the ticket in the same turn, dated.
- CI must be green on the exact commit that ships: zero failed, zero skipped. Cite the run id.
- After each commit, break the fix on purpose several ways and confirm a test goes red each time.
- Click-throughs post screenshots at 1440 and 390 width, and check changed screens with heavy data and with a brand-new empty account.
- Changes ship in release windows: green branches queue, one owner merges them in order and reruns CI on the combined tree.
- Before each release, a fresh agent with no context runs the journeys in `docs/qa-journeys.md` on the test copy. A blocker caused by this window holds the release.
- Anything visual is reviewed by the founder on the test copy before production.

## Parallel lanes (stage 3)

- One git worktree per lane. Never work in the main folder.
- Shared resources have one owner at a time: {main / test copy / build server / migration numbers}, recorded on {standing ticket}.
- Lanes never message each other. Coordinate through the ticket and the orchestrator.
- Sign every report with your callsign.
- Near the context limit: finish the hold, write a pickup note (see `docs/handover.md`), retire.

## Replies to the founder

Lead with the answer. Steps the founder must do: numbered, one action each, with what they should see. Anything to paste: in a code block. End every status reply with:

Plain words: what was found, what was done, what happens next.
For your users: what a user gains, loses or never notices (or "invisible, infrastructure only.").
