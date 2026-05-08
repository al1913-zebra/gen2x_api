# Gen2X API Reference

Source repository for the **Impinj Gen2X API Reference** documentation — covering MQTT and REST APIs for Gen2X features on Zebra fixed RFID readers.

**Published docs:** [al1913-zebra.github.io/gen2x_api](https://al1913-zebra.github.io/gen2x_api/)  
**Staging deployment:** [zebra-stage.github.io/dcs/rfid/gen2x](https://zebra-stage.github.io/dcs/rfid/gen2x/)

## Supported Hardware

| Reader | Minimum Firmware |
|--------|-----------------|
| FXR90  | 4.0.8           |

## Features Documented

| Feature | Scope | Description |
|---------|-------|-------------|
| Protected Mode | Tag-scoped | Lock/unlock tags with a 32-bit access password |
| FastID | Reader-scoped | Return EPC + TID in a single inventory response |
| TagFocus | Reader-scoped | Silence already-read tags to prioritize new ones |
| Tag Quieting | Reader-scoped | Quiet specific tags by EPC ID |

## Repository Structure

```
gen2x_api/
├── docs/                   # Built documentation (GitHub Pages source)
│   ├── index.html          # Main API reference page (MQTT + REST)
│   ├── rest.html           # REST-only Swagger/OpenAPI viewer
│   ├── openapi_md.json     # OpenAPI spec driving the main page
│   ├── rest_openapi.yaml   # OpenAPI 3.1 spec for REST endpoints
│   ├── rest_artifacts.json # Bundled REST request/response examples
│   └── css/, assets/       # Styles and images
├── rest api/               # Per-operation JSON schemas and examples
│   ├── enableTagProtection/
│   ├── disableTagProtection/
│   ├── enableFastid/
│   ├── enableTagfocus/
│   ├── quietTags/
│   ├── start/
│   ├── stop/
│   └── ...                 # One folder per API operation
├── schemas/                # Source schema definitions
│   ├── commands/gen2x/     # MQTT command schemas
│   ├── response/gen2x/     # Response payload schemas
│   ├── operation_descriptions/ # Markdown descriptions per operation
│   └── tag_descriptions/   # Section-level feature descriptions
└── scripts/                # Build and generation tooling (Python)
    ├── generate_openapi_tags_md.py      # Generates openapi_md.json
    ├── generate_combined_descriptions.py # Generates HTML descriptions
    ├── bundle_rest_artifacts.py         # Bundles REST examples into JSON
    └── gen2x_rest_tester.py             # REST API integration tester
```

## How It Works

1. **Source schemas** in `schemas/` and `rest api/` define request/response structures and operation descriptions for each Gen2X endpoint.
2. **Python scripts** in `scripts/` compile these schemas into the OpenAPI specs and bundled artifacts consumed by the documentation pages.
3. **Built output** in `docs/` is served via GitHub Pages — `index.html` renders the unified MQTT + REST reference, while `rest.html` provides a Swagger UI for the REST-only OpenAPI spec.

## Build & Regenerate Docs

```bash
# Generate the combined OpenAPI + descriptions JSON
python scripts/generate_openapi_tags_md.py

# Generate HTML operation descriptions
python scripts/generate_combined_descriptions.py

# Bundle REST artifacts into a single JSON file
python scripts/bundle_rest_artifacts.py
```

Output files are written to `docs/`. Commit and push to deploy via GitHub Pages.

## API Endpoints

| Operation | MQTT Command | REST Method | REST Path |
|-----------|-------------|-------------|-----------|
| Configure Gen2X features | `set_impinjGen2X` | `PUT` | `/cloud/impinjGen2X` |
| Get current configuration | `get_impinjGen2X` | `GET` | `/cloud/impinjGen2X` |
| Start radio (apply config) | `start` | `PUT` | `/cloud/start` |
| Stop radio | `stop` | `PUT` | `/cloud/stop` |

## Contributing

1. Edit source schemas in `schemas/` or `rest api/` directories.
2. Run the appropriate generation script(s) from `scripts/`.
3. Verify the output in `docs/` renders correctly.
4. Commit all changes (source + generated output) and push.
