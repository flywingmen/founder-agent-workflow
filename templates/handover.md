# Pickup note (lane) / Handover (orchestrator)

## Lane pickup note
- Callsign and ticket: {callsign} {ticket}
- Branch and commit: {branch} {sha}
- Worktree path: {path}
- Database or cluster: {name and port, if any}
- State: built / merged / released: {which}
- Committed vs uncommitted: {list}
- Exact next step: {one sentence}
- Hazards: {what could bite the next session}

## Orchestrator handover
Part 1, doctrine changes since the last handover:
- {YYYY-MM-DD}: {new rule, and the founder's words that set it}

Part 2, live state:
- Production: commit {sha}, build {id}, migration tip {n}
- Test copy: commit {sha}
- Recent releases: {window, time, what rode}
- Lanes: {callsign, ticket, state, what it owes}
- Release queue: {order}
- Waiting on the founder: {numbered list of open decisions}

The new session's first reply lists corrections to this note, then takes over.
