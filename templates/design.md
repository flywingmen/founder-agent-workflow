# Design rules

<!-- Written by the founder-agent-workflow skill from the existing app: read the theme, CSS and components, then fill this in. If there is no UI yet, propose a short set and get the founder's yes. Keep it to one page. Agents read it before any UI work. -->

Product feel: {one line, e.g. "calm and plain, like a good bank app"}
Reference: {a screen or product the founder points to as "like this"}

## Colors
| Use | Value | Token or class |
|---|---|---|
| Primary (buttons, links) | {#hex} | {token} |
| Text | {#hex} | {token} |
| Muted text | {#hex} | {token} |
| Background / surface | {#hex} / {#hex} | {token} |
| Success / warning / error | {#hex} / {#hex} / {#hex} | {token} |

Never add a color that is not on this list.

## Type and spacing
- Font: {family}. Sizes in use: {e.g. 14, 16, 20, 28}. Nothing in between.
- Spacing scale: {e.g. 4, 8, 12, 16, 24, 32}. Corner radius: {value}.
- Page width: {max width}. Works at 390 px wide without sideways scrolling.

## Components to reuse
| Need | Use | Where it lives |
|---|---|---|
| Button | {component} | {path} |
| Form field | {component} | {path} |
| Table or list | {component} | {path} |
| Modal or dialog | {component} | {path} |
| Empty state | {component} | {path} |
| Loading | {component, e.g. skeleton} | {path} |
| Error message | {component} | {path} |

A new component or style needs the founder's yes, same as new copy.

## Every screen has
- An empty state that says what to do first.
- A loading state that keeps the layout still.
- An error state in plain words, with what to do next.
- Room for long text: names, emails and numbers that are three times longer than the demo data.

## Accessibility basics
- Text contrast at least 4.5 to 1.
- Tap targets at least 44 px on phone.
- Every control reachable and usable from the keyboard, with a visible focus.

## Copy
- Tone: {e.g. short, plain, friendly, no jargon}.
- Words we use: {e.g. "workspace", not "account"}.
- All new user-facing words need the founder's yes.
