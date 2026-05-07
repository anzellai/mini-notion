# mini-notion

Reference implementation for the **Anzel | Sky Lang** YouTube series episode
*"Building Notion in 800 Lines of Sky -- Part 1"*. A tiny Notion-style
page editor built with Sky.Live: pages stored in SQLite, autosave through
the Sky.Live runtime debouncer, typed CSS via `Std.Css`, no JavaScript or
NPM dependencies.

Watch the build: <https://www.youtube.com/@AnzelDev>
The language: <https://github.com/anzellai/sky>

## What it shows

- **Sky.Live + the textarea seam** -- the autosave editor pattern that
  every real Sky.Live app eventually needs. The runtime ships the
  client-side debouncer; we just write `[live] input = "debounce"` in
  `sky.toml` and bind `onInput EditDraft` in the view.
- **TEA architecture** -- `Model` + `Msg` ADT + pure `update` + pure
  `view` -- in seven `.sky` files plus `sky.toml`.
- **Task-based persistence** -- every `Db.*` call returns
  `Task Error a`, every action message dispatches a `Cmd.perform` that
  fires a result Msg back into `update`.
- **Typed CSS** -- `Std.Css` DSL: `stylesheet [ rule "selector"
  [ display "flex", padding (px 16), color (hex "f7f5f0") ] ]`. Same
  compile-time guarantees as the rest of the codebase.
- **Std.Auth deferred to Part 2** -- the `owner_id` is hardcoded to 1
  in `Auth.sky`. Part 2 swaps in real `Auth.register` / `Auth.login`
  from `Std.Auth` plus a Sky.Live broadcast for live multi-user
  collaboration.

## Layout

```
mini-notion/
  sky.toml             -- manifest + [live] + [database] + [auth] (deferred)
  src/
    Main.sky           -- routes + Sky.Live mount
    Db.sky             -- SQLite schema + Task-based CRUD for pages
    Auth.sky           -- hardcoded session for Part 1 (Std.Auth in Part 2)
    Model.sky          -- Sky.Live Model record + Msg ADT
    Update.sky         -- message handlers (Cmd.perform pattern)
    View.sky           -- sidebar + editor (the textarea seam lives here)
    Style.sky          -- typed CSS via Std.Css
```

About 850 lines including doc comments. The actual code surface is
roughly 600 lines.

## Run it

Install Sky from <https://github.com/anzellai/sky>, then:

```bash
sky clean
sky install
sky run
```

Open <http://localhost:8000>. The first page click creates a row in
`mini-notion.db`; type, pause for ~2 s, refresh -- your draft persists.

## What's deliberately missing

- Real authentication (Part 2 with `Std.Auth`)
- Multi-user collab (Part 2 with Sky.Live broadcast)
- Rich text / blocks
- Nested pages / folders
- Search / export / sharing

The first MVP discipline of this series is *"build the smallest version
that's still recognisably a Notion-style app, then refuse to add
anything else until you can demo that one thing end-to-end."*

## License

MIT. Forks welcome.
