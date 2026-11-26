---
name: founder-agent-workflow
description: Set up a build workflow for a founder shipping a product alone with Claude Code agents, where the founder decides and agents build, test and release. Use when someone wants to run a real product (users, releases, production) solo with agents, not for one-off coding help.
---

# Founder + agents build workflow

One founder, one orchestrating Claude, and coding agents ("lanes") that do the hands-on work. The founder decides what the product is for, what ships and what a user sees. Agents measure, plan, build, test, check and release. Nothing reaches users without the founder's typed yes, and every release can be undone.

Every rule here was written after something went wrong on a live product with paying users. The rules are in `references/doctrine.md`. The files this skill writes into a project are in `templates/`.

## How to use this skill

When the person says what they want to build, do these four steps in order.

### 1. Interview (one message, then wait)

Ask these together, in one short numbered list. Accept short answers. Do not ask more.

1. What are you building, and who uses it?
2. Is it live? Any paying users yet?
3. Where does the code live, and how do you deploy today (a command, a script, a platform button)?
4. How do you undo a bad release today? ("I don't" is a fine answer.)
5. Where do you track work (Linear, GitHub Issues, Jira, a markdown file)?
6. Do you have a test copy of the app (staging or preview), or only production?
7. How many agents do you want working at once: one, or several in parallel?

### 2. Pick the stage

Size the workflow to the project. Start small and add a stage when its trigger is hit, not before.

| Stage | When | What it adds |
|---|---|---|
| 1. Solo loop | Pre-launch, one agent | One inbox for ideas and a weekly triage, plan before build, tests on the change, one click-through, "Customers will see" list, typed confirm before production, a rollback, no production credentials for agents |
| 2. Live product | Real users or money | Tracker as the record, CI on the exact commit that ships, release windows, production deploys only by the founder's own trigger, mutation rounds, screenshots at desktop and phone width, heavy and empty data checks, blind QA |
| 3. Parallel lanes | More work than one agent can carry | Orchestrator plus named lanes, one git worktree per lane, one owner per shared resource, handover notes, overnight work, shadow mode for risky logic |

Tell the person which stage they are in and why, in two sentences.

**If they have no rollback (Q4):** writing one is the first ticket, before any feature work.

**If they have only production (Q6):** every check that needs a test copy uses a substitute proof, named in the same sentence (doctrine 5.8): release behind a flag or to the founder's own account first, verify there, then turn it on for everyone. Setting up a preview or staging copy is the first stage 2 ticket.

### 3. Write the workflow into their project

Fill the templates with their answers and write them into the repo. Do not paste whole templates into chat.

- `templates/project-rules.md` becomes the agent rules file (append to `CLAUDE.md`, or create it). Keep only the sections for their stage.
- **The real lock is whose credentials can deploy.** Agents get their own GitHub token that can push branches and open PRs but has no Actions or Workflows access, and production secrets live only in CI or the host. `templates/deploy-workflow.yml` becomes `.github/workflows/deploy.yml`: it runs only when the founder triggers it from their own account, and refuses unless CI is green on that commit. That trigger is the typed confirm. Walk them through `references/release-lock.md` step by step; it works on any GitHub plan, including private repos on Free and Pro. If their host has a manual promote step (Vercel, Fly, Railway) and agents have no host token, that works too.
- `templates/settings.json` becomes `.claude/settings.json` (merge if one exists). It is a backup, not the lock: Claude Code asks before commands that start with these patterns, but a differently written command can slip past, and a `Read` rule does not stop `cat`. Replace the example entries with their real deploy, migration and production commands. Tell them not to run release work in a mode that skips permission prompts.
- `templates/ticket.md`, `templates/plan.md`: how work is written down in their tracker.
- Set up the tracker engine from `references/backlog.md`: inbox and triage, priorities, epics, links between tickets and bugs, and merging (statuses and labels for Linear, GitHub Issues or a file), and `templates/triage.md` for the weekly session.
- `templates/release-checklist.md`, `templates/customers-will-see.md`: from stage 1 and 2.
- `templates/qa-journeys.md` (stage 2): write 3 to 7 real journeys for their product (sign up, the main action, paying, the empty first screen).
- `templates/handover.md`, `templates/lane-brief.md` (stage 3), and `templates/traps.md` from day one.
- Stage 3: walk them through `references/lanes.md` (opening a lane in its own worktree, naming it, getting briefs to it and reports back, running it from the phone).

