# 获取新版本直传的policy凭证
```
GET https://api.bimface.com/bdfs/data/v1/projects/{projectId}/files/policy
```

### 说明

BIMFACE使用了分布式对象存储来存储用户上传的模型、图纸文件。

通过新版本直传接口，开发者在申请到一个Policy凭证后可以直接上传新版本到BIMFACE后台的分布式存储系统。

通过该接口获取到新版本直传的policy凭证后，可以直接在前端使用表单上传方式将文件上传到BIMFACE的对象存储上，具体见[根据policy凭证在web端上传文件](https://bimface.com/docs/file-management/v1/api-reference/uploadByPolicyUsingPOST.html)。

### 参数

##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|<span class="name">projectId *|项目ID|string|

*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|fileItemId *|文件项ID（fileItemId和Path，必须二选一填入）|string|
|path *|文件项所在路径（fileItemId和Path，必须二选一填入）|string|
|name|文件名称|string|
|maxLength|文件流的长度|int64|
|sourceId|调用方的文件源ID，不能重复|string|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse«UploadPolicyResponse»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|UploadPolicyResponse|
|accessId|用户请求的accessId|string|
|signature|对变量policy签名后的字符串|string|
|objectKey|对象键|string|
|expire|过期时间|int64|
|host|用户发送上传请求的域名|string|
|callbackBody|上传请求时发送给应用服务器的内容|string|
|policy|上传Policy|string|
|message|提示信息|string|

### 消耗

* `application/json`


### 生成

* `application/json`
* `*/*`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/files/policy?name=sample.rvt&fileItemId=10000022320003
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
  "data": {
    "host": "https://bf-dev-srcfile-cityha.oss-cn-shanghai.aliyuncs.com",
    "policy": "eyJleHBpcmF0aW9uIjoiMjAyMy0wMy0xNlQwMzowNDo0Mi45OTVaIiwiY29uZGl0aW9ucyI6W1siY29udGVudC1sZW5ndGgtcmFuZ2UiLDAsMTA3Mzc0MTgyNF0seyJzdWNjZXNzX2FjdGlvbl9zdGF0dXMiOiIyMDAifSxbInN0YXJ0cy13aXRoIiwiJGtleSIsIiJdXX0=",
    "accessId": "QLYNXu7B9OTjErYR",
    "signature": "mf/uEFxQVGwbEHP1RCd2O5z3TdA=",
    "expire": 1678935882995,
    "callbackBody": "eyJjYWxsYmFja1VybCI6Imh0dHBzOi8vYXBpLXRlc3QuYmltZmFjZS5jb20vYmRmcy9vc3MvcG9saWN5L2NhbGxiYWNrIiwiY2FsbGJhY2tIb3N0IjoiYXBpLXRlc3QuYmltZmFjZS5jb20iLCJjYWxsYmFja0JvZHkiOiJvYmplY3RcdTAwM2Qke29iamVjdH1cdTAwMjZzaXplXHUwMDNkJHtzaXplfVx1MDAyNmV0YWdcdTAwM2Qke2V0YWd9XHUwMDI2bmFtZVx1MDAzZOeSkOeSkOeahOacjemlsDIyMi5ydnRcdTAwMjZhcHBLZXlcdTAwM2QzejVLYXUyT3pWWDk0ak80ekxrdUd3RWpCeUNWNTQxTFx1MDAyNnR5cGVcdTAwM2RGSUxFXHUwMDI2cHJvamVjdElkXHUwMDNkMTAwMDAwMTY4NTAwNTNcdTAwMjZmaWxlSXRlbUlkXHUwMDNkMTAwMDAwMjkyODA4MzBcdTAwMjZvc3NCdWNrZXRcdTAwM2RiZi1kZXYtc3JjZmlsZS1jaXR5aGEiLCJjYWxsYmFja0JvZHlUeXBlIjoiYXBwbGljYXRpb24veC13d3ctZm9ybS11cmxlbmNvZGVkIn0=",
    "objectKey": "82610c86f1e040ecba96b1bfa30fb83f"
    },
  "message": "success"
}
```


