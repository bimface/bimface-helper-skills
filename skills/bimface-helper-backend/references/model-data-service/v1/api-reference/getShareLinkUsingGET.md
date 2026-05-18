# 获取分享链接信息
```
GET https://api.bimface.com/share
```

### 说明
获取分享链接信息，包括有效期、访问密码、访问地址。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|fileId|文件ID(与集成ID，场景ID，碰撞检测ID，token 五选一)|integer (int64)|
|integrateId|集成模型ID(与文件ID，场景ID，碰撞检测ID，token 五选一)|integer (int64)|
|sceneId|场景ID(与文件ID，集成ID，碰撞检测ID，token 五选一)|integer (int64)|
|clashDetectiveId|碰撞检测ID(与文件ID，集成ID，场景ID，token 五选一)|integer (int64)|
|token|分享链接的识别码(与文件ID、集成ID、场景ID，碰撞检测ID 五选一)|string|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«ShareLinkBean»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|ShareLinkBean|
|sourceId|被分享源ID|int64|
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
https://api.bimface.com/share?fileId=1938888813662976
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
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "expireTime" : "2020-11-30",
    "expireTimestamp" : "1606665600",
    "password" : "sdfgth",
    "projectId" : 10000000006016,
    "sourceId" : 1938888813662976,
    "sourceName" : "示例文件",
    "sourceType" : "1",
    "url" : "https://api.bimface.com/preview/e41f2092"
  },
  "message" : null
}
```
