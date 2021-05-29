---
type: docs
title: "指南：在组件中引用密钥"
linkTitle: "在组件中引用密钥"
weight: 400
description: "如何从组件定义中安全地引用密钥"
---

## 概述

组件可以在组件定义中为 `spec.metadata` 部分引用密钥。

为了引用密钥，您需要设置 `auth.secretStore` 字段以指定密钥存储的名称。

在 Kubernetes 运行时，如果 `auth.secretStore` 为空，则假定使用Kubernetes 密钥存储。

### 支持的密钥存储

跳转到 [此]({{< ref "howto-secrets.md" >}}) 链接来查看Dapr 支持的所有密钥存储，以及如何配置和使用这些存储的信息。

## 引用密钥

虽然您可以选择使用纯文本密钥，但不建议用于生产：

```yml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
  namespace: default
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    value: MyPassword
```

相反，在您应该在密钥存储中创建密钥，并在组件定义中引用它：

```yml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
  namespace: default
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    secretKeyRef:
        name: redis-secret
        key:  redis-password
auth:
  secretStore: <SECRET_STORE_NAME>
```

`SECRET_STORE_NAME` 是已配置的 [密钥存储组件]({{< ref supported-secret-stores >}}) 当在 Kubernetes 中运行并使用 Kubernetes 密钥存储时，字段 `auth.SecretStore` 默认为 `kubernetes` 并且可以留空。

上面的组件定义让Dapr从定义的秘密存储中提取一个名为 `redis-secret` 的密钥，并将密钥的值分配给组件中的 `redis-password` 密钥中的 `redisPassword` 欄位。

## 示例

### 引用一个Kubernetes密钥

下面的示例向您展示如何创建 Kubernetes 密钥来保持 Event Hubs 绑定的连接字符串。

1. 首先，创建Kubernetes密钥：
    ```bash
     kubectl create secret generic eventhubs-secret --from-literal=connectionString=*********
    ```

2. 接下来，在您的绑定中引用该密钥：
    ```yaml
    apiVersion: dapr.io/v1alpha1
    kind: Component
    metadata:
      name: eventhubs
      namespace: default
    spec:
      type: bindings.azure.eventhubs
      version: v1
      metadata:
      - name: connectionString
        secretKeyRef:
          name: eventhubs-secret
          key: connectionString
    ```

3. 最后，将组件应用到 Kubernetes 集群：
    ```bash
    kubectl apply -f ./eventhubs.yaml
    ```

## 访问密钥的范围

Dapr 可以使用其配置限制对密钥存储中的密钥的访问。 请阅读[如何：使用密钥范围]({{< ref "secrets-scopes.md" >}})和[如何：限制可从密钥存储中读取的密钥]({{< ref "secret-scope.md" >}})以了解更多信息。 这是推荐的使用 Dapr 限制访问密钥的方式。

## Kubernetes 权限

### 默认命名空间

当在 Kubernetes 中运行时，Dapr 在安装过程中定义了默认的 Role 和 RoleBinding ，用于在 `default` 命名空间中从 Kubernetes 密钥存储中获取密钥。 对于启用了 Dapr 的应用程序，从`default`命名空间获取密钥，可以在组件中定义和引用一个密钥，如上例所示。

### 非默认命名空间

如果您的 Dapr 启用的应用正在使用从非默认命名空间获取密钥的组件，在该命名空间应用以下资源：

```yaml
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: secret-reader
  namespace: <NAMESPACE>
rules:
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get", "list"]
---

kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: dapr-secret-reader
  namespace: <NAMESPACE>
subjects:
- kind: ServiceAccount
  name: default
roleRef:
  kind: Role
  name: secret-reader
  apiGroup: rbac.authorization.k8s.io
```

这些资源给予了 Dapr 权限，从Kubernetes 密钥商店获取角色和 RoleBinding 定义的命名空间的密钥。

{{% alert title="Note" color="warning" %}}
在生产场景中，仅限Dapr访问某些秘密资源时，您可以使用 `resourceNames` 字段。 请参阅此 [链接](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#referring-to-resources) 获取更多解释。
{{% /alert %}}

## 相关链接

- [使用密钥作用域]({{< ref "secrets-scopes.md" >}})
- [限制可以从密钥仓库中读取的密钥]({{< ref "secret-scope.md" >}})