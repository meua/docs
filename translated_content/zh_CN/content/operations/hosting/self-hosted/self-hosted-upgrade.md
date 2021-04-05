---
type: docs
title: "在自托管环境中升级 Dapr 的步骤"
linkTitle: "升级 Dapr"
weight: 40000
description: "按照这些步骤在自托管模式下升级 Dapr，并确保顺利升级"
---


1. 卸载当前的Dapr部署：

   {{% alert title="Note" color="warning" %}}
   这将删除默认 `$HOME/.dapr` 目录、 二进制文件和所有容器 (dapr_redis、 dapr_placement 和 dapr_zipkin)。 如果docker 命令需要sudo，Linux 用户需要运行 `sudo` 。
   {{% /alert %}}

   ```bash
   dapr uninstall --all
   ```

1. 下载和安装最新的CLI访问 [这个指南]({{< ref install-dapr-cli.md >}})。

1. 初始化 Dapr 运行时：

   ```bash
   dapr init
   ```

1. Ensure you are using the latest version of Dapr (v1.1.0) with:

   ```bash
   $ dapr --version

   CLI version: 1.1.0
   Runtime version: 1.1.0
   ```