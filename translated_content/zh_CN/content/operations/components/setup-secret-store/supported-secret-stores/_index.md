---
type: docs
title: "Supported secret stores"
linkTitle: "Supported secret stores"
weight: 50000
description: Dapr支持对接的密钥仓库
no_list: true
---

表格标题：

> `Status`: [Component certification]({{X29X}}) status
  - [Alpha]({{X18X}})
  - [Beta]({{X20X}})
  - [GA]({{X22X}}) > `Since`: defines from which Dapr Runtime version, the component is in the current status

> `组件版本`：代表组件的版本

### 通用

| 名称                                                                | 状态    | 组件版本 | 自从  |
| ----------------------------------------------------------------- | ----- | ---- | --- |
| [Local environment variables]({{< ref envvar-secret-store.md >}}) | Beta  | v1   | 1.0 |
| [Local file]({{< ref file-secret-store.md >}})                    | Beta  | v1   | 1.0 |
| [HashiCorp Vault]({{< ref hashicorp-vault.md >}})                 | Alpha | v1   | 1.0 |
| [Kubernetes secrets]({{< ref kubernetes-secret-store.md >}})      | GA    | v1   | 1.0 |

### Amazon Web Services (AWS)

| 名称                                                            | 状态    | 组件版本 | 自从  |
| ------------------------------------------------------------- | ----- | ---- | --- |
| [AWS Secrets Manager]({{< ref aws-secret-manager.md >}})      | Alpha | v1   | 1.0 |
| [AWS SSM Parameter Store]({{< ref aws-parameter-store.md >}}) | Alpha | v1   | 1.1 |

### Google Cloud Platform (GCP)

| 名称                                                      | 状态    | 组件版本 | 自从  |
| ------------------------------------------------------- | ----- | ---- | --- |
| [GCP Secret Manager]({{< ref gcp-secret-manager.md >}}) | Alpha | v1   | 1.0 |

### Microsoft Azure

| 名称                                                                                    | 状态    | 组件版本 | 自从  |
| ------------------------------------------------------------------------------------- | ----- | ---- | --- |
| [Azure Key Vault w/ Managed Identity]({{< ref azure-keyvault-managed-identity.md >}}) | Alpha | v1   | 1.0 |
| [Azure Key Vault]({{< ref azure-keyvault.md >}})                                      | GA    | v1   | 1.0 |