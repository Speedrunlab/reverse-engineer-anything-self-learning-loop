# The review: measuring the build against the original

The review runs after every build iteration. It turns the checklist into a
score with evidence, and it hunts for what the checklist missed. It is never
skipped, never shortened to "the items I changed", and never scored from
memory: every result has a file behind it.

## Where the review lives

`$B/review/<iteration>/` holds that iteration's evidence, one file per item
(`FMT-03.diff`, `UI-12-original.png`, `UI-12-build.png`, `CMD-07.txt`), the
gap report, and `score.txt`. Evidence is kept for every iteration, so a
regression can be shown, not asserted.

## Techniques, by what the item checks

- **Output equality.** Run the same input through the original (or take the
  original's recorded output from the examination) and through the build;
  compare with `diff` for text, `cmp` or SHA-256 for bytes, a structural diff
  for JSON or XML (sorted keys, normalised whitespace, so a formatting
  difference is not a behaviour difference). Store both outputs and the diff.
- **Round-trip.** Build reads the original's file, writes its own, the
  original's reader (or the format's reference reader) reads that; then the
  reverse. Compare semantically where the format allows harmless reordering;
  byte-for-byte where the checklist says so.
- **Side-by-side rendering.** Same window or viewport size, same input, same
  state; capture both; compare with a pixel diff and the threshold the item
  states; keep both captures and the diff image. Fonts, anti-aliasing and
  platform chrome differ by design: the threshold and a masked region are part
  of the item, decided when it was written, not at review time.
- **Timing and limits.** Measure on the stated machine with the stated input,
  three runs, report the median; a limit is probed at the limit and one past
  it, and the behaviour beyond it (refuse, truncate, error message) is
  compared, not just the limit's value.
- **Error paths.** Each error item is reached by the input that triggers it
  and the message, exit code or dialog is compared with what the examination
  observed or the string table holds.
- **Static-mode expectations.** When the original cannot be run, the expected
  side comes from the examination's evidence (a format spec recovered from
  the headers, a string table, a recorded capture). The item says so
  (`expected from evidence L-41`), and the fresh-eyes reviewer checks that the
  evidence actually supports the expectation.

## The fresh-eyes pass

A reviewer that did not write the batch: a subagent with a clean context, or
the agent itself after a reset, given only the checklist, the examination's
inventory and the build. It:

1. re-runs a sample of the items marked pass (at least a fifth, chosen from
   different areas) and reports any it cannot reproduce;
2. uses the build as a user would for ten minutes against the inventory of
   screens, commands and formats, and lists every behaviour the original has
   that no checklist item names;
3. reads the gap report's severities and challenges any that look soft.

Its findings go into the gap report as **candidates** (new items for the
operator to accept into a tier) and as **disputed passes**, which revert to
fail until re-proven. It never edits the checklist itself.

## Regression sampling

Every review re-runs the items the plan named, every item in the same areas,
and a rotating fifth of all passing must-haves, so each item is re-run at least
once every five iterations. An item that flips from pass to fail is a
regression: it goes to the top of the next plan and is fixed before new work.

## The gap report

Template: `templates/gap-report.md`. It holds, in this order: the score and
its delta; regressions; failing must-haves by severity (**blocks use**,
**wrong result**, **cosmetic**) each with its evidence path and a one-line
diagnosis when the cause is known (a guess is marked as such); not-testable
items with the reason and what would make them testable; should-have results;
the fresh-eyes candidates and disputed passes; and one line on what the next
iteration should attack first.

## What a review is not

- Not a code review of the build: that follows the organisation's normal
  process. The review here measures behaviour against the original.
- Not a place to decide scope: a behaviour the original has but the checklist
  omits is a candidate for the operator, never a reason to lower the bar or
  to quietly widen it.
- Not a reason to touch the original or `$W`: the review runs on fixtures and
  the build; if it needs a new observation, that is an examination step under
  the gate.
