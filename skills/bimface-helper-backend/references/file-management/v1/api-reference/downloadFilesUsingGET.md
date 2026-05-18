# 打包下载压缩文件
```
GET https://api.bimface.com/bdfs/data/v1/projects/{projectId}/fileItems/downloadZip
```
### 说明
支持打包下载多个文件，此时下载的是各个文件项的当前版本。

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
|fileItemIds *|文件项ID，若传多个，使用逗号分隔|< string > array|

*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


### 消耗

* `application/json`


### 生成

* `application/json`
* `*/*`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/fileItems/downloadZip?fileItemIds=1938888813662976,1932727889900118
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
  "message" : "success"
}
```

