# <module>

[One sentence saying what this does, in words someone outside the team would
use. Not the technology - the job. Example: "Telling cows apart from a
photograph of the nose."]

## What it is

[Two or three sentences. What problem it solves and roughly how. Assume the
reader has never seen this folder. Avoid words only this team uses, or explain
them the first time.]

[Then say what it is **not**. This is the most useful paragraph in the file,
because it stops the next person asking this module for something it was never
built to do.

Real example from ai/muzzle: "It is not a cattle detector and not a breed
classifier. It answers one question: which of the cows we already know is this."]

## Run it

```
[the one command, which someone can copy and paste]
```

[Then anything surprising about starting it:
 - how long it takes before it is ready
 - what else must already be running
 - what a slow first start looks like, so nobody kills it thinking it hung

If getting it running takes more than a command or two, put the full steps in
docs/GETTING_STARTED.md and make this a link to it.]

## Where things are

| folder | what is in it |
|---|---|
| `core/` | [the shared code everything else uses - name the important files] |
| `production/` | [the path that runs for real users] |
| `api/` | [the web layer, if there is one] |
| `configs/` | [the settings] |
| `experiments/` | [trying things out. Never imported by the three above] |

[Only list folders somebody needs to find. Do not list the whole directory -
they can run `ls`. The point of this table is answering "which folder do I open
to do X".]

[If this module has a shared `core/`, say in one line why it is shared: the
things in it must have exactly one definition, because more than one place
depends on them agreeing. See docs/FOLDER_STRUCTURE.md.]

## Before you change anything

- **[The thing that looks safe and is not.]** [What happens, and what to do
  instead.]
- **[A rule that is not visible in the code.]** [Where it comes from.]

[This section is why this file exists. Write what you would say out loud if you
were sitting next to someone about to change this.

Be concrete about the symptom, because that is what the next person arrives
with. "Replacing the embedding model makes every already-registered cow
unmatchable, and it does not error - it just starts giving wrong answers" is
useful. "Be careful with the models" is not.

If you cannot think of anything yet, leave one line saying so and add the first
real entry the day something bites you.]

## Docs

    docs/GETTING_STARTED.md    from nothing to it running on your machine
    docs/FOLDER_STRUCTURE.md   what goes where, and why core/ is shared
    docs/DOCUMENTATION.md      how it works inside
    docs/MODELS.md             saving a trained model so it can be rebuilt
    docs/ADR.md                decisions, and what each one cost

[Delete any line for a file this module does not need. A module that trains
nothing has no MODELS.md.]
