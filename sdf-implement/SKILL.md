---
name: sdf-implement
description: >-
  Implement an agreed design slice by slice, commit per slice, review the result in a fresh context against
  the requirements and the design, then open a pull request. Use after a design document is agreed, or when
  the user says "let's implement this" or asks to resume an implementation.
---

# Implement

Build what the design says, in the order it says, and prove it matches before asking the user to review.

The user reviews and merges the pull request. Do not merge.

## Flow

1. **Read the input.** Read `requirements.md` and `design.md` (see Output of the other sdf skills for where). If the design is not `agreed`, stop and ask the user.
2. **Find where you are.** Read the Slices in the design and the git log of the branch. A slice is done when its commit exists. Start from the first slice without one. This is also how you resume after a compact or in a new session.
3. **Find the project's rules.** Look in the project's `CLAUDE.md`, its rules directory (such as `.claude/rules/`), and the list of available skills for: branch and commit conventions, how to run format, lint, typecheck and tests, and how the project reviews changes. Pick only what is relevant and read it. If a project rule conflicts with this skill, the project wins. If the current branch belongs to unrelated work, cut a new one as the project says.
4. **Run each slice: build, verify, commit.** One slice is one commit. Put the slice's number or name in the commit message so the log shows progress. Run the project's checks before each commit.
5. **When the design does not hold, follow the table below.**
6. **Review in a fresh context** (below), fix what it finds, and review again until it is clean.
7. **Open the pull request.** The description links the requirements and the design, lists every deviation from the design, and states the result for each acceptance example. Update the design's status to `implemented` once the user merges, or in the same pull request if the project's flow allows.

## Keeping the context small

Implementation is long. Make it safe to drop the context at any point.

- Progress lives in git (one commit per slice) and in the design, not in your memory.
- After each slice commit, if the context is heavy, tell the user this is a cheap moment to run `/compact` or start a new session. You cannot run it yourself. Resuming is step 2.

## When the design does not hold

Judge by what the reviewer agreed: boundaries, data shape, decisions, and acceptance examples.

| Deviation | Example | What to do |
| --- | --- | --- |
| Small | None of the agreed things change: an internal function split, a detail value | Go on. Mention it in the pull request |
| The design changes | A boundary, the data shape, or a decision must change | **Stop.** Do not push through. Show what turned out different, with evidence, give options with a recommended default, and ask. After agreement, fix the design and go on |
| The requirements do not hold | An acceptance example cannot be satisfied, or a premise was wrong | Go back to the requirements. Fix them with the user, then the design |

- Keep finished slices. Redo from the slice that broke.
- Record every change to the design at its end under "Changes during implementation": date, what differed, what changed.
- Never drift silently. When unsure between small and design-changing, treat it as design-changing.

## Review

Run it in a subagent with a fresh context. A reviewer who did not write the code sees what its author does not. This is required, and it also keeps the diff out of your own context.

Give it the requirements, the design, and the diff against the base branch. Ask it to check:

- **Against the design:** the diff stays within the design's diff from the as-is, with no change outside it; boundaries have the designed shape.
- **Against the requirements:** every acceptance example, including what must **not** happen, has its test and passes. Run the project's checks to confirm.
- **General:** leftover debug code, TODO or FIXME, types, unused imports, tests that do not match the behavior. If the project has its own review rules or skill, use it for this part.

The reviewer reports each finding as must-fix, needs-clarification, or OK. Fix must-fix items, ask the user about needs-clarification items, then run the review again.

## Not here

Why the feature exists (requirements) and how it is shaped (design). If you find yourself deciding those, you are in the "design changes" row above.

## Principles

- The design is the contract. Differences are reported, not hidden.
- One slice, one commit, one verification.
- A pull request the reviewer can trust says what it did, where it differed, and how each acceptance example was checked.
