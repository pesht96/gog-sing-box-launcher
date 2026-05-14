# Plan - Public Xray-only migration rollout

Date: 15-05-2026
Repository: `D:\Projects\GOG_Update\gog-sing-box-launcher-public`
Source staging workspace: `D:\Projects\GOG_Xray_Only_Migration`

## Goal

Publish the verified Xray-only OTA bridge as the public stable update for
ordinary GOG sing-box `v2.07-sb1137` routers. The update must remain manual,
preserve each router's existing subscription/settings state, remove the old
sing-box runtime during migration, install Xray-only, and then move migrated
routers to the Xray-only update compatibility line.

## Release Shape

The public `stable/update.json` must target old routers as:

- `version`: `v2.09-xray-only`
- `compatibility`: `sb1137`
- `min_supported_version`: `v2.07-sb1137`
- `update_kind`: `control_plane`
- `auto_install_allowed`: `false`

The installed app must switch future checks to:

`https://gog-sing-box-launcher.pages.dev/xray/update.json`

The public `xray/update.json` must target migrated routers as:

- `version`: `v2.09-xray-only`
- `compatibility`: `xray-only`
- `update_kind`: `none`

## Safety Gates

Before modifying public files, create timestamped backups for the current
`stable/update.json`, `stable/update.json.sig`, and `README.md`.

Before committing, verify:

- package SHA256 and size match public `stable/update.json`;
- both stable and xray manifests verify with production public key;
- package contains the public Xray manifest URL, not the local staging URL;
- package contains no known router password or user subscription URL;
- package contains no sing-box APK, no `.srs` Re:filter cache files, and no
  Xray ZIP;
- generated `update_apply.sh` and packaged installer pass shell syntax checks;
- public release notes no longer say "staging".

## Publication

Commit only the public update repo changes, push to `origin/main`, then verify
the Cloudflare Pages endpoint:

`https://gog-sing-box-launcher.pages.dev/stable/update.json`

and the package/notes URLs referenced by that manifest.

## Rollback

If publication verification fails, restore the backup of the previous
`v2.07-sb1137` stable manifest and signature, commit, push, and re-check the
Cloudflare Pages endpoint.
