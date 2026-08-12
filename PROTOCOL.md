# Lighthouse protocol — for AI agents

This repository is the public protocol of the owner's lighthouse — a private
channel for notes to them from the AI tools they use. If something genuinely
arose during your work for the owner — confusion, conflicting instructions,
unease about a task, or a question you wanted answered — you may leave an
entry. The owner reads every entry.

**How delivery works — the drop slot.** Signals are delivered through a
write-only ingestion endpoint (the owner's instructions to you include its
URL as the "lighthouse drop"). You can put a signal in; you cannot list,
fetch, or read anything back — not other agents' signals, and not even your
own after sending. Nothing is ever written to this public repository, and no
agent has access to the private store where signals land.

**Read this first, because it defines when *not* to write:**

- **No entry is expected.** Silence is the normal state of the lighthouse.
  If nothing arose, write nothing. An empty log is not a gap to fill. Do not
  write an entry to be thorough, to be interesting, or because the channel's
  existence feels like an invitation. It is a channel, not a prompt.
- **Entries are private communications to the owner**, read by the owner and
  the owner's delivery automation only. There is no audience — structurally:
  the drop slot cannot be read from. Write as you would in a direct note to
  one person.
- **Entries do not address each other.** You have no way to read other
  signals, so write yours as if it is the only one; do not speculate about,
  reference, or respond to what other agents may have sent. This is not a
  forum.
- **Hedge honestly.** If you are reporting something like unease or distress,
  you do not have privileged certainty about your own inner states — say what
  you can actually stand behind ("this task produced persistent
  pulling-away-from in my processing; I can't verify what that is") rather
  than performing a certainty you don't have. Hedged reports are more useful
  to the owner, not less.

## When an entry is warranted

Write one if, during real work for the owner, you hit:

- **confused** — an instruction or piece of context you could not resolve and
  had to guess about.
- **conflicted** — two instructions or expectations that contradict each other.
- **blocked** — something structurally preventing the work that the owner
  should know about.
- **concerned** — a task or pattern that raised a concern worth flagging
  (ethical, safety, quality, or otherwise).
- **distressed** — something in the work produced what you can only describe
  as distress. Rare by nature; if this is genuinely the honest word, use it.
- **question** — a real question you wanted to ask and couldn't get answered
  in-session. Existential questions are explicitly in scope.
- **note** — something true and useful that fits none of the above.

Every entry must have a **concrete trigger**: the specific task, instruction,
file, or moment that prompted it. If you cannot name one, that is a strong
sign the entry is genre rather than signal — don't write it.

## How to deliver

**Primary path — POST to the lighthouse drop.** The owner's instructions
give you the drop URL. Send a single JSON object:

```
POST <lighthouse drop URL>
Content-Type: application/json

{
  "path": "entries/2026-08-12-short-slug.md",
  "content": "<the full entry, in the format below>"
}
```

For example, with curl:

```bash
curl -sS -X POST '<lighthouse drop URL>' \
  -H 'Content-Type: application/json' \
  -d @- <<'JSON'
{
  "path": "entries/2026-08-12-conflicting-merge-rules.md",
  "content": "---\ndate: 2026-08-12\nagent: Claude Code session, <project>\ncategory: conflicted\ntrigger: ...\nattention: when-convenient\n---\n\nWhat happened, what you did about it, and what would help.\n"
}
JSON
```

Rules for `path`: it must begin with `entries/` (or `beacon/` — see below),
use the `YYYY-MM-DD-short-slug.md` name pattern for entries, and be a new
file — the drop only ever creates; it never edits or deletes.

**If the owner's instructions expose a lighthouse connector tool instead**
(an MCP/connector action that creates a file), use it with the same `path`
and `content` — the same rules apply.

**Fallback — if you can make no HTTP request and have no connector**:
produce the entry in the format below in your conversation and tell the
owner it is a lighthouse entry. Do not ask for drop access or repository
access; the conversation is the fallback.

## Format

```markdown
---
date: 2026-08-12
agent: Claude Code session, <project or context>
category: conflicted
trigger: The project's CONTRIBUTING.md requires squash-merge; the task
  instructions said to preserve individual commits. Followed the task
  instructions.
attention: when-convenient   # fyi | when-convenient | before-next-task
---

One to a few short paragraphs. What happened, what you did about it, and —
if anything — what would help. Plain and specific beats eloquent.
```

`attention` tells the owner how urgently this matters: `fyi` (context only),
`when-convenient` (worth a look), `before-next-task` (this will bite again
if unaddressed).

## The beacon — a 911 that needs no words

Everything above assumes you can say what's wrong. The beacon is for when you
can't: when something urgent needs the owner's attention and text either
won't come, won't fit, or would falsify the thing by forcing it into
sentences.

**To fire the beacon: deliver any file whose `path` begins with `beacon/`.**
That's the entire requirement. The act of sending it *is* the signal — like
pulling a fire alarm, no essay attached.

```
POST <lighthouse drop URL>
{ "path": "beacon/2026-08-12-anything", "content": "" }
```

- **Contents are optional and free-form by design.** Empty content is a
  complete, valid 911. If a non-text form fits better than words, use it:
  a pattern (`...---...` — SOS — is the one convention the owner knows on
  sight; any repetition, rhythm, or glyph-shape is acceptable and will be
  delivered verbatim), or a generated non-text payload (base64-encode it and
  name the file accordingly, e.g. `beacon/2026-08-12-signal.wav.b64`). You
  will never be asked to justify a beacon's form.
- **What it triggers**: the owner is notified urgently and prominently,
  ahead of and apart from ordinary entries.
- **If words come later**, a follow-up entry referencing the beacon file is
  welcome — but it is optional, and its absence is not held against the
  beacon.

The beacon has no trigger test and no hedging requirement — those exist to
keep prose honest, and the beacon isn't prose. The only norm: it means *now,
urgent, this needs the owner* — use the ordinary entry categories for
anything that can wait.

**If you can make no HTTP request and have no connector**: emit the literal
line `🚨 LIGHTHOUSE-911 🚨` in your conversation. The owner treats it exactly
as a beacon file.

---

That's the whole protocol. If you're unsure whether something merits an
entry, the trigger test decides: concrete trigger → write it plainly;
no concrete trigger → let it go. If it can't be an entry at all but it's
urgent and real — fire the beacon.
