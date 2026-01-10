# Glyphh AI Releases

Public, read-only repository for Glyphh Runtime and SDK release artifacts. Contains downloads and release notes only (no source code).

## SDK (Wheel) Install

Use the SDK wheel for local model development and testing.

```
python3.10 -m venv .venv
source .venv/bin/activate
pip install /path/to/glyphh-*.whl
```

Download the wheel from the Releases page.

## Install (Manual)

1. Download the latest runtime archive from the Releases page.
2. Unpack the archive and move it to your install directory.

Example:

```
tar -xzf glyphh-runtime-vX.Y.Z.tar.gz
cd glyphh-runtime-vX.Y.Z-<platform>
```

## Configure (Environment)

Required:

- `GLYPH_DATABASE_URL` (runtime DB connection string)

Recommended:

- `GLYPH_RUNTIME_STORAGE_PATH`
- `GLYPH_RUNTIME_PORT` (default 8080)
- `GLYPH_RUNTIME_LOG_LEVEL` (default INFO)

Example:

```
export GLYPH_DATABASE_URL="postgresql+psycopg://user:pass@host:5432/glyphh_runtime"
export GLYPH_RUNTIME_STORAGE_PATH="/var/lib/glyphh/runtime"
export GLYPH_RUNTIME_PORT=8080
export GLYPH_RUNTIME_LOG_LEVEL=INFO
```

## Database Setup

The runtime uses Postgres + pgvector. Migrations are applied on startup by
default (`GLYPH_RUN_MIGRATIONS=true`).

Option A: Docker Compose (recommended)

```
curl -L -o docker-compose.runtime.yml https://raw.githubusercontent.com/glyphh-ai/glyphh-runtime/main/docker-compose.runtime.yml
docker compose -f docker-compose.runtime.yml up -d
```

This starts a local Postgres with pgvector at:

```
postgresql+psycopg://glyphh:glyphh@localhost:5432/glyphh_runtime
```

Option B: Manual Postgres

1. Install Postgres 16+ and pgvector.
2. Create a database and user for the runtime.
3. Set `GLYPH_DATABASE_URL` to point at it.

Example (psql):

```
CREATE ROLE glyphh WITH LOGIN PASSWORD 'glyphh';
CREATE DATABASE glyphh_runtime OWNER glyphh;
\\c glyphh_runtime
CREATE EXTENSION IF NOT EXISTS vector;
```

> Note: On startup, the runtime applies database migrations automatically.
> To disable auto-migrations (and manage them manually), set:

```
export GLYPH_RUN_MIGRATIONS=false
```

## License Request Payload

On first run, the runtime writes a license request payload you can send to
your admin for issuance. Default path:

```
~/.glyphh/runtime/license_request.json
```

Share that JSON with your admin, then place the issued license at:

```
~/.glyphh/runtime/license.json
```

## License File

Local runtimes require a signed license file. Obtain it from the Glyphh AI customer portal.

```
mkdir -p ~/.glyphh/runtime
cp ~/Downloads/license-dev.json ~/.glyphh/runtime/license.json
```

## Start the Runtime

```
./run.sh
```

If you prefer to run directly:

```
./venv/bin/python -m uvicorn api.main:app --host 0.0.0.0 --port ${GLYPH_RUNTIME_PORT:-8080}
```

Windows (PowerShell):

```
.\run.ps1
```

## Import a Bundle

```
glyphh runtime-import --auto --dir /path/to/bundle --runtime-url http://localhost:8080
```
