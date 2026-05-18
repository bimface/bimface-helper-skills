# 获取分享列表
```
GET https://api.bimface.com/shares
```

### 说明
显示Appkey下所有的分享列表（包含分页信息）。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|projectId|项目ID|string|
|pageNo|当前页码（默认为第1页，默认值:1）|integer (int32)|
|pageSize|每页显示条数（默认为20条，默认值:20）|integer (int32)|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«PagedList«ShareLinkBean»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|PagedList«ShareLinkBean»|
|page|页码|Page|
|startIndex|起始索引数|int32|
|prePage|上一页码|int32|
|nextPage|下一页码|int32|
|pageNo|当前页码|int32|
|htmlDisplay||string|
|totalPages|页码总数|int32|
|pageSize|每页条目数|int32|
|totalCount|条目总数|int32|
|list|列表|< ShareLinkBean >array|
|sourceId|资源ID|int64|
|password|访问密码|string|
|expireTime|过期时间|string|
|expireTimestamp|过期时间戳，单位为毫秒|int64|
|sourceType|资源类型|string|
|appKey|appKey|string|
|sourceName|资源名称|string|
|projectId|项目ID|int64|
|url|分享链接|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/shares
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
  "data" : {
    "list" : [ {
      "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
      "expireTime" : "2020-11-30",
      "expireTimestamp" : "1606665600",
      "password" : "sdfgth",
      "projectId" : 10000000006016,
      "sourceId" : 1938888813662976,
      "sourceName" : "示例文件",
      "sourceType" : "1",
      "url" : "https://api.bimface.com/preview/e41f2092"
    } ],
    "page" : {
      "htmlDisplay" : "string",
      "nextPage" : 1,
      "pageNo" : 1,
      "pageSize" : 20,
      "prePage" : 1,
      "startIndex" : 0,
      "totalCount" : 9,
      "totalPages" : 1
    }
  },
  "message" : null
}
```
