# Saving models

A model nobody can rebuild is a liability. Everything here exists so that
"where did this model come from" has an answer a year later.

Models are **not** kept inside this module. They go in one place at the top of
the project:

```
registry/<module>/<model>/<version>/
```

The code never names a version. Instead it reads a shortcut called `current`:

```
registry/muzzle/detector/1.1.0/
registry/muzzle/detector/1.1.1/
registry/muzzle/detector/current  ->  points at 1.1.1
```

That shortcut is a **symlink** - a file that is really just a pointer at another
folder. Switching which model is live means re-pointing it, which is instant and
just as easy to undo.

## Versions are x.y.z

The rules are in `registry/VERSIONS.md` and are not repeated here, because two
copies of a numbering scheme is how the two stop agreeing. In short:

| | means | changes when |
|---|---|---|
| **x** | the dataset generation | data is added, removed or relabelled |
| **y** | which model | e.g. `1` = detection, `2` = embedding |
| **z** | the retrain | every training run, +1 |

Two things worth knowing before you pick a number: **x going up resets z**, and
**a version folder is never edited** - a retrain is always a new folder.

## What is saved beside the model

A `model.json` in the version folder. The reproducibility claim is
*code + settings + data*, and if any one of the three is missing, the claim is
void:

| field | why it is there |
|---|---|
| `version` | the x.y.z it was given |
| `model` | which model in this module |
| `experiment` | the `exp-NNN` that produced it, or the training step's name |
| `dataset` | which dataset version, plus a fingerprint of each input file so the exact data can be confirmed |
| `code` | the git commit it was built from, the branch, and whether anything was uncommitted |
| `params` | the config values this run actually used |
| `metrics` | what was measured, with the set it was measured on |
| `environment` | python, the frameworks, the machine |
| `files` | what is in the folder |
| `note` | one line on what changed, written by whoever ran it |

The three identifiers are the ones people forget, and they are the ones that
matter:

- **`code`** - the **commit hash**, the unique ID git gives every saved change
  (it looks like `c7b2b79`). Answers "what code made this".
- **`dataset`** - a version and per-file fingerprints. Answers "was this the same
  data".
- **`experiment`** - `exp-007`. Answers "where was this tried, and what else was
  tried alongside it". Without it a registry entry is an orphan: you can rebuild
  it, but you cannot find out what it was being compared against or why it won.

## Two rules the save obeys

**Nothing is saved unless you ask.** Training behind `--save`; without the flag
it trains, prints and leaves the registry alone. Most runs are not worth keeping,
and a registry full of them is as useless as an empty one.

```
python -m core.training.train_x                  # trains, saves nothing
python -m core.training.train_x --save --note "..."   # writes a new version
```

**Uncommitted changes refuse to save.** A tree with edits you have not committed
yet is called a **dirty tree**. The commit hash only tells the truth about which
code produced the model if that code was actually committed - so saving from a
dirty tree would write a lie into the metadata. The save stops and lists what is
uncommitted.

`--allow-dirty` exists for the run at two in the morning. It saves the actual
diff as `uncommitted.patch` in the version folder rather than pretending - so the
code is still recoverable, just not by `git checkout` alone.

Check the tree **once per run**, before writing anything. The first save creates a
registry folder, which makes the tree dirty, which would make a second save in
the same run refuse.

## Deploying is one symlink

```
ln -sfn <version> registry/<module>/<model>/current
```

Saving does not deploy. Rolling back is pointing `current` back, and `git diff`
shows either as one line.

## Before replacing a model

Ask what already depends on its output. If stored values were produced by the old
model and are compared against new ones, replacing it invalidates them - and that
is usually silent. It does not error; it just gets answers wrong.

Write which models are safe to swap and which are not in `ADR.md`, with the
measurement. "Safe" without a number is a guess.
