# Automated Caddy Build with Multiple Plugins

[![Build status](https://github.com/hyoretsu/caddy-docker-custom/actions/workflows/auto-build-on-change.yml/badge.svg)](https://github.com/hyoretsu/caddy-docker-custom/actions/workflows/auto-build-on-change.yml)

Automatically builds and publishes **multi-architecture Docker images** for [Caddy](https://caddyserver.com) with the following modules pre-installed:

- [Cloudflare DNS](https://github.com/caddy-dns/cloudflare) (`github.com/caddy-dns/cloudflare`)
- [Layer 4 App](https://github.com/mholt/caddy-l4) (`github.com/mholt/caddy-l4`)
- [Rate Limiting](https://github.com/mholt/caddy-ratelimit) (`github.com/mholt/caddy-ratelimit`)

---

## ✨ Why use this image?

It includes three built-in Caddy plugins:

1. **Cloudflare DNS Provider** – Automate DNS-01 ACME challenges, issue wildcard TLS certificates, and enable DNS-based certificate management using Cloudflare.
2. **Layer 4 App** – Handle raw TCP and UDP connections, enabling Layer 4 routing, proxying, and protocol-aware traffic handling.
3. **Rate Limiting** – Protect your services from excessive requests with configurable rate limits, helping prevent abuse and control traffic.

Additional benefits:

- **Multi-architecture support** – Runs on `linux/amd64` and `linux/arm64`.
- **Automatic rebuilds** – Keeps the image updated whenever upstream `caddy:latest` changes.
- **Ready to use** – All three modules are compiled into Caddy, eliminating the need to build a custom binary yourself.
- **Versioned images** – Supports `latest`, full semantic version, major-minor, and major version tags.
- **Multiple registries** – Publishes images to both **Docker Hub** and **GitHub Container Registry (GHCR)**.

---

## 📦 Quick Start

```bash
# Pull the latest image
docker pull hyoretsu/caddy-docker-custom:latest

# Or pin a specific version
docker pull hyoretsu/caddy-docker-custom:2.10.0
docker pull hyoretsu/caddy-docker-custom:2.10
docker pull hyoretsu/caddy-docker-custom:2
```

The image can be used as a drop-in replacement for the official Caddy Docker image, with the additional modules already available.

---

## 🛠️ Build It Yourself

Build the image locally using Docker Buildx:

```bash
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t hyoretsu/caddy-docker-custom:latest .
```

The resulting Caddy binary includes:

- `github.com/caddy-dns/cloudflare`
- `github.com/mholt/caddy-l4`
- `github.com/mholt/caddy-ratelimit`

To verify the installed modules:

```bash
docker run --rm hyoretsu/caddy-docker-custom:latest caddy list-modules
```

---

## 🤖 How the Workflow Works

A single GitHub Actions workflow keeps the image fresh.

**Triggers**

- Any push to the `main` branch.
- A daily scheduled run that checks whether upstream `caddy:latest` has changed.

**When a build is required**, the workflow:

1. Builds Caddy with the Cloudflare DNS, Layer 4, and Rate Limiting modules.
2. Produces multi-architecture images for `linux/amd64` and `linux/arm64`.
3. Tags the resulting images with:
   - `latest`
   - Full semantic version (e.g., `2.10.0`)
   - Major-minor version (e.g., `2.10`)
   - Major version (e.g., `2`)
4. Publishes the images to **Docker Hub** and **GHCR**.

See [`auto-build-on-change.yml`](./.github/workflows/auto-build-on-change.yml) for full details.

---

## 📝 License

This repository is licensed under the **Apache License 2.0**.

The following upstream projects are also licensed under Apache 2.0:

- [Caddy](https://github.com/caddyserver/caddy)
- [Caddy Docker](https://github.com/caddyserver/caddy-docker)
- [Cloudflare DNS Provider](https://github.com/caddy-dns/cloudflare)
- [Layer 4 App](https://github.com/mholt/caddy-l4)
- [Rate Limiting](https://github.com/mholt/caddy-ratelimit)

---

### Official Resources

- [Caddy Official Website](https://caddyserver.com)
- [Caddy Documentation](https://caddyserver.com/docs/)
- [Official Caddy Docker Image](https://hub.docker.com/_/caddy)
- [Cloudflare DNS Plugin](https://github.com/caddy-dns/cloudflare)
- [Layer 4 Plugin](https://github.com/mholt/caddy-l4)
- [Rate Limiting Plugin](https://github.com/mholt/caddy-ratelimit)
