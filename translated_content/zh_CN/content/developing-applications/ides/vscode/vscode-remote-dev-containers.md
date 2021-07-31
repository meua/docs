---
type: docs
title: "Developing Dapr applications with remote dev containers"
linkTitle: "Remote dev containers"
weight: 20000
description: "How to setup a remote dev container environment with Dapr"
---

The Visual Studio Code [Remote Containers extension](https://code.visualstudio.com/docs/remote/containers) lets you use a Docker container as a full-featured development environment without installing any additional frameworks or packages to your local filesystem.

Dapr has pre-built Docker remote containers for NodeJS and C#. 您可以选择您的一个选择来选择一个随时制作的环境。 注意这些预制容器自动更新到最新的 Dapr 版本。

### 设置远程开发容器

#### 先决条件
- [Docker Desktop](https://www.docker.com/products/docker-desktop)
- [Visual Studio Code](https://code.visualstudio.com/)
- [VSCode 远程开发扩展包](https://aka.ms/vscode-remote/download/extension)

#### 创建远程 Dapr 容器
1. 在 VS 代码中打开您的应用程序工作区（workspace）
2. In the command command palette (`CTRL+SHIFT+P`) type and select `Remote-Containers: Add Development Container Configuration Files...` <br /><img src="/images/vscode-remotecontainers-addcontainer.png" alt="添加远程容器的截图" width="700" />
3. 输入 `dapr` 来过滤列表到可用的 Dapr 远程容器，并选择符合您应用程序的语言容器。 请注意，您可能需要选择 `Show All Definitions...` <br /><img src="/images/vscode-remotecontainers-daprcontainers.png" alt="添加 dapr 容器的截图" width="700" />
4. Follow the prompts to rebuild your application in container. <br /><img src="/images/vscode-remotecontainers-reopen.png" alt="在开发容器中重新打开应用程序的截图" width="700" />

#### Example
观看有关如何使用应用程序的 Dapr VS 代码远程容器的 [视频](https://www.bilibili.com/video/BV1QK4y1p7fn?p=8&t=120)。 <iframe width="560" height="315" src="//player.bilibili.com/player.html?aid=886064109&bvid=BV1QK4y1p7fn&cid=277945960&page=8&t=120" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen mark="crwd-mark"></iframe>