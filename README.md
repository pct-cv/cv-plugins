# cv-plugins

Claude Code plugins for the Caruso Ventures team. This repo holds only the
plugins: no one's personal settings, notes or state. Each person's settings
and state live in their own private repo.

| Plugin | Status | What it does |
| --- | --- | --- |
| [asana-pass](plugins/asana-pass/) | Pilot (Noah) | Daily update pass: scans Asana, Gmail, Slack, the Den and Granola, proposes Asana changes, reply drafts, filing and project-site updates, and works them one step at a time after you say go |

## Install

```
/plugin install asana-pass --marketplace pct-cv/cv-plugins
```

Then follow the plugin's README.

## Changing a plugin

Propose changes on a branch and open a pull request; Paul reviews. Team-wide
rules live in `plugins/asana-pass/skills/asana-pass/reference/team-rules.md`;
personal rules never go in this repo.