**Browser checks need a browser tool.** Click-throughs and blind QA use Playwright (`npm i -D playwright && npx playwright install chromium`) or a browser MCP server. If none can be installed, the founder clicks through instead, from a numbered list of what to open and what they should see.

Show the list of files written, one line each.

### 4. Run the first loop with them

Take one small real change through the whole loop. Point out each gate as it happens, once. After that, the rules file runs the workflow and this skill is only reread when they move to the next stage.

**On claude.ai (no repo access):** do steps 1 and 2, then give the filled files as downloads with one line on where each goes. The loop itself runs in Claude Code.

## The loop (every change)

0. **Inbox.** Ideas, requests, bugs and findings land in one inbox. Nothing is built from it until the founder accepts it in triage (`references/backlog.md`).
1. **Ticket.** One problem, one ticket, from an item the founder accepted (urgent bugs skip triage). The founder's words go on it as written.
2. **Measure first.** Read the code and the data before believing the ticket. Post findings with file:line and the query behind each number. If the premise is wrong, say so first.
3. **Plan, then HOLD.** What was found, what will change, what proves it worked, what a user will see. The founder answers product questions. Nothing builds before GO.
4. **Build** on its own branch (a worktree per lane at stage 3). Tests on what changed. From stage 2, break the fix on purpose to prove the tests notice.
5. **Check.** Tests green on the commit that ships (CI from stage 2). The building agent clicks through the change and posts what it saw (screenshots at both widths from stage 2). From stage 2, a separate agent with no context walks the user journeys.
6. **Customers will see.** One plain list of every visible change, or "nothing visible".
7. **Typed confirm.** The founder triggers the deploy from their own account (stage 1 without a deploy workflow: the founder runs or types the release line). Rollback is taken and verified before release.
8. **Release and verify by outcome,** not by exit code: the running version, the data, one real check of the new behaviour. Roll back on any failure without asking.
9. **Record.** Ticket closed with the released commit and what rode with it.

## Fast path (small changes)

A change is small only if all of these are true:

- It touches copy, styling, docs or a config value, and no logic.
- It touches no data, migrations, money, auth, permissions or anything that sends to users.
- It is a few lines and reverts cleanly.

Small changes skip steps 2 and 3 (measure, plan) and the mutation round. The fast path skips the plan, not the founder's say on wording: any user-facing words still need the founder's yes before release. They keep: a one-line ticket, the tests that exist, one screenshot if anything visible changed, and the typed confirm. They ride the next release with everything else. **If in doubt, it is not small.** An agent that takes the fast path says so in one line on the ticket.

## Hard rules (all stages)

- The founder decides product questions: what ships, where a feature lives, what a user sees, all user-facing words. Agents never answer these themselves; they bring the question with one recommendation.
- No new user-visible screen, button or copy gets built without the founder's yes first.
- Investigate the root cause first, then fix. A question or complaint from the founder is not an order to build.
- Nothing reaches production without the founder's typed confirm. Agents never hold production credentials; only the founder's own account can trigger a deploy, with `.claude/settings.json` as a backup. Every release has a tested rollback.
- Measured or not said. Unknown is written as "unknown", never estimated.
- Check outcomes, not exit codes.
- The tracker is the record. A ruling or finding that lives only in chat does not exist.
- No secrets, production credentials or user data in tickets, chat, test copies or agent context.
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
