# PrintBench

**Your 3D printing workspace.**

A self-hosted web app for managing and finding your 3D print files — STL, 3MF,
OBJ and PLY — and for keeping track of what you actually printed.

If your collection is a few thousand models spread across a NAS, an external
drive and a folder called `to sort`, this is for you. PrintBench points at the
folders you already have and indexes them **read-only**. It never moves,
renames or deletes anything. Your files stay yours, in the layout you chose.

## What it does

- **Find things.** Full-text search with faceted filtering by library, creator,
  tag, format and licence — plus a *never printed* facet, for the pile you
  meant to get to. Search state lives in the URL, so a filtered view is
  shareable and the back button works.
- **Look at things.** A thumbnail for every model and an in-browser 3D viewer.
- **Organise things.** Creators, tags with proper management including *merge*
  (libraries reliably grow "dragon", "Dragon" and "dragons" meaning the same
  thing), nestable collections, and a private liked list.
- **Remember what you printed.** Log printer, material, colour, layer height,
  filament used, a rating and notes, against the model you printed it from.
- **Print things.** *Open in…* launches your slicer with the file loaded, and
  *Send* pushes sliced files to OctoPrint, Moonraker or PrusaLink.
- **Share one model** by link — a token you can genuinely revoke, granting
  exactly that model and not your library.
- **Keep it healthy.** Detectors for missing files, duplicate bytes, unreadable
  meshes, models nested inside other models and metadata gaps. Each one clears
  itself once the problem is fixed.

## How it is built

Postgres is the **only** infrastructure dependency.

- **No Redis.** Background jobs run on pg-boss, backed by Postgres.
- **No Elasticsearch.** Search is a weighted Postgres tsvector with a GIN index,
  plus trigram matching so near-misses still find things.
- **No native render toolchain.** Thumbnails come from a pure-TypeScript
  software rasteriser, so a 6GB STL renders in bounded memory, results are
  identical on Windows and Linux, and there is nothing to compile or install.
- **No lock-in.** Metadata is written back to disk as a small sidecar file
  beside each model. Drop the database, migrate, rescan — your tags and notes
  come back from the files themselves.

## Getting started

```bash
cp .env.example .env   # set POSTGRES_PASSWORD and BETTER_AUTH_SECRET
docker compose up -d
```

Then open <http://localhost:8080>. Deployment guides, configuration and the
design notes behind the decisions above live in the main repository.

## Repositories

| Repository | What it is |
| --- | --- |
| [printbench](https://github.com/PrintBench/printbench) | The application — web app, background worker, and everything described here. |

## Contributing

Bug reports and pull requests are welcome; `CONTRIBUTING.md` in the main
repository covers setup and the checks CI runs.

Security vulnerabilities go through private advisories rather than the issue
tracker — please do not open a public issue for one.

---

MIT licensed · © 2026 Owl Media
