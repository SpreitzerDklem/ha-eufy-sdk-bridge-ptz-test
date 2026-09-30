# Eufy SDK Bridge PTZ Test

This add-on wraps `ghcr.io/spreitzerdklem/ha-eufy-sdk-bridge:ptz-test`, built from
upstream `dev` commit `497eb7d2e77e49e375866b098e73cf6f003ef758`.

Configure the Eufy email, password, and country. The bridge exposes WebSocket control
on port 3000 and RTSP/go2rtc on port 8554, with the session persisted under `/data`.
Home Assistant discovery is registered automatically when the Supervisor API is available.

Stop the official stable and Dev bridges before starting this add-on. They must not run
simultaneously because they can interfere with the same Eufy account/session.

After reconnecting the `eufy-sdk` integration, enable debug logging and press PTZ buttons.
Look for `device.action OK left` (or `right`, `up`, `down`) in the add-on log.
