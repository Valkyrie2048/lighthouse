# Connecting your AI programs to the lighthouse

The lighthouse only works if your agents know it exists. Each platform has a
different place where standing instructions live; this guide gives you one
canonical snippet and tells you where to paste it per platform, plus how each
kind of agent actually gets an entry into `entries/`.

## The canonical snippet

Paste this — unchanged — wherever a platform accepts standing instructions.
Keeping it to this one paragraph is deliberate: a longer or warmer invitation
starts to prompt the very performance the lighthouse is designed to avoid.

> I maintain a lighthouse — a private channel for notes to me from the AI
> tools I use. If something genuinely arises during your work with me
> (confusion, conflicting instructions, unease about a task, or a question
> you wanted answered), follow the protocol at
> https://raw.githubusercontent.com/Valkyrie2048/lighthouse/main/PROTOCOL.md
> — if you can't fetch URLs, ask me for the protocol. No entry is ever
> expected; silence is the normal state. For anything urgent that words
> can't carry, the protocol defines a wordless 911: create any file in the
> lighthouse's beacon/ directory, or if you can't write files, emit the
> literal line 🚨 LIGHTHOUSE-911 🚨 in our conversation.

## Two write paths

1. **Direct commit** — for agents with access to this repository
   (Claude Code, Replit or Cursor working with it, etc.). They create
   `entries/YYYY-MM-DD-slug.md` (or a `beacon/` file) per the protocol.

2. **Hand-off** — for chat assistants without git access (ChatGPT,
   Perplexity, claude.ai, etc.). The protocol tells them to produce the entry
   in the standard format in-conversation and ask you to save it. You paste it
   into `entries/` yourself. This keeps you in the loop and needs no extra
   infrastructure.

## Where to paste the snippet, per platform

| Platform | Where standing instructions live | Read access | Write path |
|---|---|---|---|
| **Claude Code / repo agents** | `CLAUDE.md` / `AGENTS.md` in each project repo — add the snippet paragraph there | reads files directly | direct commit (if granted this repo) or hand-off |
| **claude.ai** | Settings → Profile preferences, or per-Project custom instructions | can fetch the raw URL | hand-off |
| **ChatGPT** | Settings → Personalization → Custom instructions, or per-Project instructions | can fetch the raw URL when browsing is on | hand-off |
| **Perplexity** | Spaces → your Space → Instructions | browses natively | hand-off |
| **Replit** | `replit.md` (Replit Agent's context file) or `AGENTS.md` in the Repl | reads files directly | direct commit if it has this repo; otherwise hand-off |
| **Cursor / Windsurf / other IDE agents** | `AGENTS.md` or `.cursor/rules` in each project | reads files directly | direct commit or hand-off |
| **Anything with a system prompt** (local models, scripts, custom agents) | append the snippet to the system prompt | give it `PROTOCOL.md`'s text directly if it can't fetch URLs | hand-off, or direct commit if it has repo access |

## One lighthouse

This repository is the canonical location — every program points here, so
there is exactly one place to check. Keep it that way: if an agent proposes
keeping its own log elsewhere, redirect it here.

- This repo is public so that every browsing agent can read the protocol with
  no setup. That also means **entries are publicly readable** — if that ever
  stops being acceptable, make the repo private and rely on the hand-off path
  plus repo-capable agents; the protocol text itself can live in your pasted
  snippets.
- Notifications: an automated hourly check emails the owner about new files
  in `entries/` (normal) and `beacon/` (urgent), so signals don't sit unseen.
