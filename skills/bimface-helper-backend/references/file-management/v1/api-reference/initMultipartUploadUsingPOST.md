# 创建分片上传任务
```
POST https://api.bimface.com/bdfs/v1/data/projects/{project-id}/file-items/multi-part-files
```

### 说明

BIMFACE提供分片上传功能，可以将待上传的文件切分为多个碎片（Part）分别上传，完成上传后再调用[合并分片生成文件](https://bimface.com/docs/file-management/v1/api-reference/completeMultiPartUploadUsingPOST.html)接口，将碎片合并为完整的文件。

**适用场景**：
- 当上传的文件较大，文件大小超过5GB时，建议使用分片上传，BIMFACE支持分片上传的文件大小为0~48.8T，单个分片（Part）大小不超过5GB，最后一个Part的大小无限制；
- 当网络环境较差时，建议使用分片上传，可并行上传多个分片以加快上传速度，若上传失败，您仅需重传失败的Part。

**流程说明**：

1. 将待上传文件按照一定大小进行分片；
2. 使用[创建分片上传任务](https://bimface.com/docs/file-management/v1/api-reference/initMultipartUploadUsingPOST.html)初始化一个分片上传任务，获取文件id；
3. 使用[获取分片上传url](https://bimface.com/docs/file-management/v1/api-reference/getMultipartSignedUrlUsingPOST.html)接口，获取每个Part的上传链接，您需自行使用链接上传每个分片，需保证切片顺序和partNumber一致；
4. 如果希望终止上传任务，可调用[终止分片上传任务](https://bimface.com/docs/file-management/v1/api-reference/abortMultiPartUploadUsingPOST.html)接口，成功上传的Part会一并删除；
5. 当所有Part上传完毕后，使用[合并分片生成文件](https://bimface.com/docs/file-management/v1/api-reference/completeMultiPartUploadUsingPOST.html)接口，将碎片合并为完整的文件。


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
|name *|文件名称|string|
|parentId *|父文件夹ID(parentId和parentPath，必须二选一填入)|string|
|parentPath *|父文件夹路径(parentId和parentPath，必须二选一填入)|string|
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
|parentPath|父文件夹路径|string|
|length|总文件大小|int64|
|name|文件名称|string|
|appKey|appKey|string|
|id|初始化分片的文件ID|string|
|used|标记分片上传的记录是否已经使用|boolean|
|projectId|项目ID|string|
|objectId|对象ID|string|
|parentId|父文件夹ID|string|
|message|提示信息|string|

### 消耗

* `application/json`


### 生成

* `application/json`
* `*/*`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/bdfs/v1/data/projects/10000000006016/file-items/multi-part-files
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "length" : 100000,
  "name" : "sample.rvt",
  "parentId" : "10000000006016",
  "parentPath" : "/",
  "sourceId" : "aoihjfasjfdalsdfjas"
}
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "bimfaceservice-0000",
  "data" : {
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2022-09-27T07:53:28.408Z",
    "id" : "1938888813662976",
    "length" : 100000,
    "name" : "sample.rvt",
    "objectId" : "f0a644103c834b9e9953d38b40c6666",
    "parentId" : "10000000006016",
    "parentPath" : "/",
    "projectId" : "10000000006016",
    "sourceId" : null,
    "uploadId" : "309FAD8B9E124FF4985822478E66666",
    "used" : false
  },
  "message" : null
}
```


