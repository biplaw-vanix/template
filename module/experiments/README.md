# experiments

Anything goes in here. Notebooks, dead ends, half-finished ideas, a script that
only runs on one machine. Nobody needs permission and nothing needs tidying.

Two rules, and they are the only two.

## One folder per experiment, numbered

```
exp-001-<short-name>/
exp-002-<short-name>/
```

The number is an **identifier**, not a label. It goes into the `model.json` of
anything the experiment saves into the registry, so a model found there can be
traced back to the run that made it and to whatever it was being compared
against.

That only works if:

- **numbers are never reused** - the next one is one higher than the highest
  that has ever existed, including deleted ones
- **folders are never renamed** - the name is referred to from outside
- **a dead end keeps its number** - delete the contents if you like, but leave
  the folder and a line in it saying what was tried and why it was dropped. A
  gap in the numbering reads as a lost experiment

Put a short `README.md` in each: what you were testing, what happened, and
whether anything came out of it. Three lines is enough. An experiment whose
result nobody wrote down has to be run again.

## Nothing outside imports this

`core/`, `production/` and `api/` may never import from here. When something in
an experiment works, it is **rewritten into `core/` with a test** - not imported.

This is checked, not trusted: `tests/test_boundaries.py` fails the build if the
import appears, and the `Dockerfile` does not copy this folder at all, so a
running service cannot reach it.

The direction is one-way on purpose. Experiments import from `core/` freely -
that is what `core/` is for.
