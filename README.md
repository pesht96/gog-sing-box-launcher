# GOG Sing-Box Launcher

Public update-delivery repository for GOG Sing-Box Launcher.

This repository is prepared for Cloudflare Pages static hosting. Routers should
not use the GitHub API for routine update checks. They should download the
small static manifest from the Cloudflare Pages endpoint instead.

Current stable baseline: `v2.32-sb1137`

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
      gog-update-v2.32-sb1137.tar.gz
      gog-update-v2.32-sb1137.tar.gz.sha256
    releases/
      v2.32-sb1137/
        notes.txt
  xray/
    update.json
    update.json.sig
    releases/
      v2.10-xray-only/
        notes.txt
```

`stable/update.json` is the current signed ordinary sing-box manifest. It
publishes a signed `control_plane` package for the `sb1137` compatibility line
with `auto_install_allowed=true`, so it can be installed manually or by the
existing auto-install path when the operator has auto-install enabled. The
package still performs a bounded current-payload runtime reapply when GOG was
already running before update.

Current stable release note: this update preserves each router's saved
subscription, selected nodes, manual domains, and settings. `v2.32-sb1137`
adds Discord to the bounded `Иностранные сервисы` and blocked/Re:filter
service-core route surface. The supported Discord suffixes are
`discord.com`, `discord.gg`, `discordapp.com`, `discordapp.net`,
`discordcdn.com`, and `discord.media`. The package is published as
`control_plane` with `auto_install_allowed=true`, so routers can install it
manually or through the existing auto-install path.

Current stable package:

```text
https://gog-sing-box-launcher.pages.dev/stable/packages/gog-update-v2.32-sb1137.tar.gz
```

Current stable notes:

```text
https://gog-sing-box-launcher.pages.dev/stable/releases/v2.32-sb1137/notes.txt
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
