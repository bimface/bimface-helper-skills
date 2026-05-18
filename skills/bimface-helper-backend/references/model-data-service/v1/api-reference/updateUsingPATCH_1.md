# 更新分享链接
```
PATCH https://api.bimface.com/share
```


### 说明
对已生成的分享链接的有效期、密码进行修改和刷新。该接口为增量更新，在传参时需填入修改的参数，若想保持参数值不变，则无需传入该参数。


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|fileId|文件ID(与集成ID，场景ID，碰撞检测ID 四选一)|integer (int64)|
|integrateId|集成ID(与文件ID，场景ID，碰撞检测ID 四选一)|integer (int64)|
|sceneId|场景ID(与文件ID，集成ID，碰撞检测ID 四选一)|integer (int64)|
|clashDetectiveId|碰撞检测ID(与文件ID，集成ID，场景ID 四选一)|integer (int64)|
|expireDate|有效期限，单位：精确到天，样例"2023-01-22"；
如果设置为永久有效，则需填写值为“permanent"|string|
|needPassword|分享链接是否设置密码，若已设置过密码，填写true则表示刷新密码|boolean|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«ShareLinkBean»|
|**204**|No Content|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|ShareLinkBean|
|sourceId|被分享源ID|int64|
|password|访问密码|string|
|expireTime|过期时间|string|
|expireTimestamp|过期时间戳，单位为毫秒|int64|
|createTime|创建时间|int64|
|sourceType|资源类型|string|
|appKey|appKey|string|
|updateTime|更新时间|int64|
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
https://api.bimface.com/share?fileId=1938888813662976&expireDate=2023-12-28&needPassword=true
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
    "createTime" : "2023-02-28 10:40:47",
    "expireTime" : "2023-02-28 00:00:00",
    "expireTimestamp" : 1677513600000,
    "password" : "abcde",
    "projectId" : 10000000006016,
    "sourceId": 1938888813662976,
    "sourceName" : "示例文件",
    "sourceType" : "1",
    "updateTime" : "2023-02-28 10:55:26",
    "url" : "https://api.bimface.com/preview/e41f2092"
  },
  "message" : "null"
}
```


