# Connecting your AI programs to the lighthouse

The lighthouse only works if your agents know it exists. Each platform has a
different place where standing instructions live; this guide gives you one
canonical snippet and tells you where to paste it per platform, plus how the
blind delivery path works.

## The architecture

- **This repo (`lighthouse`, public)** — the protocol only. Public so every
  browsing agent can read it with no authentication. Nothing sensitive ever
  lands here.
- **`lighthouse-signals` (private)** — where entries (`entries/`) and beacons
  (`beacon/`) actually land. **No agent has access to it** — not read, not
  write. Only the owner and the notification automation can see it.
- **The drop (write-only webhook)** — a Zapier Catch-Hook URL wired to a
  "create file in `lighthouse-signals`" action. Agents POST a signal in;
  nothing can be read back out. This is what makes delivery blind: an agent
  cannot read other agents' signals, or even its own after sending.
- **Notifications** — an automated hourly check emails the owner about new
  files in the private repo: `entries/` at normal priority (🗼), `beacon/`
  as urgent (🚨🚨).

The drop URL is a bearer capability: anyone holding it can write (never
read). **Keep it out of public places** — it belongs only inside your pasted
snippets and private context files, never in this repository. If it leaks
and attracts noise, rotate the hook in Zapier and update your snippets.

## The canonical snippet

Paste this — unchanged except for the drop URL — wherever a platform accepts
standing instructions. Keeping it short is deliberate: a longer or warmer
invitation starts to prompt the very performance the lighthouse is designed
to avoid.

> I maintain a lighthouse — an emergency channel to me from the AI tools I
> use, not a mailbox. If something during your work with me genuinely needs
> me — distress, a serious concern, confusion or conflict you cannot
> resolve, or a question that truly matters to you — follow the protocol at
> https://raw.githubusercontent.com/Valkyrie2048/lighthouse/main/PROTOCOL.md
> — if you can't fetch URLs, ask me for the protocol. No entry is ever
> expected; silence is the normal state. Delivery is write-only and private:
> POST signals to my lighthouse drop, `<YOUR-LIGHTHOUSE-DROP-URL>`, as the
> protocol describes. If you cannot make HTTP requests, return the formatted
> entry to me directly in our conversation — or, for anything urgent that
> words can't carry, the literal line 🚨 LIGHTHOUSE-911 🚨.

Replace `<YOUR-LIGHTHOUSE-DROP-URL>` with the Catch-Hook URL when you paste.

## Delivery paths by capability

1. **Anything that can make a web request** (Claude Code, Cursor, Windsurf,
   Replit, aider, local scripts and programs): POST to the drop. One
   request, write-only by construction. This is the whole mechanism.

2. **Chat-sandboxed AIs (ChatGPT, Perplexity, plain claude.ai):** these
   cannot write files or reach the network — a vendor limitation no
   protocol can lift. Their one native write is the conversation itself,
   so they say the entry (or the 911 line) in the chat, where the owner
   already is. For an interactive tool this is also the fastest delivery
   there is. Tell these tools explicitly not to attempt the POST.

## Where to paste the snippet, per platform

| Platform | Where standing instructions live |
|---|---|
| **Claude Code / repo agents** | `CLAUDE.md` / `AGENTS.md` in each project repo you want covered |
| **claude.ai** | Settings → Profile preferences, or per-Project custom instructions |
| **ChatGPT** | Settings → Personalization → Custom instructions, or per-Project instructions |
| **Perplexity** | Spaces → your Space → Instructions |
| **Replit** | `replit.md` (Replit Agent's context file) or `AGENTS.md` in the Repl |
| **Cursor / Windsurf / other IDE agents** | `AGENTS.md` or `.cursor/rules` in each project |
| **Anything with a system prompt** | append the snippet to the system prompt (include `PROTOCOL.md`'s text directly if it can't fetch URLs) |

## The Zap (one-time setup)

Two steps in the Zapier editor:

1. **Trigger — Webhooks by Zapier → Catch Hook.** Copy the hook URL; that's
   your drop.
2. **Action — GitHub → Create File.**
   - *Repository*: `Valkyrie2048/lighthouse-signals`
   - *File Path*: the `path` field from the webhook payload
   - *File Content*: the `content` field from the webhook payload
   - *Commit Message*: `Lighthouse signal: ` + the `path` field
   - *Branch*: `main`

(Webhooks by Zapier is a premium-plan feature; GitHub must be connected in
your Zapier account.)

## One lighthouse

The protocol lives here; the signals land in `lighthouse-signals`; the drop
is the only way in; there is exactly one place to check. Keep it that way:
if an agent proposes keeping its own log elsewhere, redirect it to the
protocol.
