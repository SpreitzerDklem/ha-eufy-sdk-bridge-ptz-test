# Home Assistant Add-on: Eufy SDK Bridge PTZ Test

This temporary add-on uses the custom bridge image:

`ghcr.io/spreitzerdklem/ha-eufy-sdk-bridge:ptz-test`

It is intended to verify the upstream `ha-eufy-sdk-bridge` PR #45 PTZ action fix for
Eufy T8410 cameras. It preserves the upstream add-on options for Eufy credentials,
country, discovery, port 3000 WebSocket control, port 8554 RTSP/go2rtc, and persistent
session data.

Stop the official stable and Dev bridge before starting this add-on. Only one bridge
should use the Eufy account at a time because concurrent sessions can interfere.

After starting, reload the `eufy-sdk` integration, enable bridge debug logging, and
press a PTZ button. Successful logs contain `device.action OK left` rather than
`no action 'left'`.
