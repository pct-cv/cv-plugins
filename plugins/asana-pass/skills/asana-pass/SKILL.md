---
name: asana-pass
description: Daily Asana update pass. Scans Asana activity, Gmail, Slack, the Den and Granola since the last pass, proposes Asana changes, reply drafts, email filing and project-site updates as a numbered queue, then works it one step at a time after the user says "go". Use when the user asks for an update pass, to check Asana/email/Slack/Den for new work, to groom an Asana project, or invokes /asana-pass. "/asana-pass setup" configures a new user.
---

# Asana update pass

Keeps a person's Asana current with their in-flight work. Asana is the team's
workspace; the Den is Dan's review space; project sites hold decisions and
numbers.

## Files

Plugin (shared, read-only):
- `reference/team-rules.md`: CV working rules for everyone. Read in full
  before the first proposal of every pass.
- `reference/grooming.md`: method for project reviews.
- `reference/setup.md`: the setup interview (`/asana-pass setup`).
- `templates/`: starting files for a new user.

The user's repo (personal, written by the pass):
- `.asana-pass/config.md`: their IDs (Asana user, workspace, projects and
  sections, Gmail labels, Granola folders, people notes) and connections.
- `.asana-pass/rules.md`: their own rules. Where it conflicts with
  team-rules.md, the user's rules win for their pass.
- `.asana-pass/sites.md`: project sites they own or follow.
- `.asana-pass/state/`: `cursor.md`, `open-items.md`, `log/YYYY-MM-DD.md`.

If `.asana-pass/config.md` is missing, say so and offer `/asana-pass setup`.
Never read or write another person's `.asana-pass/`.

## Steps

1. **Load state.** `git fetch origin <state branch>` (config.md names it;
   default `main`) and read `.asana-pass/` from it; it can be newer than the
   session branch. Read team-rules.md, the user's rules.md and config.md. If
   today's log has an unfinished queue, offer to resume it before scanning.

2. **Scan sources since the cursor**, in parallel. Skip any source config.md
   marks as not connected, and say which were skipped.
   - **Asana activity:** workspace task search with
     `modified_at.after=<cursor>` limited to the user's projects and tasks
     (sort_by=modified_at, limit 100; page if full). For each task pull
     stories since the cursor and its current state (completed, assignee,
     due, parent and its state, subtasks with `completed`). Then:
     - list what others did: comments, @mentions of the user, completions,
       new tasks or subtasks, reassignments, due-date changes;
     - before proposing, check for later activity: if the user already
       answered or closed it, list it under "already handled", not in the
       queue;
     - reminder stories point at an older comment: check that comment;
     - summarize the user's own edits since the cursor, one line per task.
   - **Gmail:** `in:inbox after:<cursor date - 1 day>` plus the same with
     `-in:inbox` for sent and archived mail. `get_thread` (plain text) every
     thread with activity after the cursor; search previews show only the
     oldest messages.
   - **Slack:** search with `after:<cursor date>`, sorted by time; read the
     threads that matter.
   - **Den** (if connected): `triage`, `get` anything updated since the
     cursor, and list items awaiting the user's review.
   - **Granola** (if connected): meetings since the cursor (at least 7 days).
     Skip folders config.md marks as excluded: never open or log them. Match
     on content, not titles. Re-read a note right before proposing from it.
   - **Watch items** in open-items.md against what came in.
   - **Project sites** (`.asana-pass/sites.md`): read each site's
     collections (`ArtifactData` list) and match the items above against the
     signals listed for that site. A match becomes a queue item: the exact
     field change on the exact doc (current value, new value, source), next
     to the Asana change it rides with. List edits the site's owner made
     since the cursor. Never copy Asana dates or owners into a site.

3. **Match to Asana.** Search for an existing task (keyword search, then the
   task with subtasks, custom fields and memberships). Don't stretch a
   match; a new deliverable gets a new atomic task.

4. **Propose the queue.** A numbered list of every actionable item, one line
   each, plus a short "no action" list with why, and anything waiting on
   others. Then present **step 1 only**, in full:
   - Email: sender, time, what it says, link (team-rules.md format), label.
   - Asana change: exact task (name + gid), comment text, renames, subtasks
     (assignee, due), dates, project/section.
   - Site change: site, collection/doc, field, current → new value.
   - Context: related Slack and Den items.
   - Reply draft, when a reply is needed.
   - Attachments worth keeping.
   - Questions for the user, numbered.
   Log the queue to today's log before waiting.

5. **On "go".** Re-read the source and the Asana task (the user may have
   acted meanwhile). Then in one step: the Asana update, the Gmail reply
   draft (`create_draft` with `replyToMessageId`), the site change and the
   filing. Re-read after any create that times out before retrying. Report
   in a line or two, then present the next step. Apply the user's changes to
   a proposal exactly.

6. **Save state** after each completed step and at the end:
   ```bash
   git fetch origin <state branch>
   git stash -u            # only if the session branch has uncommitted work
   git checkout -B pass-state origin/<state branch>
   # edit .asana-pass/state/cursor.md, open-items.md, log/<today>.md
   git add .asana-pass/state && git commit -m "pass: <date>, <what changed>"
   git push origin pass-state:<state branch>
   git checkout -          # back; git stash pop if stashed
   ```
   If the push is rejected, `git pull --rebase` and push again. Commit only
   `.asana-pass/state/` this way; rule and config changes go through the
   session branch.
   - `cursor.md`: the time the source check ran, not when the pass ended.
   - `open-items.md`: add new watch items, remove resolved ones.
   - `log/<today>.md`: queue, each step's outcome, gids created.

## When the user changes a rule

Propose the exact edit to `.asana-pass/rules.md`; after "go", commit it. A
rule that should apply to the whole team goes to Paul as a suggestion for
team-rules.md, not into the plugin directly.

## Grooming

For "groom <project>": follow `reference/grooming.md`. Report facts, put
decisions to the user as a numbered list, apply their answers exactly.
