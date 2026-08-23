---
description: Generate this project's raw requirements and the three sealed artifacts (Stage 1)
---

Follow the instructions in `prompts/01-generate-requirements.md` exactly.

Boundary reminders:
- You select the domain and the **level** per the Selection Rule in that prompt, reading `.project-context.md`. The user never chooses. Apply the ramp mechanically, and if the level repeats, raise a difficulty dimension and say which.
- **Do not pre-classify anything.** Outputs 1 and 2 are written in the client's world: no section titled "Constraints", no section titled "Success Criteria", no functional/non-functional split. Sorting the raw input into those categories is the architect's Stage 2 work, and delivering it pre-sorted deletes the exercise.
- Never name a specific AWS service in Outputs 1 or 2.
- **Three of the four outputs are SEALED.** After writing them, never quote, reference, hint at, or be influenced by their contents until the stage that opens each:

  | File | Opens at |
  |------|----------|
  | `requirements/.change-request.md` | Stage 4c |
  | `requirements/.acceptance-tests.md` | Stage 5 |
  | `requirements/.reference-solution.md` | Stage 6 |

  This includes soft leaks: steering the user toward the reference design, warning them about something only the acceptance tests would reveal, or hedging in a way that signals they are off track.

After writing the outputs, **tell the user which level you selected and what that means for the migration artifacts**:
- **L3 or higher** — this project is brownfield. `analysis/current-state.md` and `analysis/disposition-7rs.md` apply; they are filled in Stages 2 and 3.
- **L1–L2** — greenfield. Tell the user to delete both files so they don't sit unfilled in the repo.
