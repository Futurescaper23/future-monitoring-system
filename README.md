# FutureScaping Monitoring System

This folder is the start of the unified estuary monitoring app.

## Run locally

From the workspace root:

```powershell
node future-monitoring-system/server.js
```

Then open `http://localhost:3080`.

## Deploying

This app is best treated as a small Node service, not a static site.

Why:

- `server.js` serves the app itself
- `/api/upload` writes files into `survey-data/`
- `/api/survey-area-metadata` and `/api/volume-change` write updates into `data/projects.json`
- `/api/weather` and `/api/tides` are server routes

### Recommended host

- GitHub for source control
- Render or Railway for hosting

`Netlify` is not the best fit for this version because the current app expects:

- a long-running Node server
- writable local folders
- writable JSON data

### Render-ready setup

This repo now includes:

- `package.json`
- `.env.example`
- `render.yaml`

### Environment variables

Set these in the host:

- `ADMIN_PASSWORD`
- `WORLDTIDES_API_KEY`
- `PORT` (optional if your host sets it automatically)

### Start command

```text
npm start
```

## What this version does

- consolidates the current Padstow prototype apps into one interface
- reads shared project data from `data/projects.json`
- lets you switch between survey rounds, areas, imagery layers, and section profiles
- reads survey-specific assets from `survey-data/`
- includes a seeded baseline survey round for `2025-02-18`
- includes an admin upload tab for writing files into the correct survey and area folder
- includes a survey-wide admin board so you can inspect all 8 areas for the selected round
- uses the darker FutureScaping visual style and a viewer-first layers panel
- supports zoom, wheel zoom, drag-to-pan, and reset in the layers view

## What the survey selector means right now

The baseline survey round has been copied into:

```text
survey-data/padstow-estuary/2025-02-18/area1/
survey-data/padstow-estuary/2025-02-18/area2/
...
```

Later rounds are now genuinely treated as missing until their own files are added. The UI will show that clearly instead of silently reusing the wrong imagery.

Each survey area folder also includes a `manifest.json` checklist describing the expected file set.

## Upload workflow

1. Start the local server.
2. Open the app in the browser.
3. Go to the `Admin` tab.
4. Select the survey round and area at the top of the app.
5. Choose any subset of the expected files and upload them.
6. The files are written into the matching `survey-data/<project>/<survey>/<area>/` folder.
7. The readiness view updates after upload.
8. Use the survey board in the same tab to jump between areas and see what is still missing.

## Next data step

For each future survey round, the intended pattern is:

1. process outputs into a survey folder, for example `padstow/2025-03-19/area1/`
2. store ortho, DSM, contour, and section outputs for each area
3. update `projects.json` so that the selected survey resolves to that round's assets
4. keep the UI unchanged

Expected files per area:

- `ortho.jpg`
- `dsm.png`
- `contour.png`
- `section_lines.png`
- `section_profiles.csv`
- `manifest.json`

## Current structure

- `index.html`: app shell
- `styles.css`: shared styling
- `app.js`: data-driven browser logic
- `data/projects.json`: project metadata and survey records
- `server.js`: tiny local static server for testing
