# 生成分页查询的ContextId
```
GET https://api.bimface.com/data/v2/query/paginationContextId
```

### 说明
生成分页查询的上下文 ID，用于 DSL 查询结果的分页场景。DSL 语法详见 [dsl-query.md](../../../dsl-query.md)。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
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
https://api.bimface.com/data/v2/query/paginationContextId
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
  "data": "229286ca-193c-4a95-9603-3ff579117b4k"
}
```


