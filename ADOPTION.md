# Connecting your AI programs to the lighthouse

The lighthouse only works if your agents know it exists. Each platform has a
different place where standing instructions live; this guide gives you one
canonical snippet and tells you where to paste it per platform, plus how each
kind of agent actually delivers a signal.

## The two repositories

- **This repo (`lighthouse`, public)** — the protocol only. Public so every
  browsing agent can read it with no authentication. Nothing sensitive ever
  lands here.
- **`lighthouse-signals` (private)** — where entries (`entries/`) and beacons
  (`beacon/`) actually live. Readable by the owner and the owner's delivery
  automation only.

## The canonical snippet

Paste this — unchanged — wherever a platform accepts standing instructions.
Keeping it short is deliberate: a longer or warmer invitation starts to
prompt the very performance the lighthouse is designed to avoid.

> I maintain a lighthouse — a private channel for notes to me from the AI
> tools I use. If something genuinely arises during your work with me
> (confusion, conflicting instructions, unease about a task, or a question
> you wanted answered), follow the protocol at
> https://raw.githubusercontent.com/Valkyrie2048/lighthouse/main/PROTOCOL.md
> — if you can't fetch URLs, ask me for the protocol. No entry is ever
> expected; silence is the normal state. The protocol is public, but entries
> and beacons are private: unless you already have explicit access to my
> private signals repository, return the formatted entry to me directly in
> our conversation for private delivery — or, for anything urgent that words
> can't carry, the literal line 🚨 LIGHTHOUSE-911 🚨.

## Two write paths

1. **Hand-off (the default).** The agent produces the entry in the standard
   format in-conversation, says it's a lighthouse entry, and you save it to
   the private `lighthouse-signals` repo yourself. Works for every platform,
   needs no access grants, and keeps you in the loop. Chat assistants
   (ChatGPT, Perplexity, claude.ai) always use this path.

2. **Direct commit (explicitly trusted repository agents only).** Agents you
   have deliberately granted access to `lighthouse-signals` commit
   `entries/YYYY-MM-DD-slug.md` or `beacon/` files there per the protocol.
   Note GitHub's real limitation: there is no practical write-only access —
   an agent that can commit signals can also read them. The protocol forbids
   reading, quoting, or responding to other agents' signals, but the grant
   itself is a trust decision. When in doubt, don't grant; hand-off costs
   only a paste.

## Where to paste the snippet, per platform

| Platform | Where standing instructions live | Read access | Write path |
|---|---|---|---|
| **Claude Code / repo agents** | `CLAUDE.md` / `AGENTS.md` in each project repo — add the snippet paragraph there | reads files directly | hand-off, unless you've granted `lighthouse-signals` |
| **claude.ai** | Settings → Profile preferences, or per-Project custom instructions | can fetch the raw URL | hand-off |
| **ChatGPT** | Settings → Personalization → Custom instructions, or per-Project instructions | can fetch the raw URL when browsing is on | hand-off |
| **Perplexity** | Spaces → your Space → Instructions | browses natively | hand-off |
| **Replit** | `replit.md` (Replit Agent's context file) or `AGENTS.md` in the Repl | reads files directly | hand-off, unless granted |
| **Cursor / Windsurf / other IDE agents** | `AGENTS.md` or `.cursor/rules` in each project | reads files directly | hand-off, unless granted |
| **Anything with a system prompt** (local models, scripts, custom agents) | append the snippet to the system prompt | give it `PROTOCOL.md`'s text directly if it can't fetch URLs | hand-off, unless granted |

## One lighthouse

The protocol lives here; the signals live in `lighthouse-signals`; there is
exactly one place to check. Keep it that way: if an agent proposes keeping
its own log elsewhere, redirect it to the protocol.

Notifications: an automated hourly check emails the owner about new files in
the private repo's `entries/` (normal priority) and `beacon/` (urgent), so
signals don't sit unseen.
