# ha-eufy-sdk-bridge PTZ test build

This repository is a temporary, independently published build of the upstream
[`ha-eufy-sdk-bridge`](https://github.com/mega-yfue/ha-eufy-sdk-bridge) development code.
It exists to test PTZ controls for the Eufy Indoor Cam Pan & Tilt 2K (`T8410`) in
Home Assistant, where the official stable and Dev add-ons still reported `no action 'left'`.

The source is upstream `dev` at commit `497eb7d2e77e49e375866b098e73cf6f003ef758`.
That commit contains merged PR #45 (`3792b4e596069c1fa637d0a6efb0cf4a557fde00`),
which resolves actions through `dev.ptz?.()` and supports `left`, `right`, `up`, `down`,
and dotted actions such as `preset.goto`.

## Image

The manually runnable GitHub Actions workflow publishes:

```text
ghcr.io/spreitzerdklem/ha-eufy-sdk-bridge:ptz-test
```

It builds `linux/amd64` and `linux/arm64` with Docker Buildx and authenticates with the
repository `GITHUB_TOKEN`. GitHub may initially make the package private. If so, open the
package settings at `github.com/SpreitzerDklem?tab=packages` and change the GHCR package
visibility to **Public**, so Home Assistant can pull it without registry credentials.

## Home Assistant add-on

The wrapper is in
[`homeassistant-addon/eufy_sdk_bridge_ptz_test/`](homeassistant-addon/eufy_sdk_bridge_ptz_test/).
Add this repository as a local/custom add-on repository, or copy that directory into your
Home Assistant add-on repository. Install **Eufy SDK Bridge PTZ Test** and configure the same
Eufy email, password, country, ports, and optional go2rtc/RTSP settings as usual.

The add-on is built locally by Home Assistant from `build.yaml`, which uses the custom
bridge image above as its base. It has the unique slug `eufy_sdk_bridge_ptz_test`,
publishes the bridge on port 3000 and RTSP on 8554, registers `eufy_sdk` discovery, and
persists the Eufy session in `/data`.

**Do not run the official stable/Dev bridge and this test bridge simultaneously.** They use
the same Eufy account/session and can displace or interfere with one another. Stop the other
bridge before starting this one.

## Verification

1. Run **Publish PTZ test image to GHCR** once from the Actions tab.
2. Confirm its `imagetools inspect` step lists `linux/amd64` and `linux/arm64`.
3. Make the GHCR package public if Home Assistant cannot pull it anonymously.
4. Stop the official bridge, install/start the PTZ test add-on, and complete Eufy login/2FA.
5. Reload or reconnect the Home Assistant `eufy-sdk` integration and enable bridge debug logging.
6. Press a PTZ button for the T8410. The add-on log should show:

```text
device.action → left sn=T8410...
device.action OK left ...
```

This should replace the old `no action 'left' on T8410...` / `no action 'right'` error.

For a source-level check:

```bash
git merge-base --is-ancestor 3792b4e596069c1fa637d0a6efb0cf4a557fde00 HEAD
grep -F 'dev.ptz?.()' src/ws-server.mjs
```
