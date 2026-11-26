# QA journeys

For the blind QA agent. It gets only this file, the test copy URL, a login and a persona. No code, no tickets, no context.

**Persona:** {e.g. "A new user who has never seen this product, in a hurry, on a phone and a laptop."}

**Run each journey at 1440 and 390 width. For each step, take a screenshot and write what you expected and what happened.**

1. **Sign up to first value:** {start at the home page, create an account, reach the first useful result}
2. **The main action:** {the one thing users pay for}
3. **Come back later:** {log out, log in, find what you did before}
4. **Empty state:** {a brand-new account: is it clear what to do first?}
5. **Paying:** {upgrade, change plan, see the invoice}
6. **Team:** {invite someone, if the product has teams}
7. **This window's changes:** {plain goals for each new feature, written as what a user wants, not how it works}

**Report:**
- Blocker: a journey cannot be completed, or something from production is missing.
- Confusing: you got there but hesitated or guessed.
- Polish: looks off, but works.
