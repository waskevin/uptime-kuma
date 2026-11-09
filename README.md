# Uptime Kuma for TerraMaster TOS 7

TOS 7 App Center package for [Uptime Kuma](https://uptime.kuma.pet).

一个把 [Uptime Kuma](https://uptime.kuma.pet) 打包上架到 TOS 7 应用中心的 Docker 应用（单容器、单数据卷、SQLite，零外部依赖）。

---

## What it does / 这是什么

Uptime Kuma watches the services you care about and tells you when they
break down - with a friendly dashboard, heartbeats, response-time charts
and notifications to almost any channel.

- Web UI on host port `10074` -> container `3001`
- Monitors HTTP(s), TCP, ping, DNS, keyword, gRPC and push heartbeats
- History and configuration live in one SQLite database on your NAS
- Runs as a non-root user, no privileged mode, no host networking

## Package contents / 包内文件

```
uptime-kuma.tar.gz
├── config.ini            # TOS application metadata
├── uptime-kuma.lang           # 14 languages (zh-cn, zh-hk, en-us, ... pt-pt)
├── uptime-kuma.svg            # application icon (SVG, transparent)
└── docker-compose.yml    # container definition
```

## docker-compose.yml

```yaml
# TOS 7 Docker application - Uptime Kuma
# Rules: Docker Hub image with a LOCKED version tag; non-root user;
#        no privileged mode, no host networking; data persisted via bind mounts.
# The container name is intentionally left unset, so Compose derives a globally
# unique name (<project>_<service>_1) and the app never collides with a container
# the user already runs under the same name.

version: "3.8"

services:
  uptime-kuma:
    image: louislam/uptime-kuma:2.5.5
    restart: unless-stopped
    user: "1000:1000"
    ports:
      - "10074:3001"
    # Host path `/data` is the platform's documented absolute form: TOS maps it
    # to /Volume<N>/DockerAppData/<appid>/data at install time (spec 9.3, rule 4).
    # Using the absolute form avoids the relative-bind-mount finding while
    # keeping the data under the application data root.
    volumes:
      - /data:/app/data
    environment:
      - TZ=Asia/Shanghai
      - UPTIME_KUMA_PORT=3001
    # The image ships its own health check helper and does not contain curl;
    # using the helper keeps the health check from failing forever.
    healthcheck:
      test: ["CMD", "extra/healthcheck"]
      interval: 60s
      timeout: 30s
      retries: 5
      start_period: 180s

x-app-meta:
  web:
    port: 10074
    protocol: http
```

The container runs with `user: "1000:1000"`; the data directory must stay writable by that user.

## Build / 打包

```sh
./build.sh          # writes uptime-kuma.tar.gz + uptime-kuma.tar.gz.sha256
```

## Data location / 数据位置

TOS places the bind mounts under `/Volume<N>/DockerAppData/uptime-kuma/`; the
`data/` folder holds everything that must survive an update or a
reinstall (database, attachments, configuration).

## Upstream / 上游

- Project: https://github.com/louislam/uptime-kuma
- Docker image: https://hub.docker.com/r/louislam/uptime-kuma
- License: MIT (see LICENSE / NOTICE in this repository)

## Notes / 注意

On first start the web UI opens a database setup wizard: choose SQLite (embedded) and confirm. Then create the administrator account - there is no default password. Docker Engine must be installed and enabled from the TOS App Center.

首次启动时网页界面会进入数据库设置向导，请选择 SQLite（内置）并确认，然后自行创建管理员账号，系统没有默认密码。需先在应用中心安装并启用 DockerEngine。
