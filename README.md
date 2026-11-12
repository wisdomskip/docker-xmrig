```markdown
# docker-xmrig
C3Pool 精简版 XMRig，基于 Alpine 构建，镜像体积约 7.44MB。
仅包含 RandomX 算法，无 CUDA、OpenCL 支持，适合低负载轻量 CPU 挖矿。

## 镜像信息
- 基础镜像：alpine:3.21
- 二进制来源：C3Pool xmrig_setup
- 标签规则：
  - `latest`：最新构建版
  - `YYYYMMDD`：按 UTC 日期打标签（GitHub Actions 自动构建）
- 支持架构：amd64

## 快速启动
### 基础示例（不带矿机名、密码）
```bash
docker run -d --name xmrig-miner \
  --cpus=0.1 \
  wisdomclouds/xmrig:latest \
  --threads=1 \
  --cpu-max-threads-hint=10
```

### 带矿机名 + 密码（推荐，方便矿池区分机器）
```bash
docker run -d --name xmrig-miner \
  --cpus=0.1 \
  -e WORKER=debian01 \
  -e PASS=123456 \
  wisdomclouds/xmrig:latest \
  --threads=1 \
  --cpu-max-threads-hint=10
```
> 环境变量说明：
> - `WORKER`：矿机名称（选填）
> - `PASS`：矿池密码（选填）
> 拼接规则：`钱包地址.矿机名:密码`，留空则不追加

## 查看日志
```bash
docker logs -f xmrig-miner
```

## Docker Compose（可选）
```yaml
services:
  xmrig:
    image: wisdomclouds/xmrig:latest
    container_name: xmrig-miner
    restart: unless-stopped
    deploy:
      resources:
        limits:
          cpus: '0.1'
    environment:
      WORKER: "debian01"
      PASS: "123456"
    command:
      - --threads=1
      - --cpu-max-threads-hint=10
```
启动：
```bash
docker compose up -d
```

## 构建说明
- 构建方式：GitHub Actions 自动构建
- 触发规则：每月1号 UTC 0点自动构建 / 手动触发
- 同步：每次构建成功自动把本 README 同步到 Docker Hub 仓库介绍页

## XMRig 常用参数
- `--threads=N`：挖矿线程数
- `--cpu-max-threads-hint=N`：CPU占用上限百分比
- `--no-color`：关闭日志彩色输出

> 说明：`--cpus=0.1` 是 Docker cgroup 硬限制，代表最多使用 0.1 核 CPU。

## 免责声明
请遵守所在地区法律法规，仅在拥有合法使用权的设备上运行。
```
