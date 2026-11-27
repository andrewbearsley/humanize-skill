# humanize

A writing-voice skill. It strips AI tells (em-dashes, corporate filler, symmetric structure, hedging) and shortens text. Use it as the last pass before you send, save or paste any written text.

The skill text is `SKILL.md`. It is a general skill for work and personal writing.

## Install

Clone the repo anywhere, then link it into the agent runtimes:

```sh
git clone https://github.com/andrewbearsley/humanize-skill.git ~/Dev/humanize-skill
~/Dev/humanize-skill/bin/install
```

The script links the repo as `humanize` into `~/.agents/skills` (Codex, Copilot, Gemini) and `~/.claude/skills`. Where the Claude app has synced the claude.ai copy, Claude Code loads that copy, so the script skips the Claude Code link.

## claude.ai

```sh
bin/package
```

The script writes `dist/humanize.zip` with a commit stamp in the frontmatter. Upload the zip in claude.ai Settings > Skills. The script also prints the stamp on the synced copy, so you can see when claude.ai is behind.

## Other agent tools

Other agent tools can load `SKILL.md` as it is. It names no tool-specific paths, so the rules work in any tool that reads a skill or a prompt file.

## Changes

The `reflect` skill proposes new rules here. Add each rule as the next number, with the date and the case that caused it. Describe the case in general terms, such as "a customer POV deck". Leave out customer and colleague names, because this repo is public.
