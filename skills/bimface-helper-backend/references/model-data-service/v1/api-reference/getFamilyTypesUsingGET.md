# 族类型列表
```
GET https://api.bimface.com/data/v2/rfaFiles/{rfaFileId}/familyTypeMetas
```


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|rfaFileId *|文件ID|integer (int64)|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«RfaFamilyType»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< RfaFamilyType >array|
|name|族类型名称|string|
|familyTypeGuid|族类型标识符|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/rfaFiles/1938888813662976/familyTypeMetas
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
    "familyTypeGuid" : "cfd78ac2-7b11-4a72-8ceb-04335916be57",
    "name" : "1200 x 2100mm"
  } ],
  "message" : ""
}
```


