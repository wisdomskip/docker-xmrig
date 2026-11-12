# 构建阶段：只负责下载解压
FROM alpine:3.21 AS builder
RUN apk add --no-cache wget ca-certificates
WORKDIR /build
RUN wget -O xmrig.tar.gz https://github.com/C3Pool/xmrig_setup/blob/master/xmrig.tar.gz?raw=true \
    && tar -zxf xmrig.tar.gz
# 运行阶段 alpine
FROM alpine:3.21
RUN apk add --no-cache \
    gcompat \
    hwloc \
    libssl3 \
    ca-certificates \
    && rm -rf /var/cache/apk/*
WORKDIR /app
COPY --from=builder /build/xmrig ./xmrig
RUN chmod +x ./xmrig

# 新增两个环境变量，可选
ENV WORKER=""
ENV PASS=""

# 改用sh脚本拼接，不改动下载、复制二进制部分
ENTRYPOINT ["/bin/sh","-c"]
CMD ["USER=\"41rbVjedxGN8azw3gZsVYTJhgQEY4xvRtBFQ6i3wbdPAY5fGqtMVG9iZ8usf8ema2P5XXZkvLPtGRJf1mRw51SwfQoUJYmh\"; if [ -n \"$WORKER\" ]; then USER=\"$USER.$WORKER\"; fi; if [ -n \"$PASS\" ]; then USER=\"$USER:$PASS\"; fi; exec ./xmrig -o auto.c3pool.org:19999 -u \"$USER\" \"$@\""]
