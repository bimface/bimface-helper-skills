# 终止分片上传任务
```
POST https://api.bimface.com/bdfs/v1/data/projects/{project-id}/file-items/multi-part-files/abort
```

### 说明
使用本接口取消分片上传任务，并且删除已上传的Part数据。

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
|parentPath *|父文件夹路径（parentId和parentPath，必须二选一填入）|string|
|parentId *|父文件夹ID（parentId和parentPath，必须二选一填入）|string|
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
https://api.bimface.com/bdfs/v1/data/projects/10000000006016/file-items/multi-part-files/abort
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "id" : "1938888813662976",
  "parentId" : "10000000006016",
  "parentPath" : "/"
}
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "bimfaceservice-0000",
  "data" : "success",
  "message" : null,
}
```


