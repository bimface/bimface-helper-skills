# 转换\集成\对比数据包相关
## 获取数据包大小
```
GET https://api.bimface.com/data/databag/length
```


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|fileId *|文件ID|integer (int64)|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«DatabagInfo»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|DatabagInfo|
|length|数据包大小|int64|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/databag/length?fileId=1938888813662976
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": {
    "length": 9409796
  }
}
```
## 获取缩略图链接
```
GET https://api.bimface.com/data/v2/databag/thumbnail
```


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|fileId *|文件ID|integer (int64)|
|size *|缩略图大小，单位为像素，可选填的参数为96、256、512|integer (int32)|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«string»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/databag/thumbnail?fileId=1938888813662976&size=96
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"	
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": "m.bimface.com/837371783ca711a6d2a8cf091b055603/thumbnail/96.png"
}
```
## 获取数据包资源
```
POST https://api.bimface.com/data/v2/databag/resources
```


### 说明
支持获取数据包中的资源，如描述构件树的文件、图纸对比结果，返回签名URL，有效期为60分钟。


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|fileId|文件ID（ID参数选填一项）|integer (int64)|
|integrateId|集成ID（ID参数选填一项）|integer (int64)|
|compareId|文件对比ID（ID参数选填一项）|integer (int64)|

##### Body
|名称|说明|类型|
|---|---|---|
|resources|资源名称列表，其中"ModelTree"为构件树，"CompareResults"为对比结果。|< string >array|
|paths|资源路径列表。仅支持json和json.gz文件，如图纸的文本信息资源为"resource/drawing/drawingFeatures.json.gz"|< string >array|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«DatabagResourceUrl»»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|< DatabagResourceUrl >array|
|resource|资源名称|string|
|url|下载地址|string|
|message|提示消息|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/databag/resources
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "resources" : [
    "ModelTree"
  ]
}
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": [
    {
      "resource": "ModelTree",
      "url": "https://bf-prod-databag.oss-cn-beijing.aliyuncs.com/837371783ca711a6d2a8cf091b055603/data/tree.json?Expires=1635064156&OSSAccessKeyId=LTAI4FeTn1BWYhirPskmAUWm&Signature=%2BauPVhf8Xm5y8OSMzO2xCfFjJBc%3D"
    }
  ]
}
```


# 获取多视角缩略图链接
```
GET https://api.bimface.com/data/v2/databag/multi-view-thumbnails
```


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|fileId *|文件ID|integer (int64)|
|size *|缩略图大小，目前支持512|integer (int32)|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«List«string»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< string >array|
|message|提示信息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/databag/multi-view-thumbnails?fileId=1349283668249920&size=96
```


##### 请求 header
```json
"Authorization: Bearer dc671840-bacc-4dc5-a134-97c1918d664b"
```


### HTTP响应示例

##### 响应 200
```json
{
    "code" : "bimfaceservice-0000",
    "message": null,
    "data": [         
      "m.bimface.com/4649a226e88baa4b03926453a4d1fb56/thumbnail/512/SouthWestTop.png",
"m.bimface.com/4649a226e88baa4b03926453a4d1fb56/thumbnail/512/SouthTop.png"
    ]
}
```


