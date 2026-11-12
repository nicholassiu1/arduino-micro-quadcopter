---
name: end-session
description: End-of-session routine for the quadcopter project. Update PLAN.md, sync GitHub issues, commit and push, and leave a note for next time. Use when the user says they're done for the day, asks to wrap up, or asks to commit and push "everything I should do at the end of a session".
---

# End-of-session routine

Run these steps in order. Explain each git step briefly as you go, since the user is learning git. Teach, don't just do (see "Teach me while we build" in CLAUDE.md).

## 1. See what changed
- Run `git status --short` and `git log origin/main..main --oneline`.
- Look for new files in the folder that should **not** be committed: large videos, build output, private info, SolidWorks lock files (`~$*`). Add a pattern to `.gitignore` if a new kind of file appears.
- Make sure `CLAUDE.local.md` is still ignored, and that no secrets, passwords or personal details are in the files being committed.

## 2. Update the tracking docs
- **PLAN.md, Done:** add a line for each thing finished this session, with the date.
- **PLAN.md, Decisions:** add any decision made this session, with its reason.
- **PLAN.md, Problems:** add any problem hit, with its cause and fix.
- **PLAN.md, Lessons:** add one line for each new concept learned (ME, electronics, git/GitHub, Claude Code).
- **PLAN.md, Next session:** replace this section with the 1–3 most important next steps, so the next session starts fast.
- **parts.md:** update the status column (☐ → 🛒 → ✅ → ✔) and any measured weights.
- **CLAUDE.md:** update it only if a lasting fact or preference changed. Keep it concise.

## 3. Sync GitHub issues
- List open issues: `gh issue list -R nicholassiu1/arduino-micro-quadcopter`.
- For each issue finished this session, **ask the user to confirm** before closing it. Close it through the commit message (`Closes #N`) when possible; otherwise use `gh issue close N -c "<what was done>"`.
- Tick off checklist items in issues that were partly done.
- If new tasks came up, create issues for them with the right milestone and label.

## 4. Commit and push
- Group the changes into small, focused commits. One logical change per commit is better than one big "end of session" commit.
- Message format: a short imperative summary line, a blank line, then *why* if it isn't obvious, then `Closes #N` lines, then the attribution lines from the system instructions.
- Run `git push`, then `git status -sb` to confirm `main` is in sync with `origin/main`.

## 5. Report back
Give the user a short summary:
- what was committed (commit hashes) and pushed
- which issues were closed or updated
- what's next (from PLAN.md "Next session"), with the nearest TIMELINE.md deadline and how many days remain
- any manual steps still waiting on them
