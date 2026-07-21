# Zhongheng browser collaboration deployment

This deployment runs the Zhongheng-branded Neko client with Google Chrome on
`192.168.1.11` and publishes only the customer-facing traffic through FRP on
`8.134.166.10`.

## Public ports

| Port | Protocol | Purpose |
| --- | --- | --- |
| 10083 | TCP | Neko web UI and WebSocket |
| 10081 | UDP | WebRTC media (UDP mux) |
| 10082 | TCP | WebRTC fallback (TCP mux) |

CDP is bound to `127.0.0.1:9223` on the Neko host and is never published by
FRP.

## Browser proxy

Google Chrome uses `http://192.168.1.8:10810` by default. Loopback and the
`192.168.1.*` LAN remain direct so internal services do not depend on the
proxy. Override `CHROME_PROXY_SERVER` or `CHROME_PROXY_BYPASS_LIST` in the
host-side `.env` file when the proxy endpoint changes.

## Browser Harness

Create an SSH tunnel from the Codex workstation:

```bash
ssh -N -L 19223:127.0.0.1:9223 lamarck@192.168.1.11
```

Then connect Browser Harness to `http://127.0.0.1:19223`.

## Operations

The deployment directory on the host is `~/services/neko-collab`.

Build the branded client before synchronizing this directory to the host:

```bash
npm --prefix client ci
npm --prefix client run build
rsync -a --delete client/dist/ deploy/collab/client-dist/
```

```bash
docker compose ps
docker compose logs -f --tail=100
docker compose up -d --build
sudo systemctl reload neko-collab
```

Runtime passwords are stored only in the host-side `.env` file with mode
`0600`; they are not part of this repository.

The host installs `neko-collab.service` as a system-level unit. Docker's
`restart: unless-stopped` policy handles container crashes, while systemd runs
Compose at boot and provides an explicit service lifecycle. Power, lid and
sleep settings are intentionally outside this deployment.
