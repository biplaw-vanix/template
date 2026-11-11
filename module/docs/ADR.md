# Decisions

One entry per decision that would be expensive to undo, newest last. Numbered so
they can be referred to in code and in conversation: "see ADR-0004".

Write one when you chose between real options and the reason will not be obvious
from the code. Not for every commit - for the handful of choices somebody will
later want to reverse, and should be able to reverse knowing what it cost.

**Never edit a decision to make it look right.** A decision that turned out badly
is the most useful entry in the file. Add a new one that supersedes it and say so
in both.

---

## ADR-0001 - [the decision, as a statement]

[One line of what was decided. Present tense: "The phone does the whole pipeline,
not just the camera".]

**Context.** [What was true that forced a choice. The constraint, the measurement,
the thing that broke. Enough that somebody who was not there understands why this
was even a question.]

**Decision.** [What was chosen, precisely enough to check the code against it.
Exact numbers if there are numbers.]

**Alternatives.** [What else was considered and why it lost. This is the first
thing somebody reversing the decision will want, and the easiest thing to forget.]

**Consequences.** [What this costs. What it rules out. What now has to stay true
for it to keep working - and what breaks if it stops being true.]

[Measured claims get their number and method: "31 of 40 whole-frame, 37 of 40
with the middles, on a herd it was not trained on". A number without a method
cannot be rechecked, so it stops being evidence the moment anybody doubts it.]

---

[Copy the block above for the next one. Keep them in order and never renumber:
the numbers are referred to from the code.]
