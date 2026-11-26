# Plan: {ticket id} {title}

Date: {YYYY-MM-DD}  Lane: {callsign}

**Premise check:** {is the ticket's explanation right? if not, what is actually happening}

**Found:**
- {fact with number and query, or file:line}

**Will change:**
- {file or area}: {what}

**Proof it worked:**
- Test: {what goes red before, green after}
- On the test copy: {page, click, what is seen}
- After release: {the real-world check}

**Mockup** (new or redesigned screens only): {image: today's screen and the proposed one side by side. How: build a throwaway page from the real components in docs/design.md, screenshot it with Playwright at 1440 and 390, and put it next to a screenshot of today's screen. Never a description in words. Founder approves before any code.}

**Customers will see:** {list, or "nothing visible"}

**Open decisions:**
1. {question} Recommend: {option}, because {one line}. Owner: {founder if product, orchestrator if engineering}

**Risk and rollback:** {what could break, how it is undone}

Status: HOLD until GO.
