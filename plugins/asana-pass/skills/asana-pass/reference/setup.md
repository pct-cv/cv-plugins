# Setup (`/asana-pass setup`)

Configures a new user. Run in a session on the user's own private repo.
Ask in small groups, confirm each answer, and write nothing until the user
has seen the files.

1. **Check the repo.** Confirm it is the user's own private repo, not
   someone else's workspace. If `.asana-pass/` already exists, offer to
   review it instead.

2. **Check connections** (read-only calls):
   - Asana REST: `curl -s https://app.asana.com/api/1.0/users/me` must
     return the user, in workspace 1202997802949315. If it fails in a cloud
     session, the environment needs an app.asana.com network secret; read
     the environment documentation and point the user to it. Never ask for a
     token in chat.
   - Connectors: Asana, Gmail, Slack, Granola, CV-MCP (Den). List which
     answer; the missing optional ones are skipped by the pass.

3. **Ask, then fill `templates/config.md`:**
   - their email, Asana user gid (from `users/me`), Slack user id;
   - the state branch (default `main`);
   - their Asana projects and sections (list the projects they're a member
     of; they pick);
   - Gmail labels and routing (list labels; they say which topics go where);
   - Granola folders to process and folders to exclude;
   - people notes they want applied (e.g. who is outside CV, who is never a
     sole assignee). Offer the team people list from Asana's workspace users.

4. **Rules.** Show `team-rules.md` in brief. Ask for personal rules: what to
   exclude, who must never see what, approval routes, processing order
   changes. Write them into `templates/rules.md` → `.asana-pass/rules.md`.

5. **Sites.** Ask about project sites they own or follow; fill `sites.md`
   (URL, Asana project or tasks, collections, signals). Empty is fine.

6. **State.** Create `state/cursor.md` with a starting point the user picks
   (default: 3 days ago), an empty `open-items.md` and `log/`.

7. **Show and commit.** Show every file. After "go", commit on the state
   branch and push. Then offer a first pass.
