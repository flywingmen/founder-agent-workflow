# The release lock

"Nothing reaches production without the founder's yes" must not depend on the model behaving. The lock is about **whose credentials can deploy**: agents can push code, only the founder can ship it.

Claude does this setup. The founder only does the steps Claude can't reach (creating the token, adding secrets, a host setting), from a numbered list that says what they should see.

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
5. **Nothing deploys except the founder's run.** Two places to check:
   - **The host.** Vercel, Netlify, Render, Railway and others often deploy production by themselves on every push to main. That goes around everything above. Turn off automatic production deploys and keep the previews: on Vercel, `"git": {"deploymentEnabled": {"main": false}}` in `vercel.json`; on Netlify, lock publishing so a deploy goes live only when published; on Render, Auto-Deploy off; elsewhere, the host's auto-deploy setting. Settings move, so read the host's current docs first. Then `deploy.sh` calls the host's CLI with a token kept in Actions secrets. If the founder would rather click the host's own "promote" or "publish" button, that button is the gate instead, as long as agents have no host token.
   - **Other workflows.** No workflow deploys on any trigger except the founder's manual run: not on `push`, tags, `release`, `pull_request` or `schedule`. If one does, remove its deploy step.
6. **Check the lock.** Claude tests it from its own shell, the same shell lanes will use:
   ```
   gh auth status
   gh workflow run deploy.yml -f sha=0000000000000000000000000000000000000000
   ```
   The first must show the agents' token, not the founder's account. The second must be refused (HTTP 403). The all-zeros commit doesn't exist, so even if the run starts, its CI check refuses and nothing ships. If it is not refused, or the host CLI in this shell is logged in, the lock is open: Claude tells the founder and fixes it before any feature work. Run the check again whenever the token or the host changes.

## Why each piece matters

| If the agent could... | Then it could... | Blocked by |
|---|---|---|
| Trigger a workflow | Run the deploy itself | Token has no Actions access |
| Edit a workflow file | Add a deploy-on-push job | Token has no Workflows access |
| Read production secrets | Deploy from its own shell | Secrets live only in GitHub or the host |
| Use the founder's login | Do anything the founder can | Agents' shell is logged in with their token only |
| Merge to main | Ship through the host's auto-deploy | Host's automatic production deploys are off |

## Other ways to get the same gate

- **Hosting platform promotion.** Vercel, Fly, Railway and others can deploy previews automatically and promote to production only by a manual action in their dashboard. If the agent has no token for the platform, that promote button is the gate.
- **Required reviewers on a GitHub Environment.** Available on public repos, and on private repos with GitHub Enterprise. If you have it, add `environment: production` to the deploy job and the founder as required reviewer, as an extra check on top of the above.

## The backup

`.claude/settings.json` asks before deploy, migration and production commands, and before pushes to main. Permission rules match how a command starts, so a reworded command can slip past, and a `Read` rule on `.env` does not stop `cat .env`. It catches honest mistakes. The token is what holds.
