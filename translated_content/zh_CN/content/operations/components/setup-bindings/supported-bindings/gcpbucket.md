---
type: docs
title: "GCP Storage Bucket binding spec"
linkTitle: "GCP Storage Bucket"
description: "Detailed documentation on the GCP Storage Bucket binding component"
---

## 配置

To setup GCP Storage Bucket binding create a component of type `bindings.gcp.bucket`. 请参阅[本指南]({{< ref "howto-bindings.md#1-create-a-binding" >}})，了解如何创建和应用绑定配置。


```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <NAME>
  namespace: <NAMESPACE>
spec:
  type: bindings.gcp.bucket
  version: v1
  metadata:
  - name: bucket
    value: mybucket
  - name: type
    value: service_account
  - name: project_id
    value: project_111
  - name: private_key_id
    value: *************
  - name: client_email
    value: name@domain.com
  - name: client_id
    value: '1111111111111111'
  - name: auth_uri
    value: https://accounts.google.com/o/oauth2/auth
  - name: token_uri
    value: https://oauth2.googleapis.com/token
  - name: auth_provider_x509_cert_url
    value: https://www.googleapis.com/oauth2/v1/certs
  - name: client_x509_cert_url
    value: https://www.googleapis.com/robot/v1/metadata/x509/<project-name>.iam.gserviceaccount.com
  - name: private_key
    value: PRIVATE KEY
```

{{% alert title="Warning" color="warning" %}}
以上示例将密钥明文存储。 更推荐的方式是使用 Secret 组件， [这里]({{< ref component-secrets.md >}})。
{{% /alert %}}

## 元数据字段规范

| 字段                              | 必填 | 绑定支持 | 详情                                                  | 示例                                                                                               |
| ------------------------------- |:--:| ---- | --------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| bucket                          | 是  | 输出   | The bucket name                                     | `"mybucket"`                                                                                     |
| type                            | 是  | 输出   | Tge GCP credentials type                            | `"service_account"`                                                                              |
| project_id                      | 是  | 输出   | GCP project id                                      | `projectId`                                                                                      |
| private_key_id                | 是  | 输出   | GCP private key id                                  | `"privateKeyId"`                                                                                 |
| private_key                     | 是  | 输出   | GCP credentials private key. Replace with x509 cert | `12345-12345`                                                                                    |
| client_email                    | 是  | 输出   | GCP client email                                    | `"client@email.com"`                                                                             |
| client_id                       | 是  | 输出   | GCP client id                                       | `0123456789-0123456789`                                                                          |
| auth_uri                        | 是  | 输出   | Google account OAuth endpoint                       | `https://accounts.google.com/o/oauth2/auth`                                                      |
| token_uri                       | 是  | 输出   | Google account token uri                            | `https://oauth2.googleapis.com/token`                                                            |
| auth_provider_x509_cert_url | 是  | 输出   | GCP credentials cert url                            | `https://www.googleapis.com/oauth2/v1/certs`                                                     |
| client_x509_cert_url          | 是  | 输出   | GCP credentials project x509 cert url               | `https://www.googleapis.com/robot/v1/metadata/x509/<PROJECT_NAME>.iam.gserviceaccount.com` |

## 绑定支持

该组件支持**输出绑定**，其操作如下:

- `create`

### Create file

To perform a create operation, invoke the GCP Storage Bucket binding with a `POST` method and the following JSON body:

> 注意：默认情况下，会随机生成一个UUID。 参见下面所示的支持的元数据设置名称

```json
{
  "operation": "create",
  "data": "YOUR_CONTENT"
}
```

#### 示例


##### Save text to a random generated UUID file

{{< tabs Windows Linux >}}
  {{% codetab %}}
  在Windows上，使用cmd提示符（PowerShell有不同的转义机制）。
  ```bash
  curl -d "{ \"operation\": \"create\", \"data\": \"Hello World\" }" http://localhost:<dapr-port>/v1.0/bindings/<binding-name>
  ```
  {{% /codetab %}}

  {{% codetab %}}
  ```bash
  curl -d '{ "operation": "create", "data": "Hello World" }' \
        http://localhost:<dapr-port>/v1.0/bindings/<binding-name>
  ```
  {{% /codetab %}}

{{< /tabs >}}

##### Save text to a specific file

{{< tabs Windows Linux >}}

  {{% codetab %}}
  ```bash
  curl -d "{ \"operation\": \"create\", \"data\": \"Hello World\", \"metadata\": { \"name\": \"my-test-file.txt\" } }" \
        http://localhost:<dapr-port>/v1.0/bindings/<binding-name>
  ```
  {{% /codetab %}}

  {{% codetab %}}
  ```bash
  curl -d '{ "operation": "create", "data": "Hello World", "metadata": { "name": "my-test-file.txt" } }' \
        http://localhost:<dapr-port>/v1.0/bindings/<binding-name>
  ```
  {{% /codetab %}}

{{< /tabs >}}


##### Upload a file

To upload a file, pass the file contents as the data payload; you may want to encode this in e.g. Base64 for binary content.

然后你就可以像平常一样上传了：

{{< tabs Windows Linux >}}

  {{% codetab %}}
  ```bash
  curl -d "{ \"operation\": \"create\", \"data\": \"(YOUR_FILE_CONTENTS)\", \"metadata\": { \"name\": \"my-test-file.jpg\" } }" http://localhost:<dapr-port>/v1.0/bindings/<binding-name>
  ```
  {{% /codetab %}}

  {{% codetab %}}
  ```bash
  curl -d '{ "operation": "create", "data": "$(cat my-test-file.jpg)", "metadata": { "name": "my-test-file.jpg" } }' \
        http://localhost:<dapr-port>/v1.0/bindings/<binding-name>
  ```
  {{% /codetab %}}

{{< /tabs >}}

## 相关链接

- [Dapr组件的基本格式]({{< ref component-schema >}})
- [绑定构建块]({{< ref bindings >}})
- [如何通过输入绑定触发应用]({{< ref howto-triggers.md >}})
- [如何处理: 使用绑定对接外部资源]({{< ref howto-bindings.md >}})
- [绑定API 参考]({{< ref bindings_api.md >}})