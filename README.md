# GOG Sing-Box Launcher

Public update-delivery repository for GOG Sing-Box Launcher.

This repository is prepared for Cloudflare Pages static hosting. Routers should
not use the GitHub API for routine update checks. They should download the
small static manifest from the Cloudflare Pages endpoint instead.

Current stable baseline: `v2.05-sb1137`

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
      gog-update-v2.05-sb1137.tar.gz
      gog-update-v2.05-sb1137.tar.gz.sha256
    releases/
      v2.05-sb1137/
        notes.txt
```

`update.json` is the current signed stable manifest. It currently publishes a
signed `control_plane` package for `v2.05-sb1137`.

Current stable release note: this update keeps the `v2.02` narrow
unsupported-core xhttp diagnostic, the `v2.03` operator-preemption lock fix,
and the `v2.04` VLESS Reality no-flow compatibility fix. It also publishes the
modern sing-box URI compatibility pass for VLESS, Trojan, VMess, Shadowsocks,
SOCKS, Hysteria2/hy2, TUIC, and AnyTLS through the common preview/cache/config
path.

Current stable package:

```text
https://gog-sing-box-launcher.pages.dev/stable/packages/gog-update-v2.05-sb1137.tar.gz
```

Current stable notes:

```text
https://gog-sing-box-launcher.pages.dev/stable/releases/v2.05-sb1137/notes.txt
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
