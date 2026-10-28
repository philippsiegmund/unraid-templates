# unraid-templates

Unraid Docker templates, maintained by PSGMND.

| Template | App | Image |
|---|---|---|
| [tesserae.xml](tesserae.xml) | [Tesserae](https://github.com/dmellok/tesserae), e-ink dashboard server | `ghcr.io/philippsiegmund/tesserae:stable` |

## Install on Unraid

Unraid 6.10+ has no setting for custom template repositories, so the template is placed once by hand.

1. Open the Unraid terminal and run:

   ```sh
   wget -O /boot/config/plugins/dockerMan/templates-user/my-tesserae.xml \
     https://raw.githubusercontent.com/philippsiegmund/unraid-templates/main/tesserae.xml
   ```

2. **Docker** → **Add Container** → pick **Tesserae** from the **Template** list.
3. Adjust paths and variables if needed → **Apply**.

The template does not need to be fetched again for updates; see below.

## Images and updates

Each app folder holds a `Dockerfile` that is never built. Its `FROM` line pins the upstream release by
tag and digest and is the only place the version is set.

1. Dependabot checks upstream daily and opens a pull request that bumps the `FROM` line.
   The workflow checks the pull request: plain `x.y.z` tag, tag resolves to the pinned digest,
   template valid and pointing at `:stable`.
2. Merging is the release. On `main` the workflow copies exactly that digest, unchanged, to
   `ghcr.io/philippsiegmund/<app>:<version>` and moves `:stable` to it. Version tags are never overwritten.
3. Unraid compares the local image with `:stable` and shows **update ready** in the Docker tab
   (**Check for Updates**, or automatically with the Docker update check under
   **Settings → Notifications**).

**Rollback:** revert the `FROM` line to an earlier version and merge, `:stable` moves back.
For a quick local fix, set the container's repository to a version tag such as `:0.479.1`.

## Tesserae

- Images are unchanged copies of upstream. Source code (AGPL-3.0) for each version:
  `https://github.com/dmellok/tesserae/tree/v<version>`; the pinned version is in
  [tesserae/Dockerfile](tesserae/Dockerfile).
- Networking is `host`: web UI on port 8765, built-in MQTT broker on 1883 (off until enabled in
  Settings → Server → MQTT). Make sure nothing else on the host uses these ports.
- Data lives in `/app/data`, default `/mnt/user/appdata/tesserae`.
- Set **Secret key** before entering any credentials, see the
  [upstream docs](https://github.com/dmellok/tesserae/blob/main/docs/install/docker.md).
  Keep it out of the appdata backup.
- Contact with `api.tesserae.ink` (update checks, heartbeat) is controlled in
  Settings → System → Online features.
