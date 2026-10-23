# Fix recipes

Fixes that moved a score, written as the cause they addressed and the rule
that prevents it, one per lesson, in the imperative. The learning loop adds
to this file (staged for the operator); keep it organised by cause, never by
run. A recipe that only ever applied to one product is not a recipe yet.

## Format and encoding

- **A byte diff on a file the build writes is almost always an encoding or
  padding rule, not a logic bug.** Compare the headers field by field before
  reading the writer's code: look for little- versus big-endian lengths,
  alignment padding, a trailing newline, a BOM, CRLF, a checksum computed
  over a different span than assumed.
- **State the normalisation the diff uses.** Unicode normalisation (NFC vs
  NFD) and line endings make two correct outputs differ; decide in the
  checklist item whether the comparison is byte-exact or normalised, and
  normalise both sides the same way.
- **Round the way the original rounds.** A numeric field that differs in the
  last digit is a rounding mode, a float-versus-double, or a locale decimal
  separator; measure the original's behaviour at a half-way value and match
  it, then write the tolerance into the item.
- **Version-gate format behaviour.** When a format has versions, a fixture
  per version beats a single "current" fixture; readers often accept older
  versions with defaults the examination can recover from the header.

## Behaviour and state

- **Persisted settings fail on the second launch, not the first.** Test a
  setting by changing it, restarting, and reading it back; an item that only
  checks the in-session value hides the bug.
- **Default values live in two places.** The original's defaults are in its
  resources or config schema as well as in code; when a build's output
  differs with no input, compare the defaults first.
- **Time and locale are inputs.** Fix the timezone, locale and clock in both
  runs of a comparison, or a correct build fails at midnight and on the first
  of the month.
- **Order is a behaviour when the user can see it.** A list, a menu or a
  file's record order that the original keeps stable is a must-have item,
  not an implementation detail; sort the way the original sorts (observed),
  not the way the language sorts by default.

## Errors and limits

- **Reproduce the original's refusal before improving on it.** An error path
  that the build handles "better" still fails parity when a caller depends
  on the original's exit code or message; match first, and record the
  improvement as a design decision for the operator.
- **Probe one past the limit.** A limit item passes at the limit and fails
  one past it unless the behaviour beyond (truncate, refuse, message) was
  observed and implemented; measure both on the original.

## Rendering

- **Mask platform chrome, compare content.** A side-by-side capture fails on
  window decorations and fonts before it fails on behaviour; the mask and the
  threshold belong in the item, decided when it was written.
- **Same size, same state, same data.** Two captures at different window
  sizes or scroll positions compare nothing; script the state before the
  capture, on both sides.

## Process

- **A fix that does not flip its named items is aimed at the wrong cause.**
  Re-read the evidence, reproduce the failure by hand, and only then change
  code again; two blind fixes in a row is a stop-and-ask signal.
- **A regression is a missing regression check.** When an item flips back,
  add the fixture that would have caught it to the area's regression set
  before fixing the code.
- **Batches that mix areas hide which change moved the score.** Keep a batch
  to one or two areas so the iteration log says what worked.
