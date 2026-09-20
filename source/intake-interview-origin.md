# Where this member came from

`mobile-intake-interviewer` was not converted from an existing markdown skill, the way
the other nine members of this department were. It was added after the department was
used on real work and one thing was missing in a way the members could not cover between
them.

## What was observed

The department was driven through five tool calls on a live request. The planner produced
a list of open questions and stopped there. Nothing in the department:

- asked those questions one at a time,
- judged whether an answer had actually closed anything,
- or said when enough had been answered to start.

So the length of the intake was decided entirely by the caller. Four questions were asked.
Whether the right number was four or sixteen, the department had no way to say — it had no
measure to offer and no opinion to give.

A second thing followed from the first: the answers that were collected lived nowhere. Each
tool call was independent, so every answer had to be retyped into the next call by hand, and
there was no frozen brief for the later phases to work from.

## What this member does about it

It runs the interview as a loop — one question, one answer, one score — and it freezes the
result into a brief that the later phases take as an argument.

Two things it deliberately does NOT do, because claiming either would be worse than the gap:

- **It does not store anything.** There is no database behind these tools. The brief travels
  in and out as text and the caller owns it. Every template says so, because a caller who
  believes the last four answers were remembered will stop sending them.
- **It does not reach the user.** It hands a question to whoever is calling it. An answer
  invented on the user's behalf is indistinguishable from a real one once it is in the brief,
  and everything built afterwards rests on it.
