# Agent guide: Open Source 3D Assets registry

This document helps AI agents and automation integrate or extend the 3D assets registry with minimal errors.

## What this repo is

- **Registry**: Curated list of open-source 3D asset collections and their assets.
- **Data**: All listable data lives under `data/`: `data/projects.json` (collections) and `data/assets/*.json` (assets per collection).
- **Schema**: Full field reference is in [docs/data-schema.md](docs/data-schema.md).

## Quick reference

| What | Where |
|------|--------|
| List of collections | `data/projects.json` |
| Assets for a collection | `data/assets/<project-id>.json` (e.g. `data/assets/pm-momuspark.json`) |
| Schema & optional fields | [docs/data-schema.md](docs/data-schema.md) |

---

## Adding a new collection (project)

1. **Choose a unique project `id`**
   - Format: `<creator-prefix>-<slug>` (e.g. `pm-momuspark`, `acme-dungeon-pack`).
   - Must be unique in `data/projects.json`.

2. **Add one entry to `data/projects.json`** (array of objects). Required fields:

   ```json
   {
     "id": "pm-newpack",
     "name": "New Pack",
     "creator_id": "Polygonal Mind",
     "description": "Short description of the collection.",
     "is_public": true,
     "license": "CC0",
     "source_type": "github",
     "storage_type": "github",
     "github_url": "https://github.com/org/repo/tree/main/path/to/project",
     "created_at": "2026-02-09T18:22:04.150Z",
     "updated_at": "2026-02-09T18:22:04.150Z",
     "asset_data_file": "assets/pm-newpack.json"
   }
   ```

   - `asset_data_file`: path **under** `data/`, e.g. `assets/pm-newpack.json` (file will live at `data/assets/pm-newpack.json`).
   - Use ISO 8601 for `created_at` and `updated_at`.

3. **Create the asset file**
   - Path: `data/assets/<project-id>.json`.
   - Content: JSON **array** of asset objects (see below). Can be `[]` initially.

4. **Keep JSON valid**
   - No trailing commas, no comments. Validate with a JSON linter if needed.

---

## Adding or editing assets

Assets live in `data/assets/<project-id>.json`. Each file is an **array** of asset objects.

### Required fields per asset

| Field | Type | Notes |
|-------|------|--------|
| `id` | string | Unique within the registry (e.g. `momuspark-001`, or `projectslug-001`). |
| `name` | string | Display name. |
| `project_id` | string | Must equal the project `id` in `projects.json` that owns this asset file. |
| `description` | string | Short description. |
| `model_file_url` | string | Direct URL to the 3D file (GLB, FBX, etc.). |
| `format` | string | e.g. `GLB`, `FBX`, `VRM`. |
| `thumbnail_url` | string | URL to preview image. |
| `is_public` | boolean | Whether the asset is listed. |
| `is_draft` | boolean | If `true`, may be hidden in production. |
| `created_at` | string | ISO 8601. |
| `updated_at` | string | ISO 8601. |

### Optional: `metadata`

- **Original-style:** `number`, `alternateModels` (e.g. `{ "vrm": "url", "fbx": "url" }`).
- **NFT-style:** `token_id`, `attributes` (array of `{ "trait_type", "value", "display_type"? }`), `external_url`, `alternateModels`.

### Example asset object

```json
{
  "id": "momuspark-001",
  "name": "Bench_01_Art",
  "project_id": "pm-momuspark",
  "description": "3D model from momuspark collection",
  "model_file_url": "https://raw.githubusercontent.com/org/repo/main/projects/MomusPark/Bench_01_Art.glb",
  "format": "GLB",
  "thumbnail_url": "https://raw.githubusercontent.com/org/repo/main/projects/MomusPark/Bench_01_Art_thumbnail.png",
  "is_public": true,
  "is_draft": false,
  "created_at": "2026-02-09T18:22:04.150Z",
  "updated_at": "2026-02-09T18:22:04.150Z",
  "metadata": {
    "number": "001",
    "github_path": "projects/MomusPark/Bench_01_Art.glb",
    "file_size": 878104,
    "attributes": [
      { "trait_type": "Theme", "value": "Nature Park" },
      { "trait_type": "Category", "value": "Park Furniture" }
    ]
  }
}
```

### Rules to avoid mistakes

- `project_id` in every asset must match the `id` of the project in `projects.json` and the filename: `data/assets/<project_id>.json`.
- Asset `id`s must be unique across the whole registry.
- Use permanent URLs for `model_file_url` and `thumbnail_url` (e.g. raw GitHub, IPFS, Arweave).

---

## Using the registry in another project (consumption)

Agents integrating this registry into games, apps, or tools can:

1. **Fetch collections**
   - URL: `https://raw.githubusercontent.com/ToxSam/open-source-3d-assets/main/data/projects.json`
   - Returns array of projects; each has `id`, `name`, `license`, `asset_data_file`, etc.

2. **Fetch assets for a collection**
   - From a project object: `asset_data_file` is relative to `data/` (e.g. `assets/pm-momuspark.json`).
   - Full URL: `https://raw.githubusercontent.com/ToxSam/open-source-3d-assets/main/data/<asset_data_file>`
   - Example: `https://raw.githubusercontent.com/ToxSam/open-source-3d-assets/main/data/assets/pm-momuspark.json`

3. **Resolve model and thumbnail**
   - Use `model_file_url` for the 3D file and `thumbnail_url` for preview images.
   - Prefer `format` and `metadata.alternateModels` when multiple formats are needed.

Example (pseudo-code):

```javascript
const projects = await fetch('https://raw.githubusercontent.com/ToxSam/open-source-3d-assets/main/data/projects.json').then(r => r.json());
const assetFile = projects.find(p => p.id === 'pm-momuspark').asset_data_file;
const assets = await fetch(`https://raw.githubusercontent.com/ToxSam/open-source-3d-assets/main/data/${assetFile}`).then(r => r.json());
const firstModelUrl = assets[0].model_file_url;
```

---

## Checklist for adding a new collection

- [ ] New project has a unique `id` in `data/projects.json`.
- [ ] `asset_data_file` is `assets/<project-id>.json` (under `data/`).
- [ ] File `data/assets/<project-id>.json` exists and is a JSON array.
- [ ] Every asset has `project_id` equal to that project `id`.
- [ ] All asset `id`s are unique.
- [ ] All required asset fields present; dates in ISO 8601.
- [ ] No trailing commas or comments in JSON; array/file is valid JSON.

For full field list and optional fields (e.g. NFT-style, `source_network`), see [docs/data-schema.md](docs/data-schema.md).
