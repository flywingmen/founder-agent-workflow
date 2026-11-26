---
name: founder-agent-workflow
description: Set up and run a build workflow where one founder makes the decisions and Claude Code agents do the building, testing and releasing. Use when someone says what they want to build and wants a way to ship it alone, or asks how to run agents on a real product safely.
---

# Founder + agents build workflow

One founder, one orchestrating Claude, and coding agents ("lanes") that do the hands-on work. The founder decides what the product is for, what ships and what a customer sees. Agents measure, plan, build, test, check and release. Nothing reaches users without the founder's typed yes, and every release can be undone.

Every rule here was written after something went wrong on a live product with paying users. The rules are in `references/doctrine.md`. The files this skill writes into a project are in `templates/`.

## How to use this skill

When the person says what they want to build, do these four steps in order.

### 1. Interview (one message, then wait)

Ask these together, in one short numbered list. Accept short answers. Do not ask more.

1. What are you building, and who uses it?
2. Is it live? Any paying users yet?
3. Where does the code live and where does it run (repo host, hosting)?
4. Where do you track work (Linear, GitHub Issues, Jira, a markdown file)?
5. Do you have a test copy of the app (staging or preview), or only production?
6. How many agents do you want working at once: one, or several in parallel?

### 2. Pick the stage

Size the workflow to the project. Start small and add a stage when its trigger is hit, not before.

| Stage | When | What it adds |
|---|---|---|
| 1. Solo loop | Pre-launch, one agent | Plan before build, tests on the change, one check on a test copy, typed confirm before production, a rollback |
| 2. Live product | Real users or money | Tracker as the record, CI on the exact commit that ships, release windows, "Customers will see" list, blind QA, screens checked on heavy and empty data |
| 3. Parallel lanes | More work than one agent can carry | Orchestrator plus named lanes, one git worktree per lane, one owner per shared resource, handover notes, overnight work, shadow mode for risky logic |

Tell the person which stage they are in and why, in two sentences.

### 3. Write the workflow into their project

Fill the templates with their answers and write them into the repo. Do not paste whole templates into chat.

- `templates/project-rules.md` becomes the agent rules file (append to `CLAUDE.md`, or create it). Keep only the sections for their stage.
- `templates/ticket.md`, `templates/plan.md`: how work is written down in their tracker.
- `templates/release-checklist.md`, `templates/customers-will-see.md`: from stage 1 and 2.
- `templates/qa-journeys.md`: write 3 to 7 real journeys for their product (sign up, the main action, paying, the empty first screen).
- `templates/handover.md`, `templates/lane-brief.md`, `templates/traps.md`: stage 3, and `traps.md` from day one.

Show the list of files written, one line each.

### 4. Run the first loop with them

Take one small real change through the whole loop: ticket, measure, plan, their GO, build, checks, "Customers will see", their typed confirm, release, verify, record. Point out each gate as it happens, once. After that, the rules file runs the workflow and this skill is only reread when they move to the next stage.

## The loop (every change)

1. **Ticket.** One problem, one ticket. The founder's words go on it as written.
2. **Measure first.** Read the code and the data before believing the ticket. Post findings with file:line and the query behind each number. If the premise is wrong, say so first.
3. **Plan, then HOLD.** What was found, what will change, what proves it worked, what a customer will see. The founder answers product questions. Nothing builds before GO.
4. **Build** in its own branch (a worktree per lane at stage 3). Tests on what changed. Break the fix on purpose to prove the tests notice.
5. **Check.** CI green on the exact commit that ships. The building agent clicks through the test copy and posts screenshots at desktop and phone width. From stage 2, a separate agent with no context walks the user journeys.
6. **Customers will see.** One plain list of every visible change, or "nothing visible".
7. **Typed confirm.** The founder types the release line. Rollback is taken and verified before release.
8. **Release and verify by outcome,** not by exit code: the running version, the data, one real check of the new behaviour. Roll back on any failure without asking.
9. **Record.** Ticket closed with the released commit and what rode with it.

## Hard rules (all stages)

- The founder decides product questions: what ships, where a feature lives, what a customer sees, all customer-facing words. Agents never answer these themselves; they bring the question with one recommendation.
- No new customer-visible screen, button or copy gets built without the founder's yes first.
- Investigate the root cause first, then fix. A question or complaint from the founder is not an order to build.
- Nothing reaches production without the founder's typed confirm. Every release has a tested rollback.
- Measured or not said. Unknown is written as "unknown", never estimated.
- Check outcomes, not exit codes.
- The tracker is the record. A ruling or finding that lives only in chat does not exist.
- No secrets, production credentials or customer data in tickets, chat, test copies or agent context.
- Agents never ask the founder for something they can get themselves (docs, logs, public APIs). They bring decisions, and things only the founder can reach.

## Talking to the founder

- Lead with the answer or the decision needed. Short. No replaying the work.
- Anything the founder must do is a numbered list, one action per step, naming exactly what they should see. Steps arrive when they are due, not in advance.
- Anything to paste or type goes in a code block.
- Bad news first, with what it costs and what is already being done.
- End every status reply with two lines:
  - `Plain words:` what was found, what was done, what happens next, in two to four sentences.
  - `For your users:` what a user gains, loses or never notices. If nothing: "invisible, infrastructure only."

See `references/doctrine.md` for the full rules, the QA system and the lessons behind them.
