# nRF Connect SDK 애플리케이션 구축을 위한 Docker 이미지

arm64 Host 개발환경 기준 Docker.

> MacOS 환경에서 Docker Container가 Host의 디바이스로 접근이 불가능.
> Flash는 Host환경에서 해야함

# Host PC Dependency

- Docker
- [nrf-command-line-tools](https://www.nordicsemi.com/Products/Development-tools/nrf-command-line-tools/download)

# Docker

## Dockerfiles
- Dockerfile.ncs : ncs(nordic connect sdk) 개발 환경
- Dockerfile.ide : neovim IDE

## Quick Start

fix volumes:${Your Work Directory Path} in docker-compose.yml


### Build images

```bash
sh build.sh
```

### Run container

```bash
sh run.sh
```

### Application Build (Container)

```bash
cd ${Your App Path}
west build -p -b ${Board_Name}
```

### Application Flash (Host)

```bash
sh flash.sh
```
