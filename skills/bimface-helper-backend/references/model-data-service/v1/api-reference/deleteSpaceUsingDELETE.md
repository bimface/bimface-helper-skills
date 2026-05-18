# 删除指定空间
```
DELETE https://api.bimface.com/feature-management/v1/spaces/{space-id}
```

### 说明

删除已生成的指定空间。

### 参数

##### Header

|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|space-id *|空间id|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|fileId *|文件ID（fileId和integrateId选填一项）|integer (int64)|
|integrateId *|集成ID（fileId和integrateId选填一项）|integer (int64)|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«Map«string,object»»|
|**204**|No Content|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "code"|string|
|data|返回数据|object|
|message|提示信息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/feature-management/v1/spaces/string
```


##### 请求 header
```json
"Authorization: Bearer dc671840-bacc-4dc5-a134-97c1918d664b"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "code",
  "data" : "object",
  "message" : "message"
}
```


