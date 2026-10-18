# CV working rules for the update pass

Shared rules for everyone's pass. Your own `.asana-pass/rules.md` adds to
these and wins where they conflict. Dates show when a rule was set.

## Flow

- **Propose first.** Show every change before making it: the exact wording
  of comments, tasks, messages and site edits. Write only after the user
  says "go", and apply their changes to the proposal exactly.
- **Never send.** Email replies are Gmail drafts (replies to the message);
  Slack messages are drafts. The user sends them.
- **Processing order (10/16).** Inbox and follow-up email first, then Asana
  status updates, then the actual work, unless the user says otherwise.
- **One step at a time.** For each item show the Asana change, the related
  emails (with label), Slack/Den context and a reply draft where needed.
  The Asana update, reply draft and filing happen together in one step.
- **Re-check right before each step.** People act in parallel. Re-read the
  source and the Asana task (with subtasks' completed state) before
  proposing and again before writing.
- **When the user says a reply is sent (10/17):** re-read the sent version,
  update the Asana comment or task to match what went out, and file the
  thread, in the same step.
- **Calendar invites (10/14):** propose title, time, guests and notes in
  chat first; create only after "go".

## Task granularity

- A task is one specific deliverable. No catch-all buckets. Search Asana for
  an existing task before proposing a new one, and don't stretch a match.
- **Subtask rule (10/13).** Steps go in the description as a checklist. A
  step becomes a subtask only when a different person does it, or it waits
  on an outside input or has its own date.
- Every subtask for someone other than the task owner has one assignee and a
  due date. When the owner also does a subtask, leave it unassigned and
  append " - task owner" to its name.
- **Approvals are never subtasks.** Each approval is one Den task, with the
  Den ID in the Asana task's Den Task Reference field. A task works toward
  one Den approval; follow-up work comes from the approval.
- **Terms in source documents** (contract terms, notice periods) go in the
  task's analysis checklist, not standalone tasks. Summarize the source
  facts in the description with a link; a link alone isn't enough.
- **One topic per task.** Order subtasks by the work. Descriptions and
  comments: thorough but concise.
- Existing tasks that break these rules are fixed in a grooming session, not
  on sight.

## Assignment and projects

- Every task assigned to someone else lives in a project, not only in their
  My Tasks.
- **Don't assign work or due dates to others without discussing it first.**
  Propose owners and dates as questions.
- People not in Asana (outside parties, tenants) can't be assignees: park
  the item under the closest internal owner and say so in the task.
- Check `.asana-pass/config.md` people notes before assigning.

## Updates and closing

- **Moving a due date:** always comment why.
- **Closing a task:** never just mark it complete; comment how it was
  resolved.
- **One channel:** when work is tracked in Asana, steer updates there rather
  than splitting them across email, Slack and Voxer. Asana is for internal
  alignment; approvals stay in the Den.

## Dan and the Den

- Dan is never in Asana: don't invite him, assign to him or point him there.
  His approvals go through the Den.
- Den tasks are for approvals only; tracking lives in Asana.
- Den `review_state: "approved"` only means Dan reviewed it. Read
  `dan_feedback` and the checked boxes for the real decision.
- Den Task Reference format: `https://www.cv-den.com/tasks/<id>`.

## Confidentiality

- Never save personnel/HR (offers, comp, candidates), tax or investment
  matters to Asana, a site or your pass state unless your rules say a
  specific item is in scope.
- Some people in Asana are outside CV (property management, consultants).
  Check a project's members before writing anything sensitive in it.

## Leasing and Chill House

- Before proposing any leasing offer or prospect, check the tenant
  restrictions register task (Leasing project).
- Broker rule: separate retail and office brokers; agreements only if
  non-exclusive and cancellable at any time.
- **No pricing in writing (10/17)** for office leasing or the Chill House
  until pricing is set: answer with space, timing and next steps, and offer
  a call. Point Chill House prospects to thechillhouse.org and the Launch
  Crew offer (https://thechillhouse.org/launch-crew).
- **Chill House is space only (10/16):** we don't provide or quote food,
  beverage or event services.
- **Booking and revenue inbound goes to David (10/16)**, with Noah as a
  follower.

## Email

- **Filing:** add the label and remove INBOX. Leave read/unread alone.
  Label routing is in your config.md.
- Leave for the user: calendar RSVPs, system notices (Asana, DocuSign, OOO,
  Drive shares, Claude share notices) and personal items.
- **Email links in Asana:** Gmail links open only in your own mailbox, so
  always pair one with a summary (sender, date, the facts that matter). Link
  format: `https://mail.google.com/mail/?authuser=<your email>#all/thread-f:<decimal of the hex thread id>`
  (`python3 -c "print(int('<hex>',16))"`).
- **Email summaries go in a comment (10/17).** When a task comes from an
  email exchange, the task body holds who, history, position and next steps;
  the summary of the user's reply goes in the first comment with the link.

## Asana API

- Scripted, bulk and write calls go through the REST API with curl
  (`https://app.asana.com/api/1.0/...`). In cloud sessions the network
  secret adds the Authorization header: never send one, never ask for or
  write down a token. The Asana connector is for quick read-only lookups.
- Workspace: carusoventures.com, 1202997802949315. Never call another one.
- Attach files via `POST /attachments` (multipart, `parent=<task gid>`);
  check existing attachments first and confirm after.
- Can't add a project to an existing task, move tasks between sections, edit
  project descriptions or delete comments. To move a task into a project,
  recreate it there and delete the original after checking its stories.
- Comment `html_text` allows only body, strong, em, u, s, code, ol, ul, li,
  a, blockquote, pre. No `br`. Plain apostrophes; escape only `&`, `<`, `>`.
- Creates can time out but still succeed: re-read before retrying.

## Reliability

- Worker restarts and disconnects are frequent: batch independent calls in
  parallel, log progress to today's state log as you go, and check for
  duplicates before retrying any create.
