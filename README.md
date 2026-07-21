# ClaudeTraining
Anthropic Academy training links for SAIGE
# Getting Started with Claude & Claude Code
### A learning pathway for data scientists

This guide collects the best free, official training for getting productive with Claude — in the chat interface for everyday analysis and writing, and in **Claude Code** for real programming work. Everything here is Claude-based because SAIGE provides Claude licenses to members.

**Time investment:** ~2–4 hours to work through the core pathway. You'll come out with a certificate, a working prompt collection, and Claude Code running on your machine.

---

## 📚 The pathway at a glance

| Step | Resource | What you get | Time |
|------|----------|--------------|------|
| 1 | [Anthropic Academy](https://anthropic.skilljar.com) — Claude 101 | Core features & workflows, certificate | ~1 hr |
| 2 | [AI Fluency: Framework & Foundations](https://anthropic.skilljar.com) | Mental model for what AI can/can't do | ~1 hr |
| 3 | [Prompt Library](https://docs.anthropic.com/en/resources/prompt-library/library) | Working prompts to reverse-engineer | ongoing |
| 4 | [Prompt engineering docs](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview) | The techniques behind good prompts | ~1 hr |
| 5 | [Claude Code](https://code.claude.com/docs) | Agentic coding in your terminal | ~30 min setup |

---

## 1. Anthropic Academy — start here

**Main site:** https://anthropic.skilljar.com
**Claude 101 directly:** https://anthropic.skilljar.com/claude-101

Anthropic's free, self-paced course platform. Video modules with an end quiz; pass it and you earn a certificate you can add to LinkedIn.

**Getting started:**

1. Go to [anthropic.skilljar.com](https://anthropic.skilljar.com) and create a free account — email only, under two minutes, then click **Enroll** on any course. This is a Skilljar account, separate from any Claude account. No payment, no API key required.
2. **Start with Claude 101.** It covers Claude's core features, the everyday workflows professionals use, and the practices that separate productive use from frustrating trial-and-error — content drafting, structured prompts for recurring tasks, summarisation.
3. **Then take AI Fluency: Framework & Foundations** (same catalog). This is the strategic, tool-agnostic companion — building a mental model for how AI works, what it can and can't do. The clearest on-ramp if you want the *why* before the *how*.
4. Work through the modules and pass the quiz for your certificate.

> 💡 **For data scientists:** the Academy catalog also includes courses on **Claude Code**, the **API**, and **Model Context Protocol (MCP)**. Once you've done the two above, browse the catalog — the Claude Code course is the natural third step for anyone who writes code.

---

## 2. The Prompt Library — your working reference

**Link:** https://docs.anthropic.com/en/resources/prompt-library/library

A grid of named, optimized prompts for real tasks — extracting insights from long reports, a meeting scribe, an email extractor, a SQL generator, a Python bug-buster, a CSV converter, a data organizer, and dozens more.

**How to use it:**

1. Open the link and pick a prompt close to something you actually do. **Read the prompt itself, not just the output.**
2. Reverse-engineer it: why did they assign a role? Why structure it that way? Many use XML-style tags like `<task>` and `<output_format>` to separate instructions from content — notice that pattern, because it's a core Claude technique.
3. Copy a prompt, swap in your own details, run it, tweak it.
4. Build your own small collection of "prompts that worked" — that's the real payoff. (Consider keeping them in a shared repo or team wiki.)

**Starting points for data scientists:** *SQL sorcerer*, *Python bug buster*, *Data organizer*, *CSV converter*, *Corporate clairvoyant* (long-report distillation), *Meeting scribe*.

---

## 3. Prompt & context engineering docs

**Overview:** https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview

Save this for after Claude 101 — it's more technical and assumes the basics. It's written for people building with Claude, but the principles apply directly to chat use.

1. Read the overview page first, then follow the side-menu into specific techniques: being clear and direct, using examples (multishot), giving Claude a role, letting Claude think step-by-step, structuring with XML tags.
2. The overview also links to Anthropic's prompting tools and interactive tutorials if you prefer hands-on learning.
3. For long-document work — the bread and butter of research — the key idea is **context engineering**: with large or data-rich inputs (20k+ tokens), *what* you put in front of the model and *how you organise it* matters as much as the instruction. Put documents first, instructions and questions last, and wrap documents in tags.

---

## 4. Claude Code — Claude in your terminal

**Docs:** https://code.claude.com/docs
**Claude Code prompt library:** https://code.claude.com/docs/en/prompt-library

Claude Code is an agentic coding tool: it reads your repo, writes and edits files, runs your code and tests, and works through multi-step tasks with your approval. For data scientists this is where Claude stops being a chatbot and starts being a colleague — debugging a pipeline, refactoring analysis scripts, writing tests, wrangling messy data files, or explaining an unfamiliar codebase.

**Getting started:**

1. Install and set up following the [quickstart](https://code.claude.com/docs). It runs in your terminal (Windows via WSL, macOS, Linux) and authenticates with your Claude account.
2. `cd` into a project and just describe what you want: *"explain what this repo does"*, *"find why this script fails on the 2024 data"*, *"add type hints and docstrings to utils.py"*.
3. Browse the [Claude Code prompt library](https://code.claude.com/docs/en/prompt-library) — copy-paste prompts tagged by task and role, each with a *"Why this works"* explanation. The core pattern: **describe the outcome, not the steps**, and let Claude find the files.
4. Add a `CLAUDE.md` file to your repo root. Claude Code reads it automatically at the start of every session — use it for project conventions, environment quirks, "always use the venv", "never touch the raw data folder", etc. This is the single highest-leverage habit for repeatable results.

**Data-science workflow tips:**

- Use **plan mode** (`Shift+Tab`) for anything non-trivial: Claude proposes a plan before touching files, and you approve or redirect.
- Work in a **git branch**. Claude Code + version control means every change is reviewable and reversible.
- Ask Claude to **write a test or validation check first**, then implement — this works remarkably well for data-cleaning and transformation code.
- It's excellent at the unglamorous stuff: converting notebooks to scripts, adding logging, updating deprecated pandas/geopandas calls, writing argparse wrappers.

---

## 5. Going further

- **Anthropic Cookbook** — https://github.com/anthropics/anthropic-cookbook — runnable Jupyter notebooks demonstrating API patterns: tool use, RAG, vision, structured outputs. The bridge from chat use to building.
- **Anthropic courses repo** — https://github.com/anthropics/courses — the interactive prompt-engineering tutorial as notebooks you can run yourself.
- **Anthropic Learn hub** — https://www.anthropic.com/learn — guides and best-practice write-ups, including how Anthropic's own teams use Claude Code.
- **Docs home** — https://docs.claude.com — API reference, model comparisons, and the full platform documentation.

---

## ⚠️ Working with sensitive data

Before pasting anything into any AI tool, know your organisation's policy. As a baseline:

- **Don't paste** personal information, confidential records, credentials, or unpublished sensitive data into prompts.
- Prefer **synthetic or de-identified samples** when you need Claude to understand a data structure — a fake five-row CSV teaches Claude the schema just as well as the real file.
- With Claude Code, be deliberate about **what's in the working directory** — Claude can read files in the project it's pointed at.
- Check whether your license (individual vs. Team/Enterprise) matches your organisation's data-handling requirements before using it on work material.

---

## Suggested first-week checklist

- [ ] Create Skilljar account, complete **Claude 101**
- [ ] Complete **AI Fluency: Framework & Foundations**
- [ ] Copy one **Prompt Library** prompt, adapt it to a real task you do weekly
- [ ] Read the **prompt engineering overview** + the XML tags page
- [ ] Install **Claude Code**, run it on a low-stakes repo, add a `CLAUDE.md`
- [ ] Save your three best prompts somewhere you'll find them again

---

*Questions, corrections, or prompts worth sharing? Open an issue or PR.*
