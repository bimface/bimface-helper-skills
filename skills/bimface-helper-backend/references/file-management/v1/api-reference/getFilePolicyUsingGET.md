# 获取文件直传的policy凭证
```
GET https://api.bimface.com/bdfs/data/v1/projects/{projectId}/fileItems/policy
```

### 说明
BIMFACE使用了分布式对象存储来存储用户上传的模型、图纸文件。

通过文件直传接口，开发者在申请到一个Policy凭证后可以直接上传文件到BIMFACE后台的分布式存储系统。

相比普通文件流上传，这样上传速度和稳定性都会有提升，因此推荐开发者采用这种上传方式。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|projectId *|项目ID|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|name *|文件名称，若包含字符&，需要使用URL编码（UTF-8）两次。|string|
|parentId *|父文件夹ID（parentId和parentPath，必须二选一填入）|string|
|parentPath *|父文件夹路径，使用URL编码（UTF-8），最多256个字符（parentId和parentPath，必须二选一填入）|string|
|sourceId|调用方的文件源ID，不能重复|string|
|maxLength|文件流的长度|int64|
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
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/fileItems/policy?name=sample.rvt&parentId=10000000006016
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
    "accessId" : "QLYNXu7B9OTjErYR",
    "callbackBody" : "eyJjYWxsYmFja0JvZHlUeXBlIjoiYXBwbGljYXRpb24veC13d3ctZm9ybS11cmxlbmNvZGVkIiwiY2FsbGJhY2tIb3N0IjoiZmls
    ZS5iaW1mYWNlLmNvbSIsImNhbGxiYWNrVXJsIjoiaHR0cHM6Ly8xMTYuMjI4LjE5NS4xOC9vc3MvcmVjZWl2ZSIsImNhbGxiYWNrQm9keSI6Im9iamVjdD
    0ke29iamVjdH0mc2l6ZT0ke3NpemV9JmV0YWc9JHtldGFnfSZuYW1lPXRlc3QucGRmJmZpbGVJZD0xNDgzMDY1NTc0NzU0NTI4JmFwcGtleT1hRGxQZjEz
    VXRpR3M3eXVIQ2Q4ZUhTTEhiSEpUVThTZCZzb3VyY2VJZD0mZmlsZUJ1Y2tldD1iZi1kZXYtc3JjZmlsZSJ9",
    "expire" : 1542792319,
    "host" : "https://bf-dev-srcfile.oss-cn-shanghai.aliyuncs.com",
    "objectKey" : "2f15df1c430b4ad3b0644029111b703a",
    "policy" : "eyJleHBpcmF0aW9uIjoiMjAxOC0xMS0yMVQwOToyNToxOS45OTZaIiwiY29uZGl0aW9ucyI6W1siY29udGVudC1sZW5ndGgtcmFuZ2Ui
    LDAsNTM2ODcwOTEyMF0sWyJzdGFydHMtd2l0aCIsIiRrZXkiLCIiXV19",
    "signature" : "q4NrZ1By/msuHOHlgpgX56mMUhY="
  },
  "message" : "success"
}
```


