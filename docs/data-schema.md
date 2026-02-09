# Data Schema

Same structure as [open-source-avatars](https://github.com/ToxSam/open-source-avatars), with `avatar_data_file` → `asset_data_file` and `avatars/` → `assets/`.

## projects.json

Array of collection/project objects.

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique project id (e.g. `example-original`) |
| `name` | string | Display name |
| `creator_id` | string | Creator identifier |
| `description` | string | Short description |
| `is_public` | boolean | Whether listed publicly |
| `license` | string | e.g. CC0, CC-BY |
| `asset_data_file` | string | Path under `data/`, e.g. `assets/example-original.json` |
| `created_at` | string | ISO 8601 date |
| `updated_at` | string | ISO 8601 date |

**Optional (original):** `source_type` (`"original"`), `storage_type` (e.g. `"arweave"`).

**Optional (NFT):** `source_type` (`"nft"`), `source_network` (string or array, e.g. `"base"` or `["ethereum","base"]`), `source_contract` (string or array), `storage_type` (string or array, e.g. `"ipfs"`), `opensea_url`.

## assets/*.json

Array of asset objects.

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique asset id (e.g. UUID or `contract/token_id`) |
| `name` | string | Display name |
| `project_id` | string | Must match a project `id` in projects.json |
| `description` | string | Short description |
| `model_file_url` | string | Direct URL to the 3D file |
| `format` | string | e.g. GLB, FBX, VRM |
| `thumbnail_url` | string | URL to preview image |
| `is_public` | boolean | Whether listed publicly |
| `is_draft` | boolean | If true, may be hidden in production |
| `created_at` | string | ISO 8601 date |
| `updated_at` | string | ISO 8601 date |
| `metadata` | object | See below |

### metadata (common)

- **Original-style:** `number`, `alternateModels` (e.g. `{ "vrm": "url", "fbx": "url" }`).
- **NFT-style:** `token_id`, `attributes` (array of `{ "trait_type", "value", "display_type"? }`), `external_url` (e.g. OpenSea item), `alternateModels` (e.g. `{ "glb": "url" }`).
