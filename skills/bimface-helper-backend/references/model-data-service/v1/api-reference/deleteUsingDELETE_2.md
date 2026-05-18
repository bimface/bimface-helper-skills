# 取消分享链接
```
DELETE https://api.bimface.com/share
```

### 说明
若不希望继续分享，可选择(fileId、integrateId、sceneId、clashDetectiveId)其中之一取消对应的分享链接，使之失效。

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

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«string»|
|**204**|No Content|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|string|
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
  "data" : null,
  "message" : null
}
```
