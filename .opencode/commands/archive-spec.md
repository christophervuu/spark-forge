Archive a completed spec.

Spec: [FS-###]

Before archiving, verify all of the following:

1. Every task file in `forge/active/FS-###/tasks/` has `Status: done`
2. Every task's `Acceptance Checks` are satisfied

If any task is not done:
- Do not archive
- List which tasks are not done and their current status
- Stop here

If all tasks are done:

1. Update the spec `Status` field in `forge/active/FS-###/spec.md` to `completed`
2. Move the entire `forge/active/FS-###/` folder to `forge/completed/FS-###/`
3. Confirm the move: `forge/completed/FS-###/` exists and `forge/active/FS-###/` no longer exists
4. Open `forge/completed/FS-###/spec.md` and extract:
   - the spec title
   - the completed date (today's date)
   - a 1–3 sentence summary of what the spec delivered
   - key decisions, patterns introduced, constraints observed, or other context a future agent would find useful
5. Add a new row to the table in `forge/architecture/COMPLETED_SPECS.md` with the extracted fields. Remove the placeholder row if it is still present.
6. Update the `Last Updated` date for the `COMPLETED_SPECS.md` row in `forge/architecture/INDEX.md` to today's date.
7. Report: "FS-### archived. N tasks completed. COMPLETED_SPECS.md updated."