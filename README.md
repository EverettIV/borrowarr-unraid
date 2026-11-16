# BorrowArr — Unraid Community Apps

Public **Community Apps** template repository for [BorrowArr](https://github.com/thefourthlion/borrowarr): one Docker image (web UI + API) for Unraid.

| Resource | Link |
|----------|------|
| Docker image | [`everettiv/borrowarr`](https://hub.docker.com/r/everettiv/borrowarr) |
| App source & docs | [BorrowArr](https://github.com/thefourthlion/borrowarr) |
| Submit / validate | [Community Apps](https://ca.unraid.net/submit/new) |

## Install on Unraid

1. Install from **Community Apps** (after this template is published), or **Docker → Add Container** using `templates/borrowarr.xml`.
2. Image: `everettiv/borrowarr:latest`
3. Map **Appdata** → `/app/data`, **Media** → `/media`, **Downloads** → `/downloads`
4. Publish **3012** (UI) and **3013** (API)
5. Open **http://[unraid-ip]:3012** and register the first user (admin)

Default paths inside the container: `/media/movies`, `/media/tv`, `/downloads/movies`, `/downloads/tv`.

## Support

- [Issues in this repo](https://github.com/EverettIV/borrowarr-unraid/issues) — template / CA listing
- [BorrowArr issues](https://github.com/thefourthlion/borrowarr/issues) — application bugs and features

## Maintainer notes

This repo is only XML + icon for Unraid CA. **No secrets, no `.env`, no database.** Sync changes from the main app repo under `unraid/community-apps/` when the template changes.

Build and push the image from the main BorrowArr repo (`Dockerfile.all-in-one`); see [unraid/README.md](https://github.com/thefourthlion/borrowarr/blob/main/unraid/README.md) there.