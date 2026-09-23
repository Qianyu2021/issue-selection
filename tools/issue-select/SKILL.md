# issue-select skill

This is the installed skill used by the eval harness and by live runs. Keep this copy
in sync with the installed directory `~/.claude/skills/issue-select/`.

Structure
- `rubric.md` — the checks and decision rules (this file)
- `scope.md` — repo scope and other metadata
- `references/evidence-guide.md` — guidance for finding evidence in issues

Usage
1. Install by copying this `skill/` folder to `~/.claude/skills/issue-select/`.
2. Edit the installed `scope.md` to set the `Repo:` line to `codepath/pathreview-ai301-fa26-s1`.
3. Run the harness with `--rubric` pointing at this `rubric.md` file.

Live mode
To grade candidate issues in live mode from any directory:

```
claude "issue-select: grade these candidate first issues: <URL> <URL> <URL>"
```

Output
The skill prints accepted issues first with short fit reasons, then rejected ones with
the check that sank them. It ends with a fenced JSON block of per-issue `accept`/`reject`.
