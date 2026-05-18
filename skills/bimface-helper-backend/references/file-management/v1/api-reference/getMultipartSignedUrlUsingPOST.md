# 获取分片上传url
```
POST https://api.bimface.com/bdfs/v1/data/projects/{project-id}/file-items/multi-part-files/signed-url
```

### 说明
初始化一个分片上传任务后，调用本接口指定分片顺序，并通过获取的URL上传分片（Part）数据。
如果使用同一个partNumber上传了新的数据，则partNumber对应的Part数据将被覆盖。


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

##### Body
|名称|说明|类型|
|---|---|---|
|id *|初始化分片上传时返回的文件ID|string|
|partNum *|指定获取第几片的签名url, 最终合并流时是按照partNum的顺序，所以需自行保证partNum的合理性|number|
|type|外链类型，可选值PUBLIC, PRIVATA, INTERNAL 默认PUBLIC|string|
|expiredTime|获取到的外链url有效期,默认3600s|number|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«string»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|string|
|message|提示信息|string|

### 消耗

* `application/json`


### 生成

* `application/json`
* `*/*`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/bdfs/v1/data/projects/10000000006016/file-items/multi-part-files/signed-url
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "expiredTime" : 3600,
  "id" : "1938888813662976",
  "partNum" : 1,
  "type" : "PUBLIC"
}
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "bimfaceservice-0000",
  "data" : "https://bf-dev-srcfile-cityha.oss-cn-shanghai.aliyuncs.com/6632fab61ede47199069b4ceb3ecae53?Expires=1664268999&OSSAccessKeyId=QLYNXu7B9OTjErYR&Signature=Kkv3acXo%2B6a9407nj3afgi8XaM8%3D&uploadId=309FAD8B9E124888888888888F&partNumber=1",
  "message" : null
}
```


