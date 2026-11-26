# Running lanes in Claude Code (stage 3)

How to actually set up an orchestrator and parallel lanes. Read this when one agent can no longer carry the work.

## The pieces

- **Orchestrator:** one long-running Claude session that holds the plan, the tracker and the founder's rulings. It writes the briefs and reads the reports. It does not write product code.
- **Lanes:** one Claude Code session per ticket, each in its own git worktree, each with a short name (a callsign) so it can be addressed.
- **Founder:** talks only to the orchestrator, and answers permission prompts in the lanes.

## Open a lane

1. Make a worktree for the ticket, next to the main checkout:
   ```
   git worktree add ../app-TKT-123 -b tkt-123 origin/main
   cd ../app-TKT-123
   ```
2. Make sure this shell is logged in to GitHub with the agents' token, not the founder's account (`references/release-lock.md`).
3. Start Claude Code there with a name:
   ```
   claude --name RADAR
   ```
   (In a session that is already running, `/rename RADAR` does the same.)
4. Keep the lane in the default ask-first permission mode, not a mode that skips prompts. The founder sees and approves anything risky.
5. Give it its first brief (`templates/lane-brief.md`): ticket, phase, scope, acceptance, where to post, and HOLD.

Each lane reads the project's `CLAUDE.md`, so the workflow rules apply in every lane without repeating them.

## Get briefs to lanes and reports back

Pick one. Start with the first; move to the second when pasting gets tiring.

1. **Founder relays.** The orchestrator writes each brief in a code block; the founder pastes it into the lane's terminal tab and pastes the lane's report back. Simple, and the founder sees everything. It costs the founder's time.
2. **Orchestrator messages lanes directly.** Run the orchestrator as a Claude Code session on the same machine as the lanes. In the setup this came from, it lists the named sessions running there and sends a message to one by name, and the lane replies the same way. This depends on your Claude Code version and surface, so test it once before relying on it: ask the orchestrator to list the running sessions and send a lane a one-line message. If it can't, stay with the founder relay. The founder now only answers questions and permission prompts. Lanes still never message each other.

Either way, the ticket is the record: lanes post plans, findings and release notes on their ticket, so nothing important lives only in a message.

## Run it from your phone

Turn on Remote Control in each lane (`/rc`). The founder can then read lanes, type short answers and approve permission prompts from the Claude app away from the desk. Anything that needs a raw shell command still needs the computer.

## Keep lanes from colliding

- Every lane works only in its own worktree. Nobody works in the main checkout that releases run from.
- Shared things have one owner at a time: the main branch, the test copy, migration numbers, the build server. Record the owner on one standing ticket.
- Migrations: a lane reserves its number on the standing ticket before writing one.
- Heavy work (full test runs, builds, browsers) goes to a build server or CI, not the founder's laptop, once more than two lanes run at once.

## Keep lanes alive

- Name every lane in the tracker: callsign, ticket, state. The orchestrator checks each lane's last activity on every sweep and nudges one that went quiet mid-task.
- Near its context limit, a lane finishes its current hold, writes a pickup note (`templates/handover.md`) and is retired. A new lane starts from the note, never "from memory".
- After a restart, re-name each lane and point it at its pickup note.
- When the orchestrator restarts, it tells every lane where to report.
- Before the founder ends the day, idle lanes get work that needs no release and no GO: the next piece in a fresh worktree, held before merge.

## Close a lane

When its ticket is released and recorded: remove the worktree by exact path (`git worktree remove ../app-TKT-123`), delete its throwaway database by exact name, mark the callsign free, close the tab.

## How many lanes

Two or three is plenty for most solo founders. The limit is not the agents; it is how many plans, screenshots and questions the founder can review in a day. Planning-only lanes (measure and plan, no build) are cheap to run in parallel; building lanes are where the review load comes from.
