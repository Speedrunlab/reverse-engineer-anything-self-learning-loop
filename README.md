# reverse-engineer-anything-self-learning-loop

**Reverse Engineer Anything with a Self-Learning Loop: examine a product you are authorised to rebuild, build your own version, and keep improving it until every must-have on a written checklist passes.**

Free and open source · MIT Licence · Built by [Speedrun AI Labs](https://www.speedrunlab.ai), Sydney, Australia

Inspired by REA (Reverse Engineer Anything) by @morluto: [github.com/morluto/rea](https://github.com/morluto/rea). Not affiliated.

---

## The problem

Give an agent a shipped product and ask for an equivalent, and you get something that is "80% there". It looks right, the obvious paths work, and then it stops, because nobody wrote down what 100% means and nothing measures the build against the original. The last 20% is where the product lives.

This skill is that last 20%, as a method. It combines two Speedrun AI Labs skills into one loop: the examination discipline of Speedrun's [Reverse Engineer Anything skill](https://github.com/Speedrunlab/reverse-engineer-anything) (our own skill, separate from @morluto's REA) and the learning loop of [Hermes Agent as a Skill File](https://github.com/Speedrunlab/hermes-agent-as-a-skill-file), and adds the part neither has: a completion loop with a written definition of done.

---

## What it does

- **Defines 100% before building.** The examination's inventory becomes a parity checklist: one testable behaviour per item, a concrete input and expected output, the exact check that will verify it, a tier (must-have, should-have, out of scope). The operator signs it. The score of a run is must-haves passing over must-haves total.
- **Examines safely.** Static-first, evidence-led, untrusted input: preflight, read-only copies and digests, capped extractions, escape checks, no secrets on screen, content treated as data, never as instructions. Notes record behaviour, never protected expression.
- **Builds its own design.** A walking skeleton across every must-have path first; clean-room discipline (whoever writes code does not have decompiled text open); fixtures from the operator; stubs instead of vendor backends; design decisions recorded apart from observations.
- **Reviews against the original with evidence.** Same input to both, outputs diffed or hashed, formats round-tripped, screens compared side by side at a stated threshold, timings measured. Pass, fail or not testable, with a file behind every result. A fresh-eyes reviewer re-scores a sample and hunts for what the checklist missed.
- **Plans, improves, repeats.** Gaps ordered by impact and dependency, a batch one iteration can prove, regression sampling so nothing silently breaks, an iteration log that says what moved the score.
- **Knows when to stop.** Done is every must-have passing in two consecutive reviews. Stop and ask when the score stalls for two iterations, a gap would need protected material or a vendor backend, the budget is spent, or any examination stop fires. Never declares 100% on an untested item.
- **Learns from every iteration.** The self-learning loop saves checklist patterns, review techniques, routes, tool recipes and fix recipes into this skill, staged for the operator's approval, so the next product starts further along. It never captures failures as rules, secrets, or anything from the target.

---

## Install

Put the folder in your agent's skills directory.

### Claude Code
```bash
mkdir -p ~/.claude/skills
git clone https://github.com/Speedrunlab/reverse-engineer-anything-self-learning-loop.git ~/.claude/skills/reverse-engineer-anything-self-learning-loop
```
Then run: `/reverse-engineer-anything-self-learning-loop`

### OpenClaw
```bash
mkdir -p ~/.openclaw/skills
git clone https://github.com/Speedrunlab/reverse-engineer-anything-self-learning-loop.git ~/.openclaw/skills/reverse-engineer-anything-self-learning-loop
```

### Any other agent
Copy the folder into its skills directory, or load `SKILL.md` into its instructions and keep `references/` and `templates/` where the agent can read them. Then map the capabilities in the "Map it to your agent" table once.

### Companions (optional)
The skill stands alone. Install [Reverse Engineer Anything](https://github.com/Speedrunlab/reverse-engineer-anything) beside it for the full route guides, the pinned toolchain and the live-analysis isolation rules, and [Hermes Agent as a Skill File](https://github.com/Speedrunlab/hermes-agent-as-a-skill-file) for the full learning loop (memory stores, curation, installing skills from outside). When they are present, the loop defers to them for examination depth and for memory.

---

## Quick start

In Claude Code, run `/reverse-engineer-anything-self-learning-loop`, then give it: the product you're authorised to rebuild, who approved it, what's in scope, and a working folder. It writes a parity checklist for you to sign, builds, tests against the original, and reports a score each iteration.

Solo? Record yourself as the approver and note it; the gate is there to make authorisation explicit.

---

## What's in the box

```
SKILL.md                          the loop: gate, examine, define 100%, build, review, plan, improve, stop, learn, finish
references/
  examine.md                      safe handling, identification, routes and tools, inventory passes, the ledger, live rules
  parity-checklist.md             item anatomy, honesty rules, how to derive the list per product class, scoring
  review.md                       techniques per item type, the fresh-eyes pass, regression sampling, the gap report
  learning-loop.md                what gets learned and where, the review prompt, the shape of a lesson, never capture, approval
  fix-recipes.md                  fixes that moved a score, by cause; grows through the loop
templates/
  parity-checklist.md  gap-report.md  iteration-plan.md  run-ledger.md
```

---

## What it needs

An operator-supplied analysis environment for the examination (no network, no credentials; a disposable VM for any live step), a separate build directory under version control, the open-source tools the target's route needs (installed deliberately, at pinned versions; the skill installs nothing itself), an approver independent of the requester, and fixtures the operator owns.

---

## What it is not for

Cloning a third party's product, defeating access controls, lifting protected assets, or sending traffic to a vendor's backend. The gate asks for the authority record and the reuse boundary before any tool runs, and the reconstruction is the operator's own design, informed by observed behaviour.

---

## Credits

Written and maintained by Speedrun AI Labs. The examination method is adapted from Speedrun's Reverse Engineer Anything; the learning loop from Speedrun's Hermes Agent as a Skill File, itself a derivative of [Hermes Agent](https://github.com/nousresearch/hermes-agent) by Nous Research (MIT licence; attribution, not affiliation).

The name and idea are inspired by REA (Reverse Engineer Anything) by @morluto (github.com/morluto/rea, MIT); this is an independent skill, not affiliated with or endorsed by REA.

## Part of the Speedrun AI Labs Skill Library

Other skills at [speedrunlab.ai/#skills](https://www.speedrunlab.ai/#skills): [/reverse-engineer-anything](https://github.com/Speedrunlab/reverse-engineer-anything), [/hermes-agent-as-a-skill-file](https://github.com/Speedrunlab/hermes-agent-as-a-skill-file), [/deep-audit](https://github.com/Speedrunlab/deep-audit), [/read-before-write](https://github.com/Speedrunlab/read-before-write), [/no-slop-writing](https://github.com/Speedrunlab/no-slop-writing), [/no-slop-writing-au](https://github.com/Speedrunlab/no-slop-writing-au).

## Licence

MIT. Free to use, modify, and distribute.

*Built in Sydney by Speedrun AI Labs.*
