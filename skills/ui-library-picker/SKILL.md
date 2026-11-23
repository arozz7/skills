---
name: ui-library-picker
description: Use whenever a task adds, replaces or hand-builds a UI widget or dependency in an app: "add a toast", dialog, modal, menu, dropdown, tooltip, tabs, popover, drawer, command palette, animated number, icons, "add a dependency", "which library for". Invoke it before searching the code or writing any of it; it gives one answer from the user's UI library policy.
last-reviewed: "2026-11-21"
guidelines: v0.3
---
# ui-library-picker

Pick the right piece for a UI task from the user's short library policy. **Decisive: one answer, not a menu.** Learn-only: nothing from outside is installed by this skill.

## Steps
1. **Name the task**, not the library the user mentioned ("a transient, non-blocking message", not "Sonner").
2. **Check `package.json` first.** If a listed library already covers it, use that one. If a competitor is installed, say so and keep using it; do not churn dependencies unasked. Look at any hand-built version of the widget before replacing it.
3. **Apply the table.** Give one answer with one sentence of reason. If the task is not on the list, say so and label your answer as off-list.
4. **Before adopting anything new**, check its licence and version on the project's own page (several were not stated on the pages read when the policy was written). Tell the user the cost: a new dependency, its styling, its accessibility claims.

## The table (policy v0.1, approved by the user 2026-11-21)
| Task | Answer | Notes |
|---|---|---|
| Dialog, menu, tabs, switch, slider, tooltip, combobox | **Radix** if the app already has it; **Base UI** for new work (MIT, unstyled, WCAG 2.2 behaviour claims) | never hand-build focus management or popups; never both libraries in one app |
| Styled copy-in components | shadcn-style **only in Tailwind apps** | plain-CSS apps keep CSS variables and their own components |
| Toast (brief, non-blocking message) | **Sonner**, opt-in | React only; accessibility and theming are not stated on its page, so verify a live region and token styling; a message that must persist is a **banner**, not a toast |
| Command palette | **cmdk**, opt-in | only with many commands (about 15 or more, inferred; 8 is well below), React 18 or later, the app wires its own shortcut; otherwise a toolbar and the shortcut sheet |
| Drawer or bottom sheet | **Vaul**, opt-in, rarely | touch-style; **for a desktop app answer with a side panel** (an overlay panel in narrow windows) built on the app's existing primitives and layout, no library |
| Animated number | **NumberFlow**, only where a changing number matters | respects reduced motion by default; no non-Latin digits or right-to-left |
| Icons | **lucide** (the target set for every app) | in an app still on another set (for example Radix icons), a new icon **uses lucide**; say that two sets will coexist until the app migrates, match size (16 px in controls) and stroke weight, avoid mixing both in one toolbar where you can, and note the migration for the app's retrofit; never a third set |
| Marketing effects (animated gradients, hero effects) | **none** | decline; offer purposeful motion within the guidelines instead |
| Long lists (1,000 or more rows) | virtualise (the app's existing virtual list, for example Virtuoso) | do not render everything |

## Common mismatches to catch
A hand-built dropdown with manual focus handling; a hand-built toast or modal; a clickable `div`; two primitives libraries in one app; a second icon set; a library added for one component; an animation library added for a hover.

## Do not
Run third-party skill installers or CLIs such as `npx skills add` (the user learns from third-party skills and installs none of them), copy library source without checking its licence, or recommend a library the policy does not list without saying it is off-list.
