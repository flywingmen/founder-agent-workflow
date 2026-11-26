# Ideas, backlog and the tracker

The loop starts at a ticket. This is the engine around it: where every idea, request and finding lands, how the founder decides which ones become work, and how tickets are prioritised, grouped, linked and merged so the tracker stays one connected picture.

The rule underneath: **nothing gets built from the inbox.** An idea becomes a ticket only when the founder says yes in triage. Agents capture; the founder decides.

## One inbox

Everything new lands in one place, unprioritised:

- the founder's ideas
- customer requests and complaints
- bugs (from users, alerts, or agents)
- findings agents make while working on something else
- "we should..." from any session

One inbox means nothing is lost in a chat, and nothing skips the founder's yes.

## Setting it up in your tracker

| | Linear | GitHub Issues | Markdown file |
|---|---|---|---|
| Inbox | Triage status (turn on Triage for the team) | Label `inbox` | `docs/inbox.md` |
| Kind | Labels `idea`, `bug`, `request`, `finding` | Same labels | A tag at the start of each line |
| Accepted, not started | Backlog | Label `accepted` | `docs/backlog.md` |
| Next up | Todo, ordered | Milestone "Next" | Top of `docs/backlog.md` |
| In work | In Progress, then In Review | Open PR linked | The ticket file |
| Shipped | Done, with the released commit | Closed by the PR | Moved to `docs/done.md` |
| Bigger goals | Projects | Milestones | A heading per goal |

Agents file into the inbox through the tracker's MCP server or CLI (Linear MCP, `gh issue create`), or by editing the file.

## How things get into the inbox

- **Founder ideas.** When the founder says "idea:" or "park this", the agent files it in the inbox in the founder's own words and starts nothing. Thinking out loud without that word is conversation, not an idea to file. Ask once if unsure.
- **Customer requests.** One item per request, the customer's words quoted, no names or personal data. If it already exists, add a comment with the date and the new wording, so the item counts how many times it was asked.
- **Bugs.** A bug a paying user hits, or anything touching money, data loss or security, skips the queue: it becomes a ticket right away and the founder is told. Every other bug goes to the inbox with how to reproduce it.
- **Agent findings.** Something an agent notices outside its ticket becomes its own inbox item with the evidence (file:line, the query, a screenshot). Never a side note in a report that nobody files.
- **Search first.** Every item is checked against open items; a duplicate becomes a comment on the original.

## Triage

A session, not a stream. Once a week, or when the inbox passes about 20 items.

1. The agent (the orchestrator at stage 3) prepares the list: drops what was settled since, merges duplicates, and for each item proposes one of:
   - **Now:** accepted, goes to the top of Todo.
   - **Later:** accepted into Backlog.
   - **Measure first:** a read-only look before deciding (common for "it feels slow", "results feel thin").
   - **No:** closed, with one line of why.

   Each proposal gets one line of reasoning.
2. The founder answers in one word per item ("1 now, 2 later, 3 no..."), and reads only the items that need a real decision.
3. The agent applies the answers in the tracker the same day and writes the founder's call on each item, dated.

Proposal order, highest first: money paths and data safety; anything a paying user sees; what moves the founder's goal for this month; speed and cost; everything else.

## Keeping the backlog honest

- **Now / Next / Later is the roadmap.** No dates unless the founder sets one.
- **Old items:** anything in Backlog untouched for 60 days comes back to triage as a batch. It is not closed off a list: group by area and impact first, bring high-impact items back, merge duplicates, and only then propose the rest for closing.
- **A request asked by several customers** shows its count in triage, so popularity is visible without a spreadsheet.
- **Ideas the founder said no to** stay closed with the reason, so the same idea doesn't come back as new next month.

## Priorities

Four levels, the same meaning everywhere. The founder sets or confirms them in triage; agents propose.

| Priority | Means | Example |
|---|---|---|
| Urgent | Drop everything. Fix before anything else ships. | Paying users can't use the main feature; money charged wrong; data lost or exposed |
| High | Next release. | A bug a paying user hit; a request several customers asked for; a blocker for this month's goal |
| Medium | Planned, in order. | Most accepted ideas and bugs with a workaround |
| Low | When there's room. | Polish, nice-to-haves, internal cleanup |

- Priority comes from impact, not from who asked loudest. Order inside a level: money and data safety, what paying users see, this month's goal, speed and cost, the rest.
- An agent never raises a priority on its own; it proposes, with one line of why.
- If everything is High, nothing is. In triage, the agent flags when High has more items than one release can carry.

## Epics

A goal bigger than one ticket (a new feature area, a redesign, a migration) is an epic.

- **Linear:** a Project, or a parent issue with sub-issues. **GitHub:** a parent issue with sub-issues, or a milestone.
- The epic holds the why, the founder's words, the plan for the whole thing, and the order of its pieces. Each piece is its own ticket with its own measure, plan and GO.
- One piece ships at a time, each one useful or invisible on its own. No epic sits half-built in production.
- Findings and bugs found while working on an epic are filed as sub-issues of it, so nothing lives only in a lane's memory.
- An epic is done when its last piece ships and the founder has seen it working, not when the code merges.

## Linking tickets

Every ticket is connected to what it touches, so anyone (person or agent) can follow the thread.

- **Mention ticket ids everywhere:** in branch names (`tkt-123-short-name`), commit messages, PR titles, and comments. Linear and GitHub then link them automatically. A PR that finishes a ticket says so (`Fixes TKT-123` / `Closes #123`).
- **Use the relations, not just text:** related, blocks / blocked by, duplicate of, parent / sub-issue.
- **A bug links to its cause:** the release or commit that introduced it, and the ticket that shipped that change. Over time this shows which areas break most.
- **A bug links to its feature or epic,** so a feature's open bugs are visible in one place.
- **A ruling links to every ticket it affects:** when the founder decides something in one ticket that changes others, each of them gets a comment pointing to the ruling.
- **An incident** (something broke in production) gets one ticket that links the bug, the fix, the release, and the line added to `traps.md`.

## Combining tickets

- **Duplicates:** keep the item with the most context (usually the oldest). Copy the other's quotes and evidence into it, mark the other as duplicate, close it. Never lose a customer's words in a merge.
- **The same defect seen from different angles** (three bug reports with one root cause): once measuring shows one cause, propose one umbrella ticket in triage. The reports become sub-issues or duplicates of it, so the fix closes all of them.
- **Related small items** in the same area can ride one ticket and one release if the founder agrees in triage. They stay separate items, linked to that ticket.
- **Never merge to make the list look shorter.** Merge only when it is the same problem or the same fix. A check that labels items as duplicates by matching titles is wrong often enough that the founder or a measuring lane confirms each merge. *(A mechanical scan once labelled live security tickets as duplicates.)*

## From backlog to the loop

When an accepted item reaches the top, it becomes the ticket in step 1 of the loop: the founder's words and the customer quotes are copied in, and the lane starts with measure and plan as usual. Accepting an idea in triage is not a GO to build; the plan still comes back for GO.
