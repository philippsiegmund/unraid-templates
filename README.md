# unraid-templates

Unraid Docker templates, maintained by PSGMND.

| Template | App | Image |
|---|---|---|
| [tesserae.xml](tesserae.xml) | [Tesserae](https://github.com/dmellok/tesserae), e-ink dashboard server (AGPL-3.0) | `ghcr.io/dmellok/tesserae` |

The templates only point at the upstream images; nothing is rebuilt or redistributed here.

## Install on Unraid

1. **Docker** tab → **Template Repositories** (bottom of the page) → add
   `https://github.com/philippsiegmund/unraid-templates` → **Save**.
2. **Add Container** → pick the template from the **Template** list.
3. Adjust paths and variables if needed → **Apply**.

## Tesserae

- Networking is `host`: web UI on port 8765, built-in MQTT broker on 1883 (off until enabled in
  Settings → Server → MQTT). Make sure nothing else on the host uses these ports.
- Data lives in `/app/data`, default `/mnt/user/appdata/tesserae`.
- Set **Secret key** before entering any credentials, see the
  [upstream docs](https://github.com/dmellok/tesserae/blob/main/docs/install/docker.md).
  Keep it out of the appdata backup.
- Contact with `api.tesserae.ink` (update checks, heartbeat) is controlled in
  Settings → System → Online features.

## Updating a template

Images are pinned to a release tag. To update, bump the tag in `<Repository>` (and the `<Icon>` URL)
to a newer [stable release](https://github.com/dmellok/tesserae/releases), commit and push.
An installed container keeps its own local copy of the template, so on Unraid edit the container,
set the new tag in **Repository** and **Apply**.
