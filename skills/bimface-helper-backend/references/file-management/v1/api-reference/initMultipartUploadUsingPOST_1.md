
# 创建版本文件分片上传任务

```
POST https://api.bimface.com/bdfs/v1/data/projects/{project-id}/files/multi-part-files
```

### 说明

BIMFACE提供分片上传功能，可以将待上传的版本文件切分为多个碎片（Part）分别上传，完成上传后再调用[合并分片生成版本文件](https://bimface.com/docs/file-management/v1/api-reference/completeMultiPartUploadUsingPOST_1.html)接口，将碎片合并为完整的文件。具体分片上传操作可参考[文档](https://bimface.com/developer-qa/1357)。

**适用场景**：
- 当上传的版本文件较大，大小超过5GB时，需使用分片上传。BIMFACE支持分片上传的单个分片（Part）大小不超过5GB，最后一个Part的大小无限制；
- 当网络环境较差时，建议使用分片上传，可并行上传多个分片以加快上传速度，若上传失败，您仅需重传失败的Part。

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
|length *|文件大小|int64|
|name *|版本文件名称|string|
|fileItemId *|文件项ID（fileItemId和path，必须二选一填入）|string|
|path *|文件项所在路径，使用URL编码（UTF-8），最多256个字符（fileItemId和path，必须二选一填入）|string|
|sourceId|调用方的文件源ID，不能重复|string|

*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«InitMultipartUploadDTO»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|InitMultipartUploadDTO|
|sourceId|调用方的文件源ID|string|
|uploadId|云存储的uploadId, 标识该分片上传事件|string|
|createTime|开始上传的初始化时间|string|
|length|总文件大小|int64|
|name|文件名称|string|
|appKey|appKey|string|
|id|初始化分片任务的ID|string|
|used|标记分片上传的记录是否已经使用|boolean|
|projectId|项目ID|string|
|objectId|对象ID|string|
|message|提示信息|string|

### 消耗

* `application/json`

### 生成

* `application/json`
* `\*/*`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/bdfs/v1/data/projects/10000000006016/files/multi-part-files
```

##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

##### 请求 body
```json
{
  "fileItemId" : "1938888813662976",
  "length" : 100000,
  "name" : "sample.rvt",
  "path" : "/",
  "sourceId" : "aoihjfasjfdalsdfjae"
}
```

### HTTP响应示例

##### 响应 200
```json
{
  "code" : "bimfaceservice-0000",
  "data" : {
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2023-09-27T07:53:28.408Z",
    "id" : "10000042160021",
    "length" : 100000,
    "name" : "sample.rvt",
    "objectId" : "9e8fe6da8c634074a392a21c3d560f86",
    "parentId" : null,
    "parentPath" : null,
    "projectId" : "10000000006016",
    "sourceId" : null,
    "uploadId" : "663CC225BC4845D59341CD06F9A9EA12",
    "used" : false
  },
  "message" : null
}
```
