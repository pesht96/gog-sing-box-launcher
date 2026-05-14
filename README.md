# GOG Sing-Box Launcher

Public update-delivery repository for GOG Sing-Box Launcher.

This repository is prepared for Cloudflare Pages static hosting. Routers should
not use the GitHub API for routine update checks. They should download the
small static manifest from the Cloudflare Pages endpoint instead.

Current stable baseline: `v2.09-xray-only`

## Cloudflare Pages

Recommended Cloudflare Pages settings:

```text
Framework preset: None
Build command: leave empty
Build output directory: update_server/public
Root directory: /
```

After deployment, the stable update manifest will be available at:

```text
https://gog-sing-box-launcher.pages.dev/stable/update.json
```

Current production Cloudflare Pages endpoint:

```text
https://gog-sing-box-launcher.pages.dev
```

## Update Layout

```text
update_server/public/
  _headers
  stable/
    update.json
    update.json.sig
    packages/
      .gitkeep
      gog-update-v2.09-xray-only-migration.tar.gz
      gog-update-v2.09-xray-only-migration.tar.gz.sha256
    releases/
      v2.09-xray-only/
        notes.txt
  xray/
    update.json
    update.json.sig
    releases/
      v2.09-xray-only/
        notes.txt
```

`update.json` is the current signed stable manifest. It currently publishes a
signed manual `control_plane` bridge package that lets ordinary
`v2.07-sb1137` routers migrate to the Xray-only runtime.

Current stable release note: this update preserves each router's saved
subscription and settings, stops the old sing-box runtime, installs bundled
Xray and lite Re:filter assets, rebuilds runtime from the saved payload,
validates Xray, and then moves future update checks to the Xray-only
compatibility line. The bridge is manual-only; operators should wait a minute
or two and refresh the UI if the old updater appears to time out during the
core replacement.

Current stable package:

```text
https://gog-sing-box-launcher.pages.dev/stable/packages/gog-update-v2.09-xray-only-migration.tar.gz
```

Current stable notes:

```text
https://gog-sing-box-launcher.pages.dev/stable/releases/v2.09-xray-only/notes.txt
```

Migrated Xray-only routers continue on:

```text
https://gog-sing-box-launcher.pages.dev/xray/update.json
```

## Security Model

The intended production updater model is:

1. Download `update.json`.
2. Verify `update.json.sig` with the public update key bundled in GOG.
3. Compare semantic GOG version and compatibility line.
4. Download the package from `package_url`.
5. Verify `package_sha256` and `package_size`.
6. Create state and code backups on the router.
7. Apply the update.
8. Verify runtime health.
9. Roll back automatically if verification fails.

Private signing keys must never be committed to this repository.
