# review-requested-prs

A [Claude Code](https://claude.com/claude-code) skill that walks through the open pull requests
**awaiting your requested review** — across one or several repos — and drafts a GitHub review for
each as **pending comments you finish and submit yourself**. It never submits anything on your
behalf.

## What it does

1. **Discovers** the PRs where your review is requested and still pending
   (`gh search prs --review-requested=@me`), excluding drafts and PRs you've already reviewed
   without being re-requested.
2. Lets you **pick the repos**, then the **PRs**, with everything selected by default.
3. For each chosen PR, reviews it **as if a junior wrote it** — correctness, security, simplicity
   ("be clear, not clever"), conventions, tests — reading prior feedback first so it doesn't repeat
   what's already been raised.
4. Leaves the feedback as a **pending (draft) review**: inline comments + a summary, never
   submitted. You review, edit, and submit on the GitHub UI.

Comments are **actionable only** (request-changes style) — no filler praise.

## Why

A full review dumped as one big block of text is hard to digest — especially when you have 10–15 PRs
to get through. This skill puts the feedback **on the PR itself, anchored to specific lines**, and
leaves it **pending** so you edit, approve, or discard each comment on your own judgment before
anything is submitted. Contextualized, line-specific, and faster to skim.

## Dry-run mode

Run it without touching GitHub: it does all the read-only work and prints, in the chat, exactly
what it *would* have posted.

```
/review-requested-prs dry-run
```

## Usage

```
/review-requested-prs            # live: creates pending reviews
/review-requested-prs dry-run    # dry-run: prints what it would post, writes nothing
```

Installed as a plugin, the skill is namespaced — invoke it as
`/review-requested-prs:review-requested-prs` (append ` dry-run` for dry-run), or just pick it from
the `/` menu.

## Requirements

- [Claude Code](https://claude.com/claude-code)
- [GitHub CLI](https://cli.github.com/) (`gh`) authenticated with `repo` scope — check with
  `gh auth status`.

## Install

### As a plugin (recommended)

```
/plugin marketplace add yoones/review-requested-prs
/plugin install review-requested-prs@yoones
```

Reload when prompted; the skill then appears in your `/` menu. Updates ship by re-running
`/plugin marketplace update yoones`.

### Manually

Copy the skill directory into your Claude Code skills folder:

```
git clone https://github.com/yoones/review-requested-prs
cp -r review-requested-prs/skills/review-requested-prs ~/.claude/skills/
```

Claude Code loads `SKILL.md` and exposes it as `/review-requested-prs`.

## Notes

- A pending review is visible **only to you** (with a "Pending" badge) until you click
  **Submit review** on the GitHub UI.
- The review prompts lean toward a Ruby on Rails codebase in the themes they watch, but the
  mechanics work for any repo.
- It won't create a second pending review on a PR that already has one of yours.

## License

MIT — see [LICENSE](LICENSE).
