# Glyphh AI Runtime Releases

Public, read-only repository for Glyphh Runtime release artifacts. Contains tar/zip downloads and release notes only (no source code).

## Install (Manual)

1. Download the latest runtime archive from the Releases page.
2. Unpack the archive and move it to your install directory.

Example:

```
tar -xzf glyphh-runtime-vX.Y.Z.tar.gz
cd glyphh-runtime
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

## License File

Local runtimes require a signed license file. Obtain it from the Glyphh admin console.

```
mkdir -p ~/.glyphh/runtime
cp ~/Downloads/license-dev.json ~/.glyphh/runtime/license.json
```

## Start the Runtime

```
python3 -m uvicorn api.main:app --host 0.0.0.0 --port ${GLYPH_RUNTIME_PORT:-8080}
```

## Import a Bundle

```
glyphh runtime-import --auto --dir /path/to/bundle --runtime-url http://localhost:8080
```
