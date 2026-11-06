# Privacy Policy - Uptime Kuma for TerraMaster TOS 7

**Application**: Uptime Kuma (`app_id: uptime-kuma`)
**Publisher**: waskevin
**Policy version**: 1.0
**Effective date**: 2026-10-29
**Contact**: https://github.com/waskevin/uptime-kuma/issues

This policy covers the TOS 7 application package distributed by waskevin
(the `uptime-kuma.tar.gz` archive and the `config.ini` / `uptime-kuma.lang` / `uptime-kuma.svg` /
`docker-compose.yml` files inside it) and the container it starts on your
device.  It does not cover the Uptime Kuma project's own website or any service you
connect the application to.

## What the package itself does

- It runs one Docker container from the public image `louislam/uptime-kuma:2.5.5`.
- It does not contain analytics, telemetry, advertising or tracking code.
- It does not send any data to waskevin or to any third party.
- It does not open network connections on its own; the only traffic is what
  the application and your browser make.

## Data the application stores

All application data (database, configuration, attachments) is written to
`/Volume<N>/DockerAppData/uptime-kuma/data/` on your own device.  Nothing is
uploaded anywhere by the packaging.

## Data the application may send

- Only the requests your monitors make (e.g. an HTTP check of a
  URL you added), plus notifications you configure (email, webhook,
  Telegram, ntfy, ...).  Nothing is sent unless you set it up.

## Removing your data

Uninstalling the application from the TOS App Center stops and removes the
container.  Deleting the folder `DockerAppData/uptime-kuma/` on your device removes
all remaining data; nothing is left elsewhere.

## Changes

Any change to this policy is published in this repository together with the
version of the package it applies to.
