# template

A starting kit. Copy it when you add a new part to the project, so every part
looks the same inside.

Nothing here runs. No build reads it, nothing imports it, nothing is deployed
from it. Empty folders and documents to fill in, that is all.

---

## What is a "module"?

One self-contained part of the project that does one job and can be worked on
without touching the others. The project has these already:

| module | its job |
|---|---|
| `backend/` | the server the phone talks to |
| `frontend/` | the Android app |
| `ai/muzzle/` | telling cows apart from a photo of the nose |
| `db/` | the database setup |

`ai/muzzle/` is the module this template was built from. If an explanation here
is unclear, open that folder and look at the real thing.

---

## How to use it

Adding a module for reading ear tags:

```
mkdir ai/eartag
cp -r template/module/. ai/eartag/
```

The `/.` matters. It copies the *contents* of `template/module`, hidden files
included, instead of the folder itself.

Then three things:

**1. Delete what you do not need.** This is the *most* a module ever has, not
the least. No web address? Delete `api/`. Trains nothing? Delete
`core/training/` and `docs/MODELS.md`. An empty folder makes the next person
hunt for something that was never there.

**2. Fill in the documents.** Each has lines in square brackets:

```
[One sentence. What this does, in the words someone outside the team would use.]
```

That is an instruction to you, not text to keep. Replace it, brackets and all. A
half-filled template is worse than none, because nobody can tell which parts
were decided and which were forgotten.

**3. Delete the `.gitkeep` files** from any folder once it has a real file in it.

---

## The folders

```
<module>/
├── core/              code more than one place needs
├── production/        the path that runs for real users
├── api/               the web layer, if it has one
├── experiments/       trying things out
├── configs/           settings, split by who edits them
├── datasets/          lists of data files
├── tests/             checks that it still works
├── tools/             scripts a person runs by hand
├── docs/              the documents, below
├── Dockerfile         how to build it into a container
├── requirements.txt   which libraries it needs
└── README.md          the front page
```

Two things to know before you put a file anywhere:

- **`core/` is for anything two places would have to agree on** - preprocessing,
  the model's shape, metrics, thresholds. One copy, everyone imports it.
- **`core/`, `production/` and `api/` may never import from `experiments/`.**
  This is the one rule that is actually enforced, by a test and by the
  Dockerfile.

Why it is shaped this way, what each `core/` subfolder holds, and the full
`configs/` and `datasets/` rules: **`module/docs/FOLDER_STRUCTURE.md`**.

---

## The documents

Six files, split by **who is reading**:

| file | who opens it | what they are asking |
|---|---|---|
| `README.md` | someone who just found the folder | what is this, and what will bite me |
| `docs/GETTING_STARTED.md` | someone who must run it today | what do I type |
| `docs/FOLDER_STRUCTURE.md` | someone adding a file | where does this go |
| `docs/DOCUMENTATION.md` | someone changing how it works | how does it work |
| `docs/MODELS.md` | someone who just trained something | how do I save it properly |
| `docs/ADR.md` | someone about to undo an old decision | why was it done this way |

Three habits keep them worth reading:

**The README stays on one screen.** Some people will never read anything else.
When it stops fitting, move the extra into `docs/DOCUMENTATION.md`.

**Fix the document in the same change that made it wrong.** Not later. An
out-of-date document is worse than a missing one, because someone trusts it,
acts on it, and loses a day.

**Write down where a number came from.**

> good: "121 of 130 cows, measured on `cattle_130_evaluation`, 2026-11-10"
> bad: "about 93%"

The first can be re-checked next year. The second stops being evidence the
moment anyone doubts it.

---

## Words you may not know

**module** - one self-contained part of the project, in its own folder.

**import** - when one file of code uses another file's code.

**registry** - the one folder at the top of the project (`registry/`) where all
trained models are kept. Models are never stored inside a module.

**version, as in `1.2.0`** - which generation of data it learned from, which
model it is, which training run. Full rules in `registry/VERSIONS.md`.

**symlink** - a shortcut pointing at another folder. A symlink named `current`
marks which model version is live, so switching is instant and easy to undo.

**commit, commit hash** - a saved change in git, and its unique ID
(`c7b2b79`). Recorded beside a model, it says exactly which code produced it.

**dirty tree** - edited files not yet committed. The commit hash only tells the
truth about a saved model if that code was committed, so saving refuses when the
tree is dirty.

**DVC** - tracks files too big for git. Git keeps a small pointer, `dvc pull`
fetches the real bytes.

**manifest** - a text file listing exactly which data files make up a dataset
version, so two people can confirm they used the same data.

**container, Dockerfile** - a container is a packaged-up program with everything
it needs to run. The Dockerfile is the recipe for building one.

**`.gitkeep`** - an empty placeholder. Git cannot store an empty folder, so this
gives it something to hold. Delete it once the folder has a real file.

**ADR (Architecture Decision Record)** - a short note recording one decision:
the situation, what was chosen, what else was considered, what it cost. Written
so someone later can tell whether reversing it is safe.

**threshold** - a cut-off used to make a yes/no call, such as "only count it as
a match above 0.65".
# template
