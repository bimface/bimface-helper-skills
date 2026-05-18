# 获取模型链接信息
```
GET https://api.bimface.com/data/v2/integrations/{integrateId}/linkGraph
```

### 说明
获取集成模型中单模型之间的链接关系，该方法仅对链接集成模型生效，返回的链接图为有向无环图。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|integrateId *|集成模型ID|integer (int64)|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«LinkGraphNode»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< LinkGraphNode >array|
|databagId|databag的ID|string|
|name|名称|string|
|linkTransform|转换矩阵|string|
|links|链接关系|< LinkGraphNode >array|
|params|参与集成的附带信息|< object >array|
|linkPathHash|链接映射ID|string|
|linkName|链接名称|string|
|fileId|文件ID|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/integrations/1738888866720224/linkGraph
```

##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : [ {
    "databagId" : "6f34dd8678c65f6f5da5cee04e964718",
    "fileId" : "1938888813662976",
    "linkName" : "y.rvt: 7 : loc <not shared>",
    "linkPathHash" : "1938888813662976",
    "linkTransform" : "",
    "links" : [ {
      "databagId" : "a2b670bf1e8fd6471b92d90f16b170ad",
      "fileId" : "1890578004335936",
      "linkName" : "y.rvt: 7 : loc <not shared>",
      "linkPathHash" : "1890578004335936",
      "linkTransform" : "",
      "links" : [],
      "name" : "x.rvt",
      "params" : []
    } ],
    "name" : "x.rvt",
    "params" : []
  } ],
  "message" : null
}
```
