# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Communication & Workflow

- Keep responses concise; explain what changed and why after edits
- Read existing code before suggesting changes; prefer editing over creating files
- Only add comments where logic is non-obvious; do not add docstrings or type annotations to unchanged code
- Use the todo list for multi-step tasks; mark tasks complete immediately when done

## Code Style

- Primary language: Python (PEP 8); also work across Node.js, C++, and mixed stacks
- Prefer clarity over cleverness; avoid over-engineering for hypothetical future needs
- Avoid unnecessary abstractions for single-use operations

---

## Projects in This Workspace

This home directory contains four independent projects.

### openai-codex-demo (Node.js / ESM)

**Commands:**
```sh
cd openai-codex-demo
npm install
npm start            # Runs index.js — calls GPT-4o-mini API
npm run calc         # CLI calculator (add/sub/mul/div)
npm run check-api-key  # Verify OPENAI_API_KEY is set
```

Requires `OPENAI_API_KEY` in a `.env` file. The project uses ES modules (`"type": "module"`). `index.js` handles rate-limit (429) and auth (401) errors and falls back to a hardcoded response when no API key is present.

---

### welcome-to-docker (React 18 + Docker)

**Commands:**
```sh
cd welcome-to-docker
npm install
npm start    # Dev server (react-scripts)
npm test     # Jest via react-scripts
npm run build  # Production build
```

**Multi-container Todo app** (Express + MongoDB):
```sh
cd welcome-to-docker/multi-container-app
docker compose up        # Starts todo-app (port 3000) + mongo:6 (port 27017)
npm run dev              # nodemon dev server (inside container or local)
```

**Architecture:** The React app (`src/App.js`) renders a welcome screen with tsparticles confetti (`src/Confetti.js`) and social share buttons. The multi-container sub-app is an Express/EJS todo app using Mongoose, with Docker Compose watch/sync for live reload during development.

**Docker production build:** Multi-stage Dockerfile — `node:18-alpine` build, then `serve` to host the static output.

---

### snort3 (C++ IPS / CMake)

**Build:**
```sh
cd snort3
export my_path=/path/to/snorty
./configure_cmake.sh --prefix=$my_path
cd build
make -j $(nproc) install
# Verify: src/snort -V
```

Requires: cmake ≥ 3.4.3, g++ ≥ 5 (C++14), daq, libdnet, flex ≥ 2.6.0, LuaJIT, OpenSSL, pcap, pcre, zlib, hwloc, pkgconfig.

**Run examples:**
```sh
$my_path/bin/snort -r a.pcap                              # Examine pcap
$my_path/bin/snort -c $my_path/etc/snort/snort.lua        # Verify config
$my_path/bin/snort --help-module suppress                 # Module help
```

**Architecture:** Plugin-based multi-threaded IPS. Key areas: `src/` (52 subdirs — detection engine, protocol inspectors, decoders), `lua/` (scriptable config via `snort.lua` / `snort_defaults.lua`), `daqs/` (Data Acquisition modules for packet capture). Linting enforced via `.clang-tidy` (clang-diagnostic, clang-analyzer, readability, bugprone, modernize checks).

---

### personal-analytics-engine (Python)

Empty skeleton. Intended pipeline: `ingest.py` → `transform.py` → `analytics.py` → `anomalies.py` → `reporting.py`. Install deps with `pip install -r requirements.txt`.
