# Claude Code Tips & Best Practices

A collection of advanced tips to get the most out of Claude Code.

---

## 1. Task Management

### Commands & Slash Commands

- If you do something more than once a day, turn it into a **skill** or **command**
- Build a `/techdebt` slash command and run it at the end of every session to find and kill duplicated code
- Set up a slash command that syncs 7 days of Slack, GDrive, Asana, and GitHub into one context dump

### Subagents

- Append `"use subagents"` to any request where you want Claude to throw more compute at the problem
- Offload individual tasks to subagents to keep your main agent's context window clean and focused
- Route permission requests to Opus 4.5 via a hook — let it scan for attacks and auto-approve the safe ones
  - See: [Hooks Documentation](https://code.claude.com/docs/en/hooks#permissionrequest)

---

## 2. CLAUDE.md - Your Most Important Asset

### Continuous Iteration

After every correction, end with:

> *"Update your CLAUDE.md so you don't make that mistake again."*

Claude is eerily good at writing rules for itself.

### Best Practices

- **Ruthlessly edit** your CLAUDE.md over time
- Keep iterating until Claude's mistake rate measurably drops
- Maintain a **notes directory** for every task/project, updated after every PR
- Point your CLAUDE.md at this directory

---

## 3. Custom Skills

### Philosophy

- Create your own skills and commit them to git
- Reuse across every project
- Build **analytics-engineer-style agents** that write dbt models, review code, and test changes in dev

---

## 4. Automated Bug Fixing

### Recommended Workflow

1. Enable the **Slack MCP**
2. Paste a bug thread from Slack into Claude
3. Just say: `"fix"`

No context switching required.

### CI/CD Integration

```
"Fix the failing CI tests"
```

Don't micromanage the *how*. Point Claude at the Docker logs and let it work.

---

## 5. Advanced Prompting Techniques

### Challenge Claude

> *"Grill me on these changes and don't make a PR until I pass your test."*

Make Claude be your reviewer.

### Proof of Function

> *"Prove to me this works"*

Have Claude diff behavior between main and your feature branch.

### Elegant Solutions

After a mediocre fix, say:

> *"Knowing everything you know now, scrap this and implement the elegant solution"*

### Detailed Specs

Write detailed specs and reduce ambiguity before handing work off. The more specific you are, the better the output.

---

## 6. Terminal & Environment Setup

### Recommended Terminal

**Ghostty** is highly appreciated for:
- Synchronized rendering
- 24-bit color
- Proper Unicode support

### Configuration

- Use `/statusline` to customize your status bar to always show:
  - Context usage
  - Current git branch
- Color-code and name your terminal tabs (also with tmux)
- One tab per task/worktree

### Voice Dictation

Use voice dictation! You speak **3x faster** than you type, and your prompts get way more detailed as a result.

- **macOS**: press `fn` twice

📖 Full documentation: [Terminal Config](https://code.claude.com/docs/en/terminal-config)

---

## 7. Data & Analytics with Claude

### Database CLI Integration

Ask Claude Code to use the `bq` CLI to pull and analyze metrics on the fly.

**Recommended setup:**
- Create a BigQuery skill checked into the codebase
- Everyone on the team can use it for analytics queries directly in Claude Code

> *"Personally, I haven't written a line of SQL in 6+ months."*

This works for any database that has a CLI, MCP, or API.

---

## 8. Learning with Claude

### Output Style

Enable the **"Explanatory"** or **"Learning"** output style in `/config` to have Claude explain the *why* behind its changes.

### Visualizations

- Have Claude generate a **visual HTML presentation** explaining unfamiliar code
- It makes surprisingly good slides!

### ASCII Diagrams

Ask Claude to draw ASCII diagrams of new protocols and codebases to help you understand them.

### Spaced Repetition Skill

Build a spaced-repetition learning skill:

1. You explain your understanding
2. Claude asks follow-ups to fill gaps
3. Stores the result for future review

---

## Quick Reference

| Action | Command/Tip |
|--------|-------------|
| Fix bug from Slack | Paste thread + `"fix"` |
| Fix CI | `"Fix the failing CI tests"` |
| Code review | `"Grill me on these changes"` |
| Refactor | `"Scrap this and implement the elegant solution"` |
| More compute | Append `"use subagents"` |
| Update memory | `"Update your CLAUDE.md"` |

---

*Document generated from Claude Code team tips*
