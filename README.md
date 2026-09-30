# OpenKE App Store Repository

Official application repository and catalog distribution system for [OpenKE](https://github.com/OpenKlipperEdition/OpenKE) and [GuppyScreen](https://github.com/OpenKlipperEdition/GuppyScreen).

## Repository Architecture

```
AppStore/
├── apps/                          # Per-application JSON definitions
│   ├── web_ui/                    # Web interfaces (Mainsail, Fluidd, etc.)
│   ├── touch_ui/                  # Touchscreen UIs (GuppyScreen, HelixScreen, etc.)
│   ├── plugin/                    # Moonraker companions & plugins (Timelapse, Spoolman, etc.)
│   └── tool/                      # System & diagnostic CLI utilities
├── categories.json                # App categories and icons
├── dist/                          # Compiled catalog output (apps.json, catalog.json)
├── tools/
│   ├── build_catalog.py           # Catalog compiler and schema validator
│   └── serve.py                   # Local development HTTP server
├── tests/
│   └── test_catalog.py            # Automated test suite
└── .github/workflows/
    └── deploy.yml                 # Automated validation & GitHub Pages deployment
```

## Adding a New Application

To submit a new application or service to the OpenKE App Store:

1. Create a JSON definition file under `apps/<category>/<app_id>.json`:

```json
{
  "id": "my_app",
  "name": "My Application",
  "category": "plugin",
  "description": "Short description of what this application or plugin does.",
  "author": "Author or Project Name",
  "version": "1.0.0",
  "type": "plugin",
  "is_builtin": false,
  "download_url": "https://github.com/example/my_app/releases/download/v1.0.0/my_app.tar.gz",
  "sha256": "optional_sha256_checksum",
  "install_path": "/usr/data/openke/apps/my_app",
  "probe_path": "/usr/data/openke/apps/my_app/main.py",
  "source_url": "https://github.com/example/my_app"
}
```

2. Run the test suite and build the catalog:
```bash
python3 tests/test_catalog.py
python3 tools/build_catalog.py
```

3. Submit a Pull Request. Once merged, GitHub Actions will compile and publish the updated catalog to GitHub Pages.

## Local Testing

To start a local test server serving `apps.json`:
```bash
python3 tools/build_catalog.py
python3 tools/serve.py --port 8000
```
Then point your local OpenKE printer or GuppyScreen to `http://<your-ip>:8000/apps.json`.
