# Scaffold

A small productivity suite built around one rule: **every tile fixes a behavior by changing the environment, not by reminding you to try harder.** No nagging, no motivational copy — each tool removes an option, adds real friction, or borrows a habit that already works, instead of telling you what to do.

Built for my own behavioral patterns (documented below), not as a generic to-do app.

## The tools

| Tool | Problem it targets | Mechanism |
|---|---|---|
| **Quick Answer** | Mid-task thoughts hijacking focus | Capture the tangent, get one compressed real answer, hard-return to the task. No open chat thread, no rabbit hole. |
| **Build a Plan** | Elaborate plans that never get subtasked | Write the whole plan up front. The UI only ever renders the current step — the rest stay counted but hidden until it's done. |
| **Borrowed Momentum** | Dreaded tasks needing willpower to start | Attach a task to a cue you already do reliably (coffee, a commute leg). The tool surfaces the task only at that cue — an honest manual tap, not a sensor. |
| **Accountability** | Needing outside accountability to move | Tracks committed-vs-delivered counts only, no task detail. Currently local-only — see Known limitations. |

## Design decisions worth knowing

- **No framework.** Four small screens, one user, no shared state complexity that would justify React or a build step. Plain DOM manipulation is the right-sized tool here.
- **Storage is an adapter, not a hardcoded call.** `storeGet`/`storeSet` are the only functions any tile talks to. They currently write to `localStorage`. Swapping to a real backend later (Supabase, most likely) means rewriting those two functions — nothing else in the app changes.
- **Hash-based routing.** Each tool lives at `#tv`, `#lc`, `#bm`, `#pl` so views are shareable and the browser back button works, without a router library.
- **Two considered themes, not one inverted.** Sunshine yellow accent throughout; light theme is a warm cream, dark theme is indigo-charcoal (not a straight brightness-flip of the light palette).

## Known limitations (intentional, not oversights)

- **Accountability is local to your device.** The original design shares committed/delivered counts with one other person. A real shared ledger needs a backend with actual multi-device sync — `localStorage` can't do that. The storage adapter above is built so this is a backend swap, not a rewrite, whenever that's worth building.
- **Quick Answer needs your own Anthropic API key**, entered in Settings and stored only in your browser. This is a deliberate trade-off for a personal, single-user tool — a real product would never call a model API directly from browser JS with an exposed key; that call belongs behind a server.
- **Borrowed Momentum's "cue" is self-reported.** A browser page has no access to the physical world — it can't know when you've had your coffee. The tap is the honesty mechanism, not a placeholder for automation.

## Running it

Just open `index.html` — no build step, no dependencies beyond two Google Fonts loaded via CDN.

## One-time setup: key safety hook

This repo includes a pre-commit hook that blocks any commit containing something that looks like an Anthropic API key — a safety net against ever accidentally hardcoding one into `index.html` instead of entering it in the app's Settings panel at runtime (where it stays local to your browser, never in the repo). Enable it once after cloning:

```
git config core.hooksPath .githooks
```

## Roadmap

- Swap `localStorage` for Supabase to make Accountability genuinely shared
- Reward Rewire tile (pairs task completion with a non-food reward), pending the above
- Friction Toll (browser-extension gate on distracting sites) exists as a separate project, not part of this app
