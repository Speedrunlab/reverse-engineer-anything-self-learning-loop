# The parity checklist: defining 100%

The checklist is the contract for a run. It is written from the examination's
inventory before the build starts, signed by the operator, and scored after
every iteration. This file says how to derive one for each class of product,
what every item must contain, and the mistakes that make a checklist useless.
The blank is `templates/parity-checklist.md`.

## Anatomy of an item

```
| id | area | behaviour | input → expected | verified by | evidence | tier | result |
```

- **id:** stable, short, never renumbered (`FMT-03`, `UI-12`, `CMD-07`).
- **area:** a screen, a format, a command, a subsystem; the grouping the gap
  report sorts by.
- **behaviour:** one sentence, in the user's or the caller's terms, about what
  the product does, not how it is implemented.
- **input → expected:** a concrete input (a fixture file the operator owns, a
  command line, a sequence of actions, a request) and the output or property
  expected. "Works correctly" is not an expectation. Where the expectation
  came from the examination and not from running the original, say so
  (`expected from evidence L-41`).
- **verified by:** the exact check that will run: a command and a diff, a
  round-trip, a side-by-side capture at a stated size, a timing with a
  threshold, a count. If it cannot be written, the item is not ready.
- **evidence:** the path the check will produce (`$B/review/<iteration>/FMT-03.diff`).
- **tier:** `must` (counts toward 100%), `should` (tracked and reported),
  `out` (named so it is not reopened).
- **result:** blank until a review fills it with `pass`, `fail` or
  `not testable (reason)`.

## Rules that keep it honest

- **One behaviour per item.** An item that bundles three behaviours hides two
  failures behind one pass.
- **Every must-have is testable now**, with fixtures the operator has already
  supplied. An item waiting on a fixture is `should` until the fixture exists.
- **Expected outputs come from the original or from the evidence, never from
  the build.** If the build is the only source of the expected value, the item
  tests nothing.
- **Tolerances are written down:** a numeric output states its precision, a
  rendering states the pixel-difference threshold and the size, a timing
  states the budget and the machine.
- **Tiers are the operator's call.** The agent proposes; the operator decides
  what counts toward 100%. Moving an item between tiers after signing is a
  new checklist version with a reason.
- **Out-of-scope items are listed, not omitted.** The fastest way to reopen an
  argument is to leave the exclusion unwritten.
- **Count the list.** 100% of 12 items is not parity with a product that has
  80 behaviours. The fresh-eyes reviewer's job (review.md) is to find what the
  list missed; a candidate it finds is added by the operator, not silently.

## Deriving the list per product class

### Desktop or mobile application
- One item per screen that exists in the original's resources: it opens, it
  shows the controls the resource names, each control does what its label and
  the observed behaviour say.
- One item per command (menu, shortcut, button) with a visible result.
- One item per supported document or file type, as a round-trip (open the
  original's sample, save, reopen; compare to the original's own output).
- Settings: each setting changes the behaviour it names and persists across a
  restart.
- Error paths: the messages the string table holds, each reached by the
  input that triggers it.
- Platform integration from the manifest: file associations, URL schemes,
  permissions actually used.
- Performance: the two or three operations users wait on, with a budget
  measured on the original.

### Command-line tool
- One item per subcommand and per documented flag, with exit code, stdout and
  stderr compared.
- Input and output formats, including stdin and stdout streaming.
- Exit codes and messages on bad input, missing files, permission errors.
- Idempotence and dry-run behaviour where the original has them.

### File format (reader, writer, converter)
- Magic, header fields, version handling (older and newer than current).
- Every field the examination found, with a fixture that exercises it.
- Round-trips in both directions with byte-level or semantic diff, stated
  which.
- Limits: maximum sizes, counts and nesting; behaviour at and beyond them.
- Malformed input: truncated, wrong checksum, wrong version, each handled as
  the original handles it (observed), never by guessing.

### Service, API or protocol
- One item per endpoint or message type: request shape, response shape, status
  codes, errors.
- Authentication and session behaviour as observed in the capture (never
  against the vendor's backend; against a stub that replays recorded
  fixtures).
- Ordering, retries, timeouts, idempotency keys.
- Rate limits and pagination.

### Firmware or device software
- Boot and update behaviour as observed statically (partition layout, update
  format, signature checks) and, only inside an approved live envelope, as
  measured.
- Each service the image starts and its configuration surface.
- Each protocol the device speaks, from the capture.
- Hardware abstraction: which peripherals the original drives, from the
  device tree or drivers present.

### A suite (several applications sharing formats and services)
- Write one checklist per application plus one for the shared layer (the
  formats, the shared service, the plug-in interface), and a top-level list
  of cross-application behaviours (a file created in one app opens in another).
- Score each list; the suite's score is the lowest, not the average, because
  a suite with one unusable member is not 100%.

## Scoring

`score = must-haves passing / must-haves total`, printed after every review.
Should-haves get their own line. Not-testable items count as not passing and
are listed with their reason. A run is **done** when every must-have passes in
two consecutive reviews and the operator accepts the residual list.
