# Reviews and grooming

Goal: bring a project's tasks into line with the rules in `team-rules.md` and your `rules.md`
(granularity, subtask rule, approvals in the Den, every task in a project).

## Method

1. **Report what the record shows, per open task:** source, named owner
   (assignee, or the owner named in notes), due date, last activity, open
   subtasks and their owners.
2. **List, as facts:**
   - exact overlaps (the same work in two places);
   - items with no owner anywhere in the record;
   - overdue tasks;
   - tasks that break the subtask or approvals rule;
   - assignees who aren't project members.
3. **Don't infer** owners from meeting notes, and don't decide merges.
4. **Put the decisions to the user** as a numbered list.
5. **Apply their answers exactly**, one change set at a time. Moving a task into
   a project means recreating it there (see API limits in `team-rules.md` and your `rules.md`).

Save the audit to `.asana-pass/state/` (e.g. `underground-audit-<date>.md`) so a
later session can apply the user's answers.
