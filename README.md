# nRF Connect SDK 애플리케이션 구축을 위한 Docker 이미지

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

### Application build

```bash
cd ${Your App Path}
west build -p -b ${Board_Name}
```
