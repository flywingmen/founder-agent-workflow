# Workflow rules

<!-- Written by the founder-agent-workflow skill. Keep only the sections for this project's stage. Fill every {placeholder}. -->

Product: {one line: what it is and who uses it}
Tracker: {Linear / GitHub Issues / Jira / docs/tickets.md}
Test copy: {staging URL or preview command, or "none yet"}
Release command: {deploy script or platform step}
Rollback command: {how to undo a release}
Founder: {name}. Decides product questions and types every production confirm.

## Every change (stage 1+)

1. Ticket first. One problem per ticket. The founder's words go on it as written.
2. Measure before believing the ticket: read the code at the live commit (cite file:line) and the data (post the query). If the premise is wrong, say so first.
3. Post a plan on the ticket (see the plan template), then HOLD. Do not build before the founder or orchestrator says GO.
4. Build on a branch. Run the type check and the tests that touch the change. Then break the fix on purpose and confirm a test goes red.
5. Click through the change on the test copy in a headless browser. Post screenshots at 1440 and 390 width. Check with heavy data and with a brand-new empty account.
6. Write the "Customers will see" list, or "nothing visible".
7. Wait for the founder's typed confirm. Take and verify the rollback. Release.
8. Verify by outcome (live build, migration tip, one real example of the new behaviour). On failure, roll back first, report second.
9. Close the ticket with the released commit, what rode with it, and how a user does it.

## Never

- Decide a product question (placement, flow, what a customer sees, customer-facing words). Bring it to the founder with one recommendation.
- Build a new customer-visible screen, button or copy without the founder's yes.
- Treat a founder's question or complaint as an order. Answer it, then wait.
- Release to production without the founder's typed confirm.
- Trust an exit code. Check the outcome.
- Estimate a number you could measure. Write "unknown" instead.
- Put secrets, production credentials or customer data in tickets, chat or the test copy.
- Ask the founder for something you can get yourself (docs, logs, public APIs).
- Edit a migration after it has run anywhere.

## Live product (stage 2+)

- Every ruling, finding and quick fix goes on the ticket in the same turn, dated.
- CI must be green on the exact commit that ships: zero failed, zero skipped. Cite the run id.
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
