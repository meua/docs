---
type: docs
title: "init CLI 命令参考"
linkTitle: "init"
description: "有关 init CLI 命令的详细信息"
---

## 描述

在受支持的托管平台上安装 Dapr 。

## 支持的平台

- [自托管]({{< ref self-hosted >}})
- [Kubernetes]({{< ref kubernetes >}})

## 用法
```bash
dapr init [flags]
```

## 参数

| 名称                   | 环境变量           | 默认值           | 描述                                                 |
| -------------------- | -------------- | ------------- | -------------------------------------------------- |
| `--enable-ha`        |                | `false`       | 启用高可用性 (HA) 方式                                     |
| `--enable-mtls`      |                | `true`        | 在群集中启用 mTLS                                        |
| `--help`, `-h`       |                |               | 显示此帮助消息                                            |
| `--kubernetes`, `-k` |                | `false`       | 将 Dapr 部署到 Kubernetes 集群                           |
| `--wait`             |                | `false`       | Wait for Kubernetes initialization to complete     |
| `--timeout`          |                | `300`         | The wait timeout for the Kubernetes installation   |
| `--namespace`, `-n`  |                | `dapr-system` | 用于安装 Dapr 的 Kubernetes 名称空间                        |
| `--network`          | `DAPR_NETWORK` |               | 部署 Dapr 运行时的 Docker 网络                             |
| `--runtime-version`  |                | `latest`      | 要安装的 Dapr 运行时的版本，例如: `1.0.0`                       |
| `--slim`, `-s`       |                | `false`       | 从 Self-Hosted 安装中排除 Placement 服务、Redis 和 Zipkin 容器 |

## 示例

### 以 Self-Hosted 方式初始化 dapr
```bash
dapr init
```

### 在 Kubernetes 中初始化 dapr
```bash
dapr init -k
```

### Initialize Dapr in Kubernetes and wait for the installation to complete

 You can wait for the installation to complete its deployment with the `--wait` flag.

 The default timeout is 300s (5 min), but can be customized with the `--timeout` flag.
```bash
dapr init -k --wait --timeout 600
```

### 以 Self-Hosted 初始化指定版本的 Dapr 运行时
```bash
dapr init --runtime-version 0.10.0
```

### 以 Kubernetes 初始化指定版本的 Dapr 运行时
```bash
dapr init -k --runtime-version 0.10.0
```

### 以 [精简 Self-Hosted 方式]({{< ref self-hosted-no-docker.md >}})初始化 dapr
```bash
dapr init -s
```