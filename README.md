# Timed Checklist — a to-do list that wakes your assistant

A daily checklist where **any item can carry a one-time timestamp**. When that
time arrives, the backend **wakes your AI assistant and has it proactively
message the user** — a real alarm, not just a passive due-date badge.

Built for an assistant/companion app, but the design is backend-agnostic: a
table, a small REST surface, and a once-a-minute tick.

## What's here

- **[SPEC.md](SPEC.md)** — the full design: data model, time formats, REST API,
  the alarm/wake pipeline (with an atomic-claim + retry pattern and a sequence
  diagram), and the daily auto-clear rules. Written for **bring-your-own-frontend**
  integrators.
- **[examples/receipt-checklist.html](examples/receipt-checklist.html)** — a
  self-contained reference UI (a "paper receipt" bottom-sheet). Open it directly
  in a browser: it renders with mock data, drags between two snap heights, and
  marks every spot where a real API call goes. Theme it with the `--rc-*` CSS
  variables; fill in the two names in the CONFIG block.

## The three item shapes

| Shape | Behavior |
|---|---|
| **Fixed daily** | Recurs every day; un-checks at midnight. |
| **One-off** | A task for today; cleared at midnight. |
| **Timed reminder (alarm)** | Fires at its time (wakes the assistant → it messages the user). Done → cleared next midnight; not done → kept and floated to the top with a `!`. |

## Setting a time (no epoch math)

A create payload takes any one of:

```jsonc
{ "body": "wake me up", "at": "07:00" }   // HH:MM today, or tomorrow if past
{ "body": "stretch",    "in": 30 }        // N minutes from now
{ "body": "…",          "trigger_at": 1783696800000 }  // absolute ms epoch
```

See **[SPEC.md](SPEC.md)** for the endpoints, the tick, and how to run the wake
outside a CPU-metered cron.

## License

[MIT](LICENSE) — do whatever you want with it.
