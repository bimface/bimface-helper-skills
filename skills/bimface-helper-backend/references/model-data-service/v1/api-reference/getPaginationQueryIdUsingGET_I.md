# 生成分页查询的ContextId
```
GET https://api.bimface.com/data/v2/query/paginationContextId
```


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|bearer {accessToken}|string|
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
|data|样例: "data"|string|
|message|样例: ""|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/query/paginationContextId
```


##### 请求 header
```json
"Authorization: Bearer dc671840-bacc-4dc5-a134-97c1918d664b"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : "data",
  "message" : ""
}
```


