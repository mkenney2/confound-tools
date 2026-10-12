# confound-tools

A Claude Code plugin for stress-testing experiment results before you believe, write up or share them.

- **`confound-check` skill**: pins down each claim (metric, baseline, data, scripts), sends it to an independent
  reviewer, runs the cheapest checks that settle the serious suspicions, and reports a verdict per claim
  (survives / weakened / artifact / unresolved) with a corrected headline.
- **`confound-reviewer` agent**: a read-only, skeptical methods reviewer with a fresh context. It reads the code and
  result files behind a claim and hunts for confounds, leaks and unfair comparisons, each with a concrete check.

## Install

In Claude Code:

```
/plugin marketplace add mkenney2/confound-tools
/plugin install confound-tools@confound-tools
```

Then start a new session. To get updates later: `/plugin marketplace update confound-tools`.

## Use

Ask something like "is this result legit?", or run it explicitly:

```
/confound-tools:confound-check <the claim, or a path to a README / write-up>
```

With no argument it checks the most recent headline result in the conversation. The skill asks before running
anything that costs money (paid API judging, GPU jobs).

## Without the plugin system

Copy `skills/confound-check/` into `~/.claude/skills/` and `agents/confound-reviewer.md` into `~/.claude/agents/`.
