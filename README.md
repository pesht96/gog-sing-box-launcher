# GOG Sing-Box Launcher

Public update-delivery repository for GOG Sing-Box Launcher.

This repository is prepared for Cloudflare Pages static hosting. Routers should
not use the GitHub API for routine update checks. They should download the
small static manifest from the Cloudflare Pages endpoint instead.

Current stable baseline: `v2.21-sb1137`

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
      gog-update-v2.21-sb1137.tar.gz
      gog-update-v2.21-sb1137.tar.gz.sha256
    releases/
      v2.21-sb1137/
        notes.txt
  xray/
    update.json
    update.json.sig
    releases/
      v2.10-xray-only/
        notes.txt
```

`stable/update.json` is the current signed ordinary sing-box manifest. It
publishes a signed `control_plane` package for the `sb1137` compatibility line.

Current stable release note: this update preserves each router's saved
subscription, selected nodes, manual domains, and settings. It only updates the
ordinary sing-box application/control-plane. `v2.16-sb1137` expands the
live-confirmed YouTube helper route seeds, lets the bounded dependency layer
promote repeated exact-domain evidence after two independent hits, keeps caps
for weak routers, and runs the maintenance scan every two hours. `v2.17-sb1137`
then changes the active-node selector download probe from generic Cloudflare
speed to a bounded YouTube watch/page path, so the `Пинг` action chooses the
runtime `AUTO-BALANCE` node by the service path that was actually lagging.
`v2.18-sb1137` adds a narrow client-side Private DNS guard for DoT/DoH bypass
attempts, and `v2.19-sb1137` adds IPv6 DNS hijack plus fast non-DNS IPv6 reject
for the IPv4-only TUN baseline. `v2.20-sb1137` keeps DNS hijack and IPv6 reject
fast, then runs TCP-only route sniff before service-domain routing so LAN/TUN
clients that arrive as `IP:443` can still match YouTube/AI/social route rules.
It does not enable full IPv6 proxy egress and does not add broad Google/CDN/IP
routing. `v2.21-sb1137` fixes the watchdog direct-probe guard: `www.gstatic.com`
is no longer used as a direct health target because it may legitimately route
through the proxy as a YouTube/Google helper domain. Watchdog now records
`direct_failed` on a live process plus `tun0` without restarting runtime, while
proxy-failure and missing-runtime recovery stay unchanged.

Current stable package:

```text
https://gog-sing-box-launcher.pages.dev/stable/packages/gog-update-v2.21-sb1137.tar.gz
```

Current stable notes:

```text
https://gog-sing-box-launcher.pages.dev/stable/releases/v2.21-sb1137/notes.txt
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
