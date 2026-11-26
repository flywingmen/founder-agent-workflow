# founder-agent-workflow

A Claude skill for building and running a product alone: you make the decisions and talk to customers, Claude Code agents do the building, testing and releasing.

Give it to your Claude, say what you are building, and it will:

1. Ask six short questions about your product and setup.
2. Pick the stage you are at: solo loop, live product, or parallel agents.
3. Write the rules and templates into your repo, sized to that stage.
4. Take your first real change through the whole loop with you.

Every rule in it was written after something went wrong on a live product with paying users.

## Install

Claude Code, for all your projects:

```
git clone https://github.com/flywingmen/founder-agent-workflow ~/.claude/skills/founder-agent-workflow
```

Claude Code, for one project: clone it into `.claude/skills/founder-agent-workflow` in that repo.

Claude.ai: download this repo as a zip and upload it under Settings, Capabilities, Skills.

Then tell Claude what you want to build.

## What's inside

- `SKILL.md`: the interview, the three stages, the change loop, the hard rules.
- `references/doctrine.md`: the full rules, including how checking works when agents write the code.
- `templates/`: project rules, ticket, plan, release checklist, "Customers will see", QA journeys, handover, lane brief, traps.

## License

MIT
