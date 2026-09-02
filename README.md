[Official release is here: https://github.com/home-assistant/docker/releases/tag/2026.07.0](https://github.com/home-assistant/docker/releases/tag/2026.07.0)

# THIS IS NOT OFFICIAL RELEASE !

---
⚠️ **IMPORTANT DISCLAIMER**

  Unofficial Build: This repository is not created or supported by the official Home Assistant team. Please do not submit issue reports or complaints to Home Assistant developers.

  32-bit Deprecation: Official support for 32-bit systems (armv7) has ended. This repository was created strictly for experimental/testing purposes following official build instructions.

  No Warranty & No Support: This repository is untested and unmaintained. Do not run this in a production environment.

  Limitation of Liability: The creator of this repository assumes no responsibility or liability for any failures, data loss, or damage to hardware. No claims or demands for damages will be entertained.

USE AT YOUR OWN RISK.
---

### STEP No.3

# Home Assistant Core Base Image - for arm 32bit platforms

Home Assistant Core base image built on top of [https://github.com/villgzs/basic-python](https://github.com/villgzs/basic-python).

Includes:

- ssocr
- libcec (+ Python bindings)
- PicoTTS
- Telldus core
- Common HA system packages (bluez, ffmpeg, mariadb, postgresql, etc.)
- Python dependencies from `requirements.txt`

## Base image

```dockerfile
FROM ghcr.io/villgzs/basic-python:latest
```

You can override the base tag via build-arg or the GitHub Actions manual trigger.

## Supported platforms

- `linux/arm/v7`
- `linux/arm/v6`

## Required files

| File / Directory       | Description                                      |
|------------------------|--------------------------------------------------|
| `requirements.txt`     | Python packages to install                       |
| `patches/`             | Telldus patches (already included as placeholders) |
| `rootfs/`              | Extra files copied into the final image root     |

## Quick start

### Local build

```bash
docker buildx build \
  --platform linux/arm/v7 \
  --build-arg BASE_VERSION=latest \
  -t ha-core-base:local \
  --load \
  .
```

### GitHub Actions

The included workflow (`.github/workflows/docker-build.yml`) automatically builds and pushes to GHCR on push to `main`/`master` or on tags.

Manual trigger allows selecting platforms and base image tag.

## Build arguments

| Argument       | Default                          | Description                |
|----------------|----------------------------------|----------------------------|
| `BASE_IMAGE`   | `ghcr.io/villgzs/basic-python`   | Base image repository      |
| `BASE_VERSION` | `latest`                         | Base image tag             |

## Notes

- Multi-stage build with BuildKit cache mounts for faster rebuilds.
- Telldus requires the three patch files in `patches/`.
- `requirements.txt` must exist (even if empty) because it is bind-mounted during the build.
