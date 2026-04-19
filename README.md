# rapidgator-dl

A Docker image for batch-downloading files from Rapidgator. Supports account login for share links and direct CDN URLs, with a TUI progress display.

![Screenshot](https://i.imgur.com/uThb2Uz.jpeg)

## Quick Start

Create a `links.txt` with one URL per line, then run:

```sh
docker run --rm -it \
  -e RG_EMAIL=you@example.com \
  -e RG_PASSWORD='your-password' \
  -v "$PWD/links.txt:/app/links.txt:ro" \
  -v "$PWD/downloads:/app/downloads" \
  gc112/rapidgator-dl:latest
```

Or pass configuration via an `.env` file:

```sh
docker run --rm -it \
  --env-file .env \
  -v "$PWD/links.txt:/app/links.txt:ro" \
  -v "$PWD/downloads:/app/downloads" \
  gc112/rapidgator-dl:latest
```

Downloaded files appear in `./downloads` on the host.

## Links File

One URL per line. Supported:

- Rapidgator share pages: `https://rapidgator.net/file/...` (login required)
- Direct CDN URLs (no login needed)

Example:

```
https://rapidgator.net/file/abc123.../example.mp4.html
https://rapidgator.net/file/def456.../another.zip.html
```

Empty lines and lines starting with `#` are ignored.

## Environment Variables

| Variable | Default | Description |
| --- | --- | --- |
| `RG_EMAIL` | — | Rapidgator account (required for share links) |
| `RG_PASSWORD` | — | Rapidgator password |
| `RG_DELAY` | `30s` | Delay between files (e.g. `45s`, `1m`) |
| `RG_REQUEST_TIMEOUT` | `30s` | Per-request HTTP timeout |
| `RG_RATE_KBPS` | `0` | Download speed cap in KB/s (`0` = unlimited) |

Example `.env`:

```
RG_EMAIL=you@example.com
RG_PASSWORD=your-password
RG_DELAY=45s
RG_RATE_KBPS=2048
```

## Notes

- Credentials are only needed when a `rapidgator.net/file/...` share link is encountered; direct CDN URLs work without login.
- Completed files are skipped on rerun; interrupted downloads resume from `.part` temp files.
- Press `Ctrl+C` to stop safely — progress on the current file is preserved.
