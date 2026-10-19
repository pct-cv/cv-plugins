# asana-pass plugin (pilot: Noah)

The daily update pass Paul runs, packaged so anyone at CV can run their own.
It reads your sources since the last pass and proposes a numbered queue of
Asana changes, reply drafts, email filing and project-site updates. Nothing is
written until you say "go". Nothing is ever sent: emails and Slack messages
are drafts.

## What you need

| Need | Why | How |
| --- | --- | --- |
| A private repo of your own (e.g. `<you>-workspace`) | Holds your config, rules and pass state | Create it on GitHub, start Claude Code sessions on it |
| Asana access for scripted calls | The pass writes to Asana through the REST API | Cloud sessions: add an app.asana.com network secret to your environment. Local: a token in a gitignored `.secrets/` file |
| Connectors in claude.ai | Reading your sources | Asana, Gmail, Slack, Granola (optional), CV-MCP for the Den (optional) |

## Install and set up

1. Register the marketplace in Claude Code, then install from it:

   ```
   /plugin marketplace add pct-cv/cv-plugins
   /plugin install asana-pass@cv-plugins
   ```
2. In a session on your repo, run `/asana-pass setup`. Claude asks for your
   Asana user, projects, email labels, Granola folders and your own working
   rules, then writes `.asana-pass/` in your repo and commits it.
3. Run `/asana-pass` each day (or "run the update pass").

## Files it creates in your repo

```
.asana-pass/
  config.md       your IDs: Asana user, projects and sections, labels, folders, people notes
  rules.md        your working rules (starts from the team rules; add your own)
  sites.md        project sites you own or follow, and what feeds them
  state/
    cursor.md     when the last source check ran
    open-items.md watch items, waiting-on, deferred work
    log/          one file per day: queue, outcomes, IDs created
```

## Pilot notes

- Paul's own pass keeps running from his workspace; this plugin is the shared
  version. Report anything that reads as Paul-specific so it moves to config.
- Feedback: comment in the pilot thread or tell Paul.
