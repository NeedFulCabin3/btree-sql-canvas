# BTREE SQL Canvas

A browser-native database storage engine built around a custom degree-3 B-Tree index structure, coupled with a handcrafted SQL query engine and a real-time canvas pixel processing engine.

## Overview

Most web applications delegate data indexing and relational storage to server-side processes. `btree-sql-canvas` moves the entire lifecycle into the browser context. It implements a self-balancing B-Tree data structure ($M=3$) to index relational table records directly in JavaScript, exposes a custom-built SQL tokenizer and execution loop, and provides a real-time video canvas workspace that logs frame snapshots into the database.

Everything runs inside the browser tab. Data stays local, queries execute against memory pointers, and state persists across reloads through local browser storage synchronization.

## How It Works

1. **Storage & Indexing Engine**: Records are stored in JavaScript memory inside `SimpleDatabaseEngine`. The primary key (`id`) is indexed using `BTree`, a self-balancing search tree implementation where each node manages up to 3 keys ($2t - 1$ with $t=2$).
2. **SQL Parser & Query Execution**: The `executeSQL()` function parses incoming SQL string statements into command tokens. Queries filtering on `id = X` execute a direct $O(\log n)$ tree point lookup. Range queries or filter commands step through an in-order tree traversal.
3. **Canvas Processing Loop**: A real-time `requestAnimationFrame` loop pulls frame buffers from an active video stream, runs an array-level Euclidean distance comparison for pixel removal, and composites replaced background layers.
4. **Snapshot & Local Persistence**: Capturing a snapshot inserts record metadata into the B-Tree index and serializes the state to browser storage (`localStorage`) for persistence across sessions.

## Key Features

* **Custom B-Tree Implementation**: Self-balancing degree-3 tree managing node splits, recursive insertions, point lookups, and full tree traversals.
* **Handcrafted SQL Processor**: String query parsing supporting `SELECT`, `INSERT`, `WHERE` filtering (`=`, `>`, `<`), and index clearing commands.
* **Direct Point Lookup Optimization**: Queries querying equality on `id` route directly through tree search branches instead of linear array scans.
* **Real-Time Visualizer**: Dynamic HTML rendering showing parent-child node relationships and key distributions across tree branches live as data grows.
* **Canvas Pixel Pipeline**: Parallel green-screen chroma keying loop using direct `ImageData` array manipulation.
* **Persistence Layer**: Automatic state sync with local browser storage on every mutation.

## Tech Stack Breakdown

* **JavaScript (ES6+)**: Custom class architecture (`BTree`, `BTreeNode`, `SimpleDatabaseEngine`) with zero framework dependencies.
* **HTML5 Canvas & Web APIs**: `CanvasRenderingContext2D`, `MediaDevices.getUserMedia`, and `requestAnimationFrame`.
* **Tailwind CSS (CDN)**: Lightweight UI layout framing the canvas viewport and database console.

## Web-Based Quick Start

You can run and test this repository completely through your browser without cloning it to a local machine or installing Node.js dependencies.

### Option A: Using GitHub Codespaces (Browser Terminal & Server)

1. Click the **Code** button at the top right of this repository.
2. Select the **Codespaces** tab and click **Create codespace on main**.
3. Once the cloud editor loads, start a local HTTP server inside the integrated terminal:
   ```bash
   python3 -m http.server 8000
   ```
4. Click the popup notification to open port 8000 in a new browser tab.

### Option B: Local Browser File Access

1. Download index.html from the repository file viewer.

2. Double-click index.html to open it directly inside Chrome, Firefox, Safari, or Edge.

## Repository Structure

```bash
btree-sql-canvas/
├── .github/
│   └── workflows/
│       └── health-check.yml   # Workflow validating file integrity and markup syntax
├── .gitignore                  # Exclusion rules for local OS files and temp artifacts
├── LICENSE                     # MIT Open-Source License terms
├── README.md                   # Repository documentation
└── index.html                  # Single-file entry point housing UI, engine logic, and CSS
```

## Roadmap

[ ] Add support for multi-column indices in the B-Tree data structure.

[ ] Expand SQL tokenizer to support UPDATE queries and AND/OR multi-condition filters.

[ ] Implement disk-style page serialization into IndexedDB for higher record storage capacity.

[ ] Add index deletion algorithms with node rebalancing and merging logic.
