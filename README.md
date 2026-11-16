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

## Community Apps submission checklist

Use this repo layout at the **root** of the public GitHub repo (e.g. [EverettIV/borrowarr-unraid](https://github.com/EverettIV/borrowarr-unraid)), then run **Validate** and **Scan** at [ca.unraid.net/submit/new](https://ca.unraid.net/submit/new).

| Requirement | Status |
|-------------|--------|
| Public, active GitHub repo (not archived) | Publish `borrowarr-unraid` |
| OSI license at repo root (`LICENSE`) | MIT in this bundle |
| `ca_profile.xml` at repo root with non-empty `<Profile>` inside `<CommunityApplications>` | `ca_profile.xml` |
| Valid Docker template with `<Repository>` | `templates/borrowarr.xml` |
| Image exists at `<Repository>` | `everettiv/borrowarr:latest` on Docker Hub |

After editing files here, sync to the nested CA repo:

```bash
./scripts/sync-unraid-ca.sh
cd borrowarr-unraid && git add -A && git commit -m "Sync CA templates" && git push
```

Canonical edits live in **`unraid/community-apps/`** (tracked by the main BorrowArr repo). **`borrowarr-unraid/`** at the repo root is a separate git remote and is listed in the parent `.gitignore`.