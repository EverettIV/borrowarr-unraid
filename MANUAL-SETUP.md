# BorrowArr on Unraid — manual container setup

Use this when Community Apps isn’t installed yet. **One container** = UI + API (`everettiv/borrowarr:latest`).

**Requires:** Image on Docker Hub with **`linux/amd64`** (build with `--platform linux/amd64`).

---

## 1. Open Add Container

1. Unraid web UI → **Docker** tab  
2. **Add Container**  
3. Leave **Template** as “Select a template”  

---

## 2. Basic fields

| Field | Value |
|--------|--------|
| **Name** | `BorrowArr` |
| **Repository** | `everettiv/borrowarr:latest` |
| **Network Type** | `Bridge` |
| **Console shell command** | `Shell` |
| **Use Tailscale** | Off |
| **Privileged** | Off |

---

## 3. Advanced View → Extra Parameters

1. Top right: switch **Basic View** → **Advanced View**  
2. Find **Extra Parameters** (near Repository / Network)  
3. Paste:

```text
--add-host=host.docker.internal:host-gateway
```

*(Optional but helps the container reach qBittorrent/SAB on the Unraid host. If this field is missing, use the tower LAN IP in download client settings instead, e.g. `http://192.168.0.66:<port>`.)*

---

## 4. Path mappings

Click **+ Add another Path, Port, Variable, Label or Device** → **Path** for each row.

**Config type:** Path · **Access mode:** Read/Write  

| Name (label) | Container path | Host path (example — use yours) |
|--------------|----------------|----------------------------------|
| Appdata | `/app/data` | `/mnt/user/appdata/borrowarr` |
| Media | `/media` | `/mnt/user/Portkeylabs/docker/sharedmedia` |
| Downloads | `/downloads` | `/mnt/user/appdata/qbittorrentvpn/qBittorrent/downloads` |

Create `/mnt/user/appdata/borrowarr` on the host first if it doesn’t exist.

Inside the container BorrowArr expects:

- `/media/movies` and `/media/tv` (library)  
- `/downloads/movies` and `/downloads/tv` (incoming)  

Create those folders on the host under your mappings, or set `BORROWARR_ENSURE_MEDIA_DIRS=true` (step 5).

---

## 5. Port mappings

**+ Add another…** → **Port** (twice). **Protocol:** TCP  

| Name | Container port | Host port |
|------|----------------|-----------|
| Web UI | `3012` | `3012` |
| API | `3013` | `3013` |

Keep host **3013** published — the browser uses the same Unraid IP on port **3013** for the API.

---

## 6. Environment variables

**+ Add another…** → **Variable** for each row.  
Fill **Key** and **Value** (Name is optional label).

| Key | Value |
|-----|--------|
| `BORROWARR_MEDIA` | `/media` |
| `BORROWARR_DOWNLOADS` | `/downloads` |
| `SQLITE_STORAGE_PATH` | `/app/data/database.sqlite` |
| `BORROWARR_ENSURE_MEDIA_DIRS` | `true` *(first install; use `false` later if you manage folders yourself)* |
| `NODE_ENV` | `production` |
| `API_PORT` | `3013` |
| `CLIENT_PORT` | `3012` |

**Optional (TMDb, your keys):**

| Key | Value |
|-----|--------|
| `TMDB_API` | *(your v3 API key)* |
| `TMDB_API_READ` | *(your read access token)* |

---

## 7. Create the container

1. **Apply**  
2. Wait for image pull (should succeed on **amd64** towers)  
3. **Docker** → **BorrowArr** → **Logs** — you should see the API and Next.js start  

**Web UI:** `http://<unraid-ip>:3012`  
Example: `http://192.168.0.66:3012`

---

## 8. First run in the app

1. **Register** — first account is **admin** (public signup closes after that)  
2. **Settings → Indexers** — add Torznab/Newznab, test, enable  
3. **Settings → Download Clients** — add qBittorrent (or SAB, etc.)  

**Download client URL examples (qB on Unraid host):**

| Method | URL |
|--------|-----|
| With extra host (step 3) | `http://host.docker.internal:<qb-webui-port>` |
| Without extra host | `http://192.168.0.66:<qb-webui-port>` |

Use the **Web UI port** from your qBittorrent VPN container (not the BT port).

4. Align qB **save path** with your **Downloads** mapping so categories/paths match `/downloads/movies` and `/downloads/tv`.  
5. **File Management** in BorrowArr should show your `/media` and `/downloads` trees when paths are correct.

---

## 9. Edit later

**Docker** → **BorrowArr** → **Edit** → change paths/vars → **Apply** (container recreates).

To update the app image: **Edit** → ensure Repository is still `everettiv/borrowarr:latest` → **Apply** (pulls newer `latest` if you pushed one).

---

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| `no matching manifest for linux/amd64` | Re-push image with `docker buildx build --platform linux/amd64 … --push` |
| UI loads, login/API fails | Publish host port **3013** |
| Can’t reach qB | Fix client URL; try tower IP or `host.docker.internal` + extra param |
| Empty library paths | Check volume mappings and `BORROWARR_*` vars; create `movies`/`tv` subfolders |

---

## Checklist (printable)

- [ ] Repository `everettiv/borrowarr:latest`  
- [ ] Advanced → Extra Parameters (`host.docker.internal`)  
- [ ] Appdata → `/app/data`  
- [ ] Media → `/media`  
- [ ] Downloads → `/downloads`  
- [ ] Ports **3012**, **3013**  
- [ ] Variables (at least `BORROWARR_MEDIA`, `BORROWARR_DOWNLOADS`, `SQLITE_STORAGE_PATH`)  
- [ ] Apply, open `:3012`, register admin  
- [ ] Indexers + download client configured  