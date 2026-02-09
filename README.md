# Open Source 3D Assets
A curated registry of free 3D assets for games, VR, and creative projects

🌐 [opensource3dassets.com](https://opensource3dassets.com)

## What is this?

A simple directory that helps you find 3D assets you can actually use. Whether you're building a game, creating VR experiences, or need environment models for the metaverse - we've collected links to assets with clear licensing so you know exactly what you can do with them.

## Quick Start

Just want assets? Visit [opensource3dassets.com](https://opensource3dassets.com) to browse and download.

Building something? Grab our JSON files:

- https://raw.githubusercontent.com/ToxSam/open-source-3d-assets/main/data/projects.json
- https://raw.githubusercontent.com/ToxSam/open-source-3d-assets/main/data/assets/pm-momuspark.json

Each asset includes:

- Direct download link to the model file
- Preview images
- Clear license (CC0, CC-BY, etc.)
- Format (GLB, FBX, etc.) and metadata

## Collections

We curate collections from talented creators in the open source community:

**Polygonal Mind Collection (CC0)**

- **MomusPark** - Versatile park environment assets for testing and demos
- **abm** - Museum pack depicting Blockchain and Ethereum history
- **aero-system** - Floating transit system with sci-fi elements
- **avatar-garden** - Landscape assets with Gauguin-inspired art style
- **avatar-show** - Interview space elements with elegant furniture
- **ca-world** - Classical mansion architecture with avant-garde decor
- **christmas** - Seasonal decorations and winter elements
- **chromatic-chaos** - Vaporwave 80s aesthetic with retro furniture
- **cryptoavatars-retro-booth** - 80s-themed virtual booth with Japanese street elements
- **crystal-crossroads** - Moebius-inspired surrealist desert ruins
- **lunar-year** - Asian-inspired Lunar New Year decorative assets
- **medieval-fair** - Medieval festival assets including food booths and structures
- **tomb-chaser-1** - Egyptian pyramid platformer assets
- **tomb-chaser-2** - Neonwave Japanese pagoda with neon lights
- **towers** - Multi-tower dynamic art gallery
- **transit** - Retro-futuristic transport station
- **trash-polka** - Abstract graffiti-style with bold black and red aesthetic
- **xyz** - 60 textured and rigged creatures for 3D printing or any project

All Polygonal Mind collections are CC0 licensed - no attribution needed! More collections from other artists coming soon.

Each collection clearly states its license - check `projects.json` for details.

## For Developers

### Simple Integration

```javascript
// Fetch available collections
const collections = await fetch('https://raw.githubusercontent.com/ToxSam/open-source-3d-assets/main/data/projects.json')
  .then(r => r.json());

// Get assets from a collection
const assets = await fetch(`https://raw.githubusercontent.com/ToxSam/open-source-3d-assets/main/data/${collections[0].asset_data_file}`)
  .then(r => r.json());

// Load the model
const modelUrl = assets[0].model_file_url;
```

### Need help loading 3D files?

- **Web**: Use [three.js](https://threejs.org/) with GLTFLoader
- **Unity**: Import GLB/FBX files directly
- **Unreal**: Use FBX or convert via Datasmith
- **Blender**: File > Import > glTF 2.0 or FBX

## Data Structure

```
/data/
  projects.json          → List of all collections
  /assets/
    pm-momuspark.json    → Assets from each collection
    pm-abm.json
    pm-aero-system.json
    pm-avatar-garden.json
    pm-avatar-show.json
    pm-ca-world.json
    pm-christmas.json
    pm-chromatic-chaos.json
    pm-cryptoavatars-retro-booth.json
    pm-crystal-crossroads.json
    pm-lunar-year.json
    pm-medieval-fair.json
    pm-tomb-chaser-1.json
    pm-tomb-chaser-2.json
    pm-towers.json
    pm-transit.json
    pm-trash-polka.json
    pm-xyz.json
```

- `projects.json` tells you what collections exist and their licenses.
- `assets/*.json` contains the actual asset data (download links, metadata, etc.)

## Contributing

Want to add your collection?

Open a GitHub Discussion with:

- Collection name & description
- License type (must be open source: CC0, CC-BY, etc.)
- Links to model files (IPFS, Arweave, or permanent hosting)
- Preview images

We'll review and add it to the registry!

### Requirements

- ✅ Open source license
- ✅ Permanently hosted files (no temporary links)
- ✅ Preview images
- ✅ Common 3D formats (GLB, FBX, OBJ, etc.)

## License

- **This registry (JSON metadata)**: CC0 (public domain)
- **Individual assets**: Check each collection's license in `projects.json`

## Support

- Questions? [Open a Discussion](https://github.com/ToxSam/open-source-3d-assets/discussions)
- Found a broken link? [Submit an Issue](https://github.com/ToxSam/open-source-3d-assets/issues)

Made by [@ToxSam](https://github.com/ToxSam)
