---
name: ui-review
description: Use when asked to review, audit or critique an existing screen, component or stylesheet in one of the user's desktop apps against the user's UI guidelines. Read-only; reports findings with file and line.
last-reviewed: "2026-11-21"
guidelines: v0.3
---
# ui-review

Review an existing UI against the checklist below, which is the whole rule set (guidelines v0.2). **Read-only: report, then ask before changing anything.** Refine, do not redesign. For a browser-tool visual check or mobile responsiveness use another skill; this one reads desktop app code.

## Steps
1. Read the app's `DESIGN.md` (the brief) and token file (often `tokens.css`; ask if you cannot tell which). If there is no `DESIGN.md`, or it records no guidelines version, the app is **before its retrofit**: its token names are the old ones, which is expected and not a defect (its `DESIGN.md` carries the map once it exists). Tailwind apps map their names in an `@theme` block; read it. If `DESIGN.md` records a version other than v0.2, say so.
2. Read the files asked for, plus the shell settings that set the window (minimum size, saved position) when sizing is in scope; note which you did **not** read.
3. Go through the checklist. Report only what the checklist says, with the file and line.
4. Rank: **Blocker** (keyboard, focus, contrast, data loss, and any control or content unreachable at the minimum window), **Should** (a rule broken: colour-alone status S, missing live region N, sizing findings W, missing states P), **Nit** (scale or polish: Z and off-scale Tailwind values, I font notes). An app that has no width classes or minimum window yet is a **Should** for its retrofit, not a Blocker and not a defect of the screen. A shell minimum window above 800 px wide or 560 px tall is a **Should**: record it as a deviation (and say whether it fits a 1280 x 720 logical screen, which leaves about 640 px of height).
5. Say what you did not check (running app, other themes, window sizes, scalings, real data).

## Checklist (ids for reference)
- **F focus:** a visible focus style on every interactive element (a 2 px accent ring offset 2 px); no `outline: none` without a replacement.
- **K keyboard:** controls are real buttons, links or inputs; no `onClick` on a `div` or `span`; shortcuts discoverable.
- **N names:** icon-only controls have an accessible name; inputs have labels; dynamic status text sits in a live region.
- **S status:** symbol plus word plus colour, never colour alone.
- **C colour and tokens:** no raw hex or one-off colour in components; tokens only (the 18: surface-0..3, border, border-strong, text-primary/secondary/tertiary, accent, accent-contrast, accent-subtle, success, warning, danger, info, scrim, stage); if the app has a tokens test, say whether it covers the pairs used.
- **T themes:** `data-theme` and `color-scheme` set; nothing breaks in a light theme; accent is a dark and a light value; the image stage is a neutral grey.
- **H hover and selected:** hover is a faint step, selected is the accent tint plus a bar and reads stronger than hover.
- **M motion:** only `transform` and `opacity`; no `transition: all`; 100 to 200 ms; reduced motion respected.
- **P states:** empty, loading and error exist for every list and screen; error text says how to fix it.
- **Z size:** targets at least 24 px; type, spacing and radius on the scale (11, 13, 14, 16, 20 px; 4 px spacing base; radius 4, 8, 12). Tailwind arbitrary values such as `text-[11.5px]` or `h-[27px]` step off the scale.
- **I icons and font:** one icon set (lucide target); Inter or a recorded exception (before the app's retrofit, a different font is a retrofit note, not a finding); tabular figures for aligned numbers.
- **W window sizes** (needs a declared minimum window, default 640 x 480):
  - **W1** the shell enforces a minimum window and the app declares it (in its `DESIGN.md`; with none, the shell's own setting counts as the declared minimum and the missing declaration is a retrofit item). Use the declared minimum, or the 640 x 480 default when none is set, for W2 and W3;
  - **W2** fixed widths (sidebar, inspector, panels) whose sum plus a content minimum exceeds the minimum window, with no collapse (inspector to an overlay below 1200, sidebar to a rail and toolbar overflow below 840);
  - **W3** fixed pixel heights on text containers; `100vw`; `overflow: hidden` on the page; absolute pixel positioning of layout items; a minimum width above the declared minimum window;
  - **W4** the page scrolls sideways, or chrome scrolls with the content; a wide table with no scroll container of its own;
  - **W5** dialogs, menus, popovers or toasts that can exceed the window (no maximum size of window minus 16 px, no own scroll);
  - **W6** long names with no truncation or no way to see the full value; on large monitors, grids that stretch instead of adding columns, reading text wider than about 80 characters;
  - **W7** photo or canvas views: "100 percent" is not one image pixel per device pixel (`devicePixelRatio` ignored), a canvas not rendered at device resolution, or no re-render when the window moves between monitors;
  - **W8** window state restored without checking that the saved position is on a visible monitor; size, position or maximised state not remembered.
  - **W9** something that leaves the screen at a smaller size with no visible, keyboard-reachable way back (navigation removed instead of moving to a rail, menu button or drawer) is a **Blocker**; the collapse order is rail, then drawer, then "More" menu, then strips shrink and go, then a bottom sheet under 800 px.
- **B break test** (guides your thinking; report only a concrete finding): a very long name, thousands of rows, nothing at all, a failed load. A name or value shown with no truncation is W6.
- **U consistency test** (same): if a state is not in the screen (an error, a disabled), would it reuse tokens or invent a value?

## Output format
```
<file>
  L<line> [Should] <id> <what is wrong> -> <the fix>
✓ pass  <file>
Not checked: <list>
```
Terse. One line per finding and per rule id (a line that breaks two ids gets two lines). The severity word (`Blocker`, `Should`, `Nit`) replaces `[Should]`. A finding with no line (a config file, a feature that is absent) names the file and omits the line. If a file is clean, say `✓ pass <file>` and nothing else; `Not checked:` is **one trailing line for the whole review**, not per file. A rule that does not apply, or that passes, is not listed. **Never claim a layout works at all sizes from reading the code**; list window sizes and scalings under Not checked unless the app was run at them.

## Do not
Report a rule that is not in this checklist; call old token names a defect before the app's retrofit; recommend a library (hand over to `ui-library-picker`); design for mobile; edit files; invent findings to look thorough.
