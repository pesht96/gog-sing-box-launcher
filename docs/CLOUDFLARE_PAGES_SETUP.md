# Cloudflare Pages Setup

Use this repository as a static Cloudflare Pages deployment.

## Steps

1. Open Cloudflare Dashboard.
2. Go to `Compute` -> `Workers & Pages`.
3. Choose `Pages`.
4. Choose `Create application`.
5. Choose `Connect to Git`.
6. Select the GitHub repository `pesht96/gog-sing-box-launcher`.
7. Use these build settings:

```text
Framework preset: None
Build command: leave empty
Build output directory: update_server/public
Root directory: /
```

8. Deploy the project.
9. Open the generated `*.pages.dev` URL.
10. Verify that this endpoint returns JSON:

```text
https://gog-sing-box-launcher.pages.dev/stable/update.json
```

Current deployed Pages endpoint:

```text
https://gog-sing-box-launcher.pages.dev
```

## Current State

The current manifest is a placeholder. It does not publish an installable
package yet. This is intentional: the GOG router updater should first be
implemented and tested against a no-update manifest before real packages are
published.
