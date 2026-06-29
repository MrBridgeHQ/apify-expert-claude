# Installation — Apify Expert skill

This skill is designed to be installed at the **user level** in Claude Code, so it's available across all your projects without copying it into each repo.

It is pure reference content (Markdown) — there are **no runtime dependencies**, no scripts to run, and no Python or Node packages to install.

## Prerequisites

- Claude Code installed and working
- A terminal with `unzip` (macOS, Linux) or PowerShell with `Expand-Archive` (Windows)

## Installation

### macOS / Linux

```bash
# 1. Create the user-level skills directory if it doesn't exist
mkdir -p ~/.claude/skills

# 2. Unzip the skill into that directory
unzip apify-expert.zip -d ~/.claude/skills/

# 3. Verify the structure
ls ~/.claude/skills/apify-expert/
# Expected: SKILL.md  README.md  INSTALL.md  references/
```

### Windows (PowerShell)

```powershell
# 1. Create the user-level skills directory if it doesn't exist
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills"

# 2. Unzip the skill into that directory
Expand-Archive -Path .\apify-expert.zip -DestinationPath "$env:USERPROFILE\.claude\skills\"

# 3. Verify the structure
Get-ChildItem "$env:USERPROFILE\.claude\skills\apify-expert\"
# Expected: SKILL.md  README.md  INSTALL.md  references\
```

## Verification

Open Claude Code and ask:

> "What skills do you have access to?"

You should see `apify-expert` in the list. If not, check that the SKILL.md file is at the path:

- macOS / Linux: `~/.claude/skills/apify-expert/SKILL.md`
- Windows: `%USERPROFILE%\.claude\skills\apify-expert\SKILL.md`

## First test

In any Claude Code session, ask:

> "What's the memory-to-CPU coupling on Apify? How many vCPU cores does a 4096 MB Actor get?"

The skill should activate and answer from `references/platform-overview.md` (4096 MB = 1 full vCPU core).

Or try a "what's new" question:

> "What's new on Apify lately?"

The skill should consult `references/changelog-digest.md` and summarize the newest entries.

## Keeping the changelog digest current

`references/changelog-digest.md` is a manually-maintained, newest-first log of recent Apify platform changes. To refresh it:

1. Open <https://apify.com/change-log>.
2. Identify entries newer than the digest's current newest entry.
3. For each new entry, prepend a formatted, impact-tagged block (format in the digest header) directly under the `INSERT-NEW-ENTRIES-BELOW` marker, newest-first.
4. Flag high-impact entries (pricing/billing changes, API breaks, Standby/MCP behavior changes, schema/template changes) under "⚠ Needs human review".

If the digest's newest entry is older than ~2 weeks, it's a good time to refresh.

## Updating the skill

To install a newer version, just replace the directory:

```bash
# macOS / Linux
rm -rf ~/.claude/skills/apify-expert
unzip apify-expert.zip -d ~/.claude/skills/
```

Any manual edits you made to `references/changelog-digest.md` will be lost. If you want to preserve them, back up that file before updating.

## Uninstalling

```bash
# macOS / Linux
rm -rf ~/.claude/skills/apify-expert

# Windows (PowerShell)
Remove-Item -Recurse -Force "$env:USERPROFILE\.claude\skills\apify-expert"
```

## Troubleshooting

**Skill doesn't activate when expected.**
The skill's auto-activation depends on the description in `SKILL.md`'s frontmatter. Triggers include questions about Apify Actors, runs, storage, proxy, the CLI/SDK, the REST API, Standby mode, Store mechanics, and "what's new on Apify". If your phrasing doesn't match, force activation: "Use the `apify-expert` skill to..."

**A platform figure looks out of date.**
Figures flagged *(verify)* in the reference files should be confirmed against <https://docs.apify.com> before being quoted as hard limits — they drift. If you find a stale number, edit the relevant `references/*` file directly.
