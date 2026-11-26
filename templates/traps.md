# Traps

Things that bit this project once. Read before planning. Add one line each time something goes wrong, with the date and the rule it produced.

- {YYYY-MM-DD}: {what happened, in one line}. Rule: {what we do now}.

Starter entries every project inherits:
- A script exited cleanly having done nothing. Rule: check outcomes, never exit codes.
- Two agents worked in one folder and broke each other's test run. Rule: one worktree per lane.
- A "small" release carried far more than its ticket. Rule: decide checks by everything on the tree, not by the ticket.
- A filter treated missing data as a no and quietly dropped good records. Rule: unknown is never a no.
- A test passed by matching a logged command instead of what ran. Rule: mutation round on every new test.
- A test run used the production model account and emptied it. Rule: separate billing for test keys; state the cost before any spend.
