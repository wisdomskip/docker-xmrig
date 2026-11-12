```markdown
# docker-xmrig
C3Pool 精简版 XMRig，基于 Alpine 构建，镜像体积约 7.44MB。
仅保留 RandomX CPU 算法，不含 GPU 相关代码，适合轻量、低负载、CPU 限流场景。

## 镜像信息
- 基础镜像：alpine:3.21
- 二进制来源：C3Pool xmrig_setup 精简包
- 标签规则：
  - `latest`：最新构建版本
  - `YYYYMMDD`：UTC 打包日期标签（GitHub Actions 自动生成）
- 支持架构：amd64

## 快速测试
```bash
# 查看版本，验证镜像可正常运行
docker run --rm -it wisdomclouds/xmrig:latest -V
```

## 运行示例（限制单核 10% CPU）
```bash
docker run -d --name xmrig-miner \
  --cpus=0.1 \
  wisdomclouds/xmrig:latest \
  -o auto.c3pool.org:19999 \
  -u 41rbVjedxGN8azw3gZsVYTJhgQEY4xvRtBFQ6i3wbdPAY5fGqtMVG9iZ8usf8ema2P5XXZkvLPtGRJf1mRw51SwfQoUJYmh \
  --threads=1 \
  --cpu-max-threads-hint=10
```

## 日志查看
```bash
# 持续跟踪实时日志
docker logs -f xmrig-miner

# 从最后20行开始持续跟踪
docker logs -f --tail 20 xmrig-miner
```

## 容器管理
```bash
# 停止容器
docker stop xmrig-miner

# 删除容器
docker rm xmrig-miner

# 停止并删除（一条命令）
docker stop xmrig-miner && docker rm xmrig-miner
```

## Docker Compose（可选）
新建 `docker-compose.yml`
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
    command:
      - -o
      - auto.c3pool.org:19999
      - -u
      - 41rbVjedxGN8azw3gZsVYTJhgQEY4xvRtBFQ6i3wbdPAY5fGqtMVG9iZ8usf8ema2P5XXZkvLPtGRJf1mRw51SwfQoUJYmh
      - --threads=1
      - --cpu-max-threads-hint=10
```
启动：
```bash
docker compose up -d
```

## 构建说明
- 构建工具：GitHub Actions
- 触发规则：每月1号定时构建 + 手动触发
- 流程：下载 C3Pool xmrig 压缩包 → 多阶段镜像打包 → 推送 Docker Hub，同时打上 `latest` 和日期标签
- 工作流路径：`.github/workflows/build.yml`

## XMRig 常用参数
- `--threads=N`：指定线程数量
- `--cpu-max-threads-hint=XX`：CPU 占用比例提示
- `--quiet`：精简日志输出，减少刷屏

> 说明：`--cpus=0.1` 是 Docker cgroup 硬限制，代表最多使用 0.1 个 CPU 核心。

## 免责声明
请遵守所在地区法律法规，仅在拥有合法使用权的设备上运行。
```
