# 生成分享链接
```
POST https://api.bimface.com/share
```

### 说明
文件发起转换、集成后，可以根据fileId或integrateId生成对应文件的分享链接；创建GIS场景、发起碰撞检测后，也可通过GIS场景ID和碰撞检测ID生成分享链接。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|fileId|文件ID(与集成ID，场景ID，碰撞检测ID 四选一)|integer (int64)|
|integrateId|集成模型ID(与文件ID，场景ID，碰撞检测ID 四选一)|integer (int64)|
|sceneId|场景ID(与文件ID，集成ID，碰撞检测ID 四选一)|integer (int64)|
|clashDetectiveId|碰撞检测ID(与文件ID，集成ID，场景ID 四选一)|integer (int64)|
|needPassword|分享链接是否生成访问密码|boolean|
|activeHours|有效时长（与有效期限，二选一），单位：小时，如果不设置表示永久有效|integer (int32)|
|expireDate|有效期限（与有效时长，二选一），单位：精确到天，如果不设置表示永久有效|string|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«ShareLinkBean»|
|**201**|Created|-|
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

### 消耗

* `application/json`

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
    "expireTime" : "2020-11-30 00:00:00",
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
