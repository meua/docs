---
type: 文档
title: "MySQL binding spec"
linkTitle: "MySQL"
description: "Detailed documentation on the MySQL binding component"
---

## 设置 Dapr 组件

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <NAME>
  namespace: <NAMESPACE>
spec:
  type: bindings.mysql
  version: v1
  metadata:
    - name: url # Required, define DB connection in DSN format
      value: <CONNECTION_STRING>
    - name: pemPath # Optional
      value: <PEM PATH>
    - name: maxIdleConns
      value: <MAX_IDLE_CONNECTIONS>
    - name: maxOpenConns
      value: <MAX_OPEN_CONNECTIONS>
    - name: connMaxLifetime
      value: <CONNECTILN_MAX_LIFE_TIME>
    - name: connMaxIdleTime
      value: <CONNECTION_MAX_IDLE_TIME>
```

The MySQL binding uses [Go-MySQL-Driver](https://github.com/go-sql-driver/mysql) internally so the `url` parameter should follow the `DSN` format shown below:

- `url`: Required, represent DB connection in Data Source Name (DNS) format.

  **Example DSN**

  ```yaml
  - name: url
    value: user:password@tcp(localhost:3306)/dbname
  ```

If your server requires SSL your connection string must end of `&tls=custom` for example, `"<user>:<password>@tcp(<server>:3306)/<database>?allowNativePasswords=true&tls=custom"`. You must replace the `<PEM PATH>` with a full path to the PEM file. If you are using [MySQL on Azure](http://bit.ly/AzureMySQLSSL) see the Azure [documentation on SSL database connections](http://bit.ly/MySQLSSL), for information on how to download the required certificate. The connection to MySQL will require a minimum TLS version of 1.2.

- `pemPath`: path to the PEM file

also support connection pool configuration variables:

- `maxIdleConns`: integer greater than 0
- `maxOpenConns`: integer greater than 0
- `connMaxLifetime`: duration string
- `connMaxIdleTime`: duration string

{{% alert title="Warning" color="warning" %}} The above example uses secrets as plain strings. 更推荐的方式是使用 Secret 组件， [here]({{< ref component-secrets.md >}}})。 {{% /alert %}}

## 输出绑定支持的操作

- `exec`
- `query`
- `close`

### exec

The `exec` operation can be used for DDL operations (like table creation), as well as `INSERT`, `UPDATE`, `DELETE` operations which return only metadata (e.g. number of affected rows).

**Request**

```json
{
  "operation": "exec",
  "metadata": {
    "sql": "INSERT INTO foo (id, c1, ts) VALUES (1, 'demo', '2020-09-24T11:45:05Z07:00')"
  }
}
```

**Response**

```json
{
  "metadata": {
    "operation": "exec",
    "duration": "294µs",
    "start-time": "2020-09-24T11:13:46.405097Z",
    "end-time": "2020-09-24T11:13:46.414519Z",
    "rows-affected": "1",
    "sql": "INSERT INTO foo (id, c1, ts) VALUES (1, 'demo', '2020-09-24T11:45:05Z07:00')"
  }
}
```

### query

The `query` operation is used for `SELECT` statements, which returns the metadata along with data in a form of an array of row values.

**Request**

```json
{
  "operation": "query",
  "metadata": {
    "sql": "SELECT * FROM foo WHERE id < 3"
  }
}
```

**Response**

```json
{
  "metadata": {
    "operation": "query",
    "duration": "432µs",
    "start-time": "2020-09-24T11:13:46.405097Z",
    "end-time": "2020-09-24T11:13:46.420566Z",
    "sql": "SELECT * FROM foo WHERE id < 3"
  },
  "data": "[
    [0,\"test-0\",\"2020-09-24T04:13:46Z\"],
    [1,\"test-1\",\"2020-09-24T04:13:46Z\"],
    [2,\"test-2\",\"2020-09-24T04:13:46Z\"]
  ]"
}
```

### close

Finally, the `close` operation can be used to explicitly close the DB connection and return it to the pool. This operation doesn't have any response. This operation doesn't have any response.

**Request**

```json
{
  "operation": "close"
}
```

> Note, the MySQL binding itself doesn't prevent SQL injection, like with any database application, validate the input before executing query.

## 相关链接

- [Bindings building block]({{< ref bindings >}})
- [如何通过 input binding 触发应用]({{< ref howto-triggers.md >}})
- [How-To：使用绑定与外部资源进行交互]({{< ref howto-bindings.md >}})
- [绑定API 参考]({{< ref bindings_api.md >}})