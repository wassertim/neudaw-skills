# neu.daw skills

Skills that teach an AI agent to make music in [neu.daw](https://neuseq.com), a digital audio workstation that runs in the browser and as a desktop app.

A skill is a folder with a `SKILL.md`: a short header (`name`, `description`) and step-by-step guidance for one longer piece of work. The agent does the work through neu.daw's tools, so it needs a neu.daw project open.

| Skill                              | What it does                                                                         |
| ---------------------------------- | ------------------------------------------------------------------------------------ |
| [fugue](fugue/SKILL.md)            | Writes a fugue from subject to finished render, stopping for approval at each checkpoint. |
| [piano-arrange](piano-arrange/SKILL.md) | Arranges tracks so one pianist can play them, one track per hand.               |

## Use them

neu.daw already ships these skills. An agent connected to neu.daw reads one with the `skill` tool and needs nothing installed.

- **Desktop app (macOS, Linux):** turn on "Allow MCP connections" in Settings → Agents and add your agent as a client there. The agent then has the `skill` tool.
- **Browser** ([daw.neuseq.com](https://daw.neuseq.com)): turn on "Allow browser agents" in Settings → Agents. An agent running in the page, such as Claude in Chrome, calls `neudaw.help()` to list the skills and `neudaw.skill('fugue')` to read one.

To keep a skill in your agent's own skill folder instead, copy its folder there, for example into `~/.claude/skills/` for Claude Code.

## Contribute

New skills and improvements are welcome as pull requests.

- One folder per skill, named like the skill, holding a `SKILL.md`.
- The header has `name` (the folder name) and `description` (one sentence: what the skill does and when to use it).
- Keep a skill under 600 words. Name only tools neu.daw offers, and no source files.
- Say what the agent should do and where it should stop for the user. Leave out what neu.daw's tools already explain.

neu.daw pins this repository at a tag, so a merged change reaches the app with its next release.

## License

[MIT](LICENSE)
