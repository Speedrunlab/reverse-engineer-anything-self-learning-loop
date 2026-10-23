# Parity checklist: <product> (version <n>)

Signed by: <operator name>, <date>. Changes after signing: new version, one line each in the log below.
Original: <real file name>, SHA-256 <digest>. Build: `$B` at <commit>.
Scope in: <areas>. Scope out: <areas, named so they are not reopened>.
Done = every `must` passes in two consecutive reviews and the operator accepts the residual list.

| id | area | behaviour | input → expected | verified by | evidence | tier | result |
|---|---|---|---|---|---|---|---|
| FMT-01 | <format> | <one sentence> | <fixture> → <output or property> | <command / diff / round-trip / capture + threshold / timing + budget> | `$B/review/<i>/FMT-01.*` | must | |
| UI-01 | <screen> | | | | | must | |
| CMD-01 | <command> | | | | | should | |
| X-01 | <area> | <behaviour deliberately out of scope> | | | | out | |

Score: <must passing> / <must total>. Should-haves: <passing> / <total>. Not testable: <ids and reasons>.

## Version log
- v1, <date>: signed.
- v2, <date>: <id> moved from should to must, reason: <one line>.
