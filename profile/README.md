# PrintBench

**Your 3D printing workspace.**

PrintBench is a self-hosted web app for managing and finding your 3D print
files (STL, 3MF, OBJ and PLY), and for keeping track of what you have actually
printed.

If your collection is a few thousand models spread across a NAS, an external
drive and a folder called `to sort`, this is built for you. PrintBench points
at the folders you already have and indexes them in place, read-only. It never
moves, renames or deletes anything, so your files stay exactly where you put
them.

![The PrintBench model browser, showing an indexed library](https://raw.githubusercontent.com/PrintBench/.github/main/profile/screenshot.png)

## What it does

- **Find things.** Full-text search with filters for library, creator, tag,
  format and licence, plus a "never printed" filter for the pile you meant to
  get to. The search state lives in the URL, so a filtered view can be shared
  and the back button behaves properly.
- **Look at things.** Every model gets a thumbnail, and there is a 3D viewer
  built into the browser.
- **Organise things.** Creators, tags, nestable collections and a private liked
  list. Tags can be renamed, recoloured and merged, which matters more than it
  sounds: libraries reliably end up with "dragon", "Dragon" and "dragons" all
  meaning the same thing.
- **Remember what you printed.** Log the printer, material, colour, layer
  height, filament used, a rating and any notes, all against the model you
  printed from.
- **Print things.** "Open in" launches your slicer with the file already
  loaded. "Send" pushes sliced files straight to OctoPrint, Moonraker or
  PrusaLink.
- **Share a single model** by link. The token can be revoked, and it grants
  access to that one model rather than to your whole library.
- **Keep it healthy.** Built-in checks look for missing files, duplicate bytes,
  unreadable meshes, models nested inside other models and gaps in metadata.
  Each check clears itself once you fix the problem.

## How it is built

Postgres is the only infrastructure you need.

- **No Redis.** Background jobs run on pg-boss, which is backed by Postgres.
- **No Elasticsearch.** Search is a weighted Postgres tsvector with a GIN
  index, plus trigram matching so near misses still find what you wanted.
- **No native render toolchain.** Thumbnails come from a software rasteriser
  written in pure TypeScript. A 6GB STL renders in bounded memory, the result
  is identical on Windows and Linux, and there is nothing to compile or
  install.
- **No lock-in.** Metadata is written back to disk in a small sidecar file next
  to each model. Drop the database, migrate, rescan, and your tags and notes
  come back from the files themselves.

## Getting started

```bash
cp .env.example .env   # set POSTGRES_PASSWORD and BETTER_AUTH_SECRET
docker compose up -d
```

Then open http://localhost:8080. The main repository covers deployment,
configuration and the reasoning behind the decisions above.

## Repositories

| Repository | What it is |
| --- | --- |
| [printbench](https://github.com/PrintBench/printbench) | The application: web app, background worker, and everything described here. |

## Contributing

Bug reports and pull requests are welcome. `CONTRIBUTING.md` in the main
repository covers how to get set up and which checks CI runs.

Please report security vulnerabilities through private advisories rather than
the issue tracker.

---

MIT licensed. © 2026 Owl Media
