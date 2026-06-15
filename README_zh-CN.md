# DevContainers

![Docker Image Version](https://img.shields.io/docker/v/snowdreamtech/devcontainers)
![Docker Image Size](https://img.shields.io/docker/image-size/snowdreamtech/devcontainers/latest)
![Docker Pulls](https://img.shields.io/docker/pulls/snowdreamtech/devcontainers)
![Docker Stars](https://img.shields.io/docker/stars/snowdreamtech/devcontainers)

为 DevContainers 打包的 Docker 镜像。(amd64, arm64)

# 使用说明

为了帮助您开始使用此镜像创建容器，您可以使用 docker-compose 或 docker cli。

## Docker Cli

### 简单配置

```bash
docker run -d \
  --name=devcontainers \
  -e TZ=Asia/Shanghai \
  --restart unless-stopped \
  snowdreamtech/devcontainers:latest
```

### 高级配置

```bash
docker run -d \
  --name=devcontainers \
  -e TZ=Asia/Shanghai \
  -v /path/to/data:/path/to/data \
  --restart unless-stopped \
  snowdreamtech/devcontainers:latest
```

## Docker Compose

### 简单配置

```yaml
services:
  base:
    image: snowdreamtech/devcontainers:latest
    container_name: devcontainers
    environment:
      - TZ=Asia/Shanghai
    restart: unless-stopped
```

### 高级配置

```yaml
services:
  base:
    image: snowdreamtech/devcontainers:latest
    container_name: devcontainers
    environment:
      - TZ=Asia/Shanghai
    volumes:
      - /path/to/data:/path/to/data
    restart: unless-stopped
```

# 开发

```bash
docker buildx create --use --name build --node build --driver-opt network=host
docker buildx build -t snowdreamtech/devcontainers --platform=linux/amd64,linux/arm64 . --push
```

## 参考资料

1. [使用 buildx 构建多平台 Docker 镜像](https://icloudnative.io/posts/multiarch-docker-with-buildx/)
1. [如何使用 docker buildx 构建跨平台 Go 镜像](https://waynerv.com/posts/building-multi-architecture-images-with-docker-buildx/#buildx-%E7%9A%84%E8%B7%A8%E5%B9%B3%E5%8F%B0%E6%9E%84%E5%BB%BA%E7%AD%96%E7%95%A5)
1. [Building Multi-Arch Images for Arm and x86 with Docker Desktop](https://www.docker.com/blog/multi-arch-images/)
1. [How to Rapidly Build Multi-Architecture Images with Buildx](https://www.docker.com/blog/how-to-rapidly-build-multi-architecture-images-with-buildx/)
1. [Faster Multi-Platform Builds: Dockerfile Cross-Compilation Guide](https://www.docker.com/blog/faster-multi-platform-builds-dockerfile-cross-compilation-guide/)
1. [docker/buildx](https://github.com/docker/buildx)

## 联系方式（备注：devcontainers）

- Email: <sn0wdr1am@qq.com>
- QQ: 3217680847
- QQ群: 949022145
- WeChat/微信群: sn0wdr1am

## 许可证

MIT
