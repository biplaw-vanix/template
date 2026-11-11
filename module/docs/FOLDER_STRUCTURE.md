# Folder structure

Every module has the same shape, so that knowing one means knowing all of them.

New to the project? `template/README.md` at the top of the repo explains this
shape from scratch and has a list of terms at the end - symlink, manifest, DVC,
dirty tree, and the rest.

```
<module>/
├── core/              THE SHARED LIBRARY. Everything below imports this
│   ├── data/              preprocessing.py, dataset.py
│   ├── architectures/     the networks themselves
│   ├── models/            loading and running them
│   ├── features/          input -> numbers
│   ├── evaluation/        metrics and thresholds
│   ├── training/          the training steps
│   └── utils/             config.py, savemodel.py
├── production/        the one prediction path. Thin: load, call core/, return
├── api/               HTTP. Routes and schemas, no logic of its own
├── experiments/       exp-001, exp-002, ... anything goes inside one
├── configs/           every setting, split by who changes it
├── datasets/          manifests in git, bytes outside it
├── tests/             including test_boundaries.py
├── tools/             one-off scripts for humans, not imported by anything
├── docs/              README is the front door; these four are the rest
├── Dockerfile
├── requirements.txt
└── README.md
```

## core/ is the point

**Anything two places depend on lives in `core/`.**

Preparing the data is the clearest example. Three different places have to do it:
the training code, the live code serving real users, and any experiment. They
must all resize, crop and normalise in *exactly* the same way, because those
steps decide what a saved number means.

If each kept its own copy, one day somebody changes one of them a little. Now the
model was taught on data prepared one way and is being shown data prepared
another. It quietly gets worse and **nothing reports an error** - which is the
hardest kind of bug to find.

So `core/` is not a drawer for leftovers. It is the set of things that must have
exactly one definition.

| | |
|---|---|
| goes in `core/` | preprocessing, architectures, metrics, thresholds, anything that defines what a saved artefact means |
| stays out | HTTP handling, CLI parsing, file layout of one script, anything only one caller needs |

## The one rule that is enforced

> `core/`, `production/` and `api/` may never import from `experiments/`.

That is the line between the person trying things and the person keeping the
service up. Inside `experiments/` nobody needs permission. When something works
it is **rewritten into `core/` with a test** - not imported.

Two things hold the line, because a rule nobody checks is a comment:

- `tests/test_boundaries.py` fails the build if the import appears.
- The `Dockerfile` does not copy `experiments/`, so a running service
  physically cannot reach it.

## experiments/

One folder per experiment, numbered, never renamed:

```
experiments/
├── exp-001-<short-name>/
├── exp-002-<short-name>/
└── README.md
```

`exp-001` is an **identifier**. It goes in the model metadata of anything the
experiment produces, so a model in the registry can be traced back to the run
that made it. That only works if the number is never reused and never renamed -
see `MODELS.md`.

## configs/

Split by **who changes it**, not by component. Somebody tuning training should
not have to read past the data paths to find the learning rate.

| file | holds |
|---|---|
| `data.yaml` | where the data is, which version |
| `preprocessing.yaml` | sizes, interpolation, normalisation. Changing these invalidates saved artefacts |
| `model.yaml` | architecture and shape |
| `training.yaml` | epochs, batch size, learning rate |
| `inference.yaml` | thresholds and runtime settings |

Every number a run depends on is in one of these. A number that only exists as a
default in code cannot be recorded in the model metadata, so the run cannot be
reproduced.

## datasets/

The **lists** of data files are kept in git. The image files themselves are not -
they are far too big for it.

`datasets/manifests/` holds those lists. One is called a **manifest**: a text
file naming exactly which files a dataset version contains, so two people can
confirm they trained on the same data.

The bytes are tracked separately by **DVC**, a tool for large files. Git stores a
small pointer; DVC fetches the real thing on request:

```
git clone ...     you get the code and the manifests
dvc pull          you get the actual images
```
