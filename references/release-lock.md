# The release lock

"Nothing reaches production without the founder's yes" must not depend on the model behaving. The lock is about **whose credentials can deploy**: agents can push code, only the founder can ship it.

## Setup (GitHub, any plan, private repo)

1. **Give agents their own token.** Create a fine-grained personal access token (GitHub, Settings, Developer settings, Fine-grained tokens) for the product repo only, with:
   - Contents: Read and write (so agents can push branches)
   - Pull requests: Read and write (so they can open PRs)
   - Metadata: Read
   - **Actions: no access** (so they can't trigger or re-run workflows)
   - **Workflows: no access** (so they can't change files in `.github/workflows`)

   Everything else: no access. Set an expiry.
2. **Make the agents use it.** On the machine or cloud environment where agents run, authenticate git and `gh` with this token only. The founder's own GitHub login lives in the browser and on the phone, not in the agents' terminal. If the founder's login is in the agents' shell, the lock is open.
3. **Keep production secrets out of the agents' reach.** Deploy keys, hosting tokens and server access go into GitHub Actions secrets (or the hosting platform), never into `.env` files on the agents' machine.
4. **Deploy only by hand.** Copy `templates/deploy-workflow.yml` to `.github/workflows/deploy.yml`. It runs only on `workflow_dispatch`, checks that the commit's CI is green, then deploys. The founder triggers it from their own account (Actions tab, "Run workflow", or the GitHub phone app). **That trigger is the typed confirm.**
5. **No other workflow deploys.** Check that no workflow deploys on `push` to main. If one does, remove its deploy step.

## Why each piece matters

| If the agent could... | Then it could... | Blocked by |
|---|---|---|
| Trigger a workflow | Run the deploy itself | Token has no Actions access |
| Edit a workflow file | Add a deploy-on-push job | Token has no Workflows access |
| Read production secrets | Deploy from its own shell | Secrets live only in GitHub or the host |
| Use the founder's login | Do anything the founder can | Agents' shell is logged in with their token only |

## Other ways to get the same gate

- **Hosting platform promotion.** Vercel, Fly, Railway and others can deploy previews automatically and promote to production only by a manual action in their dashboard. If the agent has no token for the platform, that promote button is the gate.
- **Required reviewers on a GitHub Environment.** Available on public repos, and on private repos with GitHub Enterprise. If you have it, add `environment: production` to the deploy job and the founder as required reviewer, as an extra check on top of the above.

## The backup

`.claude/settings.json` asks before deploy, migration and production commands, and before pushes to main. Permission rules match how a command starts, so a reworded command can slip past, and a `Read` rule on `.env` does not stop `cat .env`. It catches honest mistakes. The token is what holds.
