# <module>

[For somebody about to change this. The README said what it is; this says how it
works and why it is built this way.]

## How it works

[Follow one real request, or one real input, all the way through. Name the actual
functions and files it passes through, in order, so the reader can open them.]

```
[input]  ->  [function in file]  ->  [what it is now]
         ->  [function in file]  ->  [what it is now]
         ->  [output]
```

[A flow somebody can follow with the files open beats a box diagram of
components. Keep it to the path that actually runs.]

## The parts

### [part]

[What it owns, and the one thing to know about it. If a number in here was chosen
rather than derived, say where it came from - a measurement, a constraint, or a
guess that has not bitten anybody yet. "A guess, never tested" is a useful thing
to write down.]

## Running it

[Covered in the README for the normal case. Here: the other ways. In production,
against real data, on one input for debugging, with logging turned up.]

## [Rebuilding / migrating / regenerating]

[If something here is generated - models, schemas, fixtures, clients - the
command that regenerates it, and what breaks downstream when it changes. Delete
this section if nothing is generated.]

## Things that will bite you

### [the trap]

[What happens, why it happens, and how to avoid it. Be specific about the
symptom: the next person arrives with the symptom, not the cause.]

[Anything that has already cost somebody a day belongs here the same day. This is
the section people come back for.]

## Where it is weakest

[The honest list: what it handles badly, what is held together with tape, what
would need rewriting if the load doubled. Known and written down is a decision.
Known and unwritten is a trap.]

## How it connects to everything else

[What calls this, what this calls, and over what - HTTP, a shared folder, the
database. Enough that somebody can tell what they break by changing it.]
