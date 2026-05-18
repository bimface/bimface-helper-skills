# 删除文件夹
```
DELETE https://api.bimface.com/bdfs/data/v1/projects/{projectId}/folders
```

### 说明
删除指定文件夹，文件夹内所有文件（包括文件项下的所有版本文件）也被删除。

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
|folderId *|文件夹ID（folderId和path，必须二选一填入）|string|
|path *|文件夹路径,使用URL编码（UTF-8），最多256个字符（folderId和path，必须二选一填入）|string|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse«string»|
|**204**|No Content|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|


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
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/folders?folderId=1938888813662976&path=%2FBDFS
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


