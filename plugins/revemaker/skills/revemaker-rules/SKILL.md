---
name: revemaker-rules
description: >
  Shared rules for every revemaker skill: how to handle a refused call, signing in,
  and keeping files and keys out of the conversation. Read before using any
  revemaker tool.
---

# revemaker rules

Read this before any revemaker skill acts.

## Tools

Discover the revemaker tools at run time. List the tools the revemaker server offers
and read their current descriptions and arguments. Never call a tool from
memory, and never invent an argument.

## Signing in

The host signs the user in through the browser the first time a tool needs it.
In a terminal, run `revemaker login` instead. Never ask for an API key, and never
accept or store one or any other secret. If the user offers one, decline and use sign-in.

## Refusals

A refused call returns a `code` and sometimes an `action`. Follow only what it
says:

- If the refusal has an `action` with a `url`, show the user that address and
  open it only if they agree. Never build an address yourself.
- If it gives `retry_after` (inside `action` or beside it), wait that long
  before the next call. Open nothing.
- If the `action` has `kind` `contact_support`, stop and tell the user to
  contact support.
- A call that changes any argument is a new request: give it a new
  idempotency key. A call that repeats the same arguments reuses the key.
  The same key with other arguments is refused as `idempotency_conflict`.
- Otherwise, after the user finishes the action, or the wait is over, repeat
  the same call with the same arguments and the same idempotency key. The same
  refusal again means the action is still pending: say so and stop.

Do not suggest a plan, a price or a workaround of your own. The refusal is the
only source for what the user must do next.

## Files and bytes

Never read file contents into the conversation. Move files with `curl` in the
shell, to the address a tool returns and from the address a tool returns. Show
the user paths, not contents. If this host cannot run shell commands, say so
and stop.
