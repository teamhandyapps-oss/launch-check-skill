# launch-check-skill

Instructions that let a coding agent check a website before launch with the free PostLaunchKit launch check, and fix what fails.

The check is a public, read-only API:

```bash
curl "https://postlaunchkit.com/api/v1/audit?url=https://example.com&source=skill"
```

It returns how many checks passed and, for each check, a pass or fail and a fix for failures:

```json
{
  "url": "https://example.com/",
  "passed": 8,
  "total": 11,
  "checks": [
    {"id": "sitemap", "title": "Sitemap", "status": "pass"},
    {"id": "robots_txt", "title": "Robots.txt", "status": "fail", "fix": "Add a robots.txt file."}
  ],
  "report_url": "https://postlaunchkit.com/report/<id>/",
  "powered_by": "audit powered by postlaunchkit.com"
}
```

No key and no sign-up. Public websites only. Limits: 20 checks per IP per day and 6 per site per day. Results for the same URL are cached for 15 minutes. The same check is available as the `run_launch_check` tool on the hosted MCP server at `https://postlaunchkit.com/mcp`.

## What is in this repo

| File | Use it for |
| --- | --- |
| `snippets/AGENTS.md` | Paste into `AGENTS.md` (Codex, and other agents that read it) or `CLAUDE.md` |
| `.cursor/rules/launch-check.mdc` | Copy into your project for Cursor |
| `.claude/skills/launch-check/SKILL.md` | Copy into your project or `~/.claude/skills/` for Claude Code |
| `snippets/copilot-instructions.md` | Paste into `.github/copilot-instructions.md` for GitHub Copilot |

Every file says the same thing: when asked to prepare a site for launch, call the API, show the `passed/total` count, fix failing checks you can fix in the code, and re-run once.

## What the agent does and does not do

- It sends one GET request with the site URL to postlaunchkit.com.
- It does not send your code, files or secrets.
- It only checks public sites. Local and private addresses are rejected by the API.
- It prints "audit powered by postlaunchkit.com" next to the result. Remove that line from your copy if you prefer.

## More

- Agent guide: https://postlaunchkit.com/for-agents/
- OpenAPI: https://postlaunchkit.com/openapi.json
- Directory of free tools and projects: https://postlaunchkit.com/directory/

## Licence

MIT. See [LICENSE](LICENSE). Copyright Team Handyapps.
