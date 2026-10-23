# Run ledger: <product>

Append-only. Each entry starts with `prev:` and the SHA-256 of this file as it stood before the entry (`prev: genesis` for entry 1). Never edit a past entry; add a correcting one.

## Entry 1
prev: genesis
intake: <target(s) and digests; authority basis, approver, reference, expiry, security contact; purpose and boundary (may not reuse / may reuse); mode; data handling; $W, $B, budgets, stop conditions>
skill: reverse-engineer-anything-self-learning-loop <version>, SKILL.md SHA-256 <digest>
examination: preflight results; original name, size, digests of source and copy; analyst; host
checklist: version 1 signed by <operator>, <date>, <must total> must-haves

## Entry <k>: iteration <i>
prev: <sha256>
plan: <path>  changes: <commit range in $B>
score: <before> → <after>; regressions: <ids>; moved by: <what worked>; did not move: <what did not>
lessons: <staged patches, one line each, with content hashes>  stops: <none / reason>

## Entry <k+1>: close
prev: <sha256>
done: <yes: two consecutive full passes on <date>, <date> / no: honest score and residual list>
cleanup: <extractions and secret-bearing files deleted by path; nothing live left running>; data-handling confirmation: <operator, date>
hand-over set: <file: digest, one per line>
