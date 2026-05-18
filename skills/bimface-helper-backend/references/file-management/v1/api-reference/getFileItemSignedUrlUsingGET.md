# 源文件下载
```
GET https://api.bimface.com/bdfs/data/v1/projects/{projectId}/fileItems/downloadUrl
```

### 说明
通过该接口可获取文件项的当前版本的下载地址，下载地址有效时间为60分钟，可通过参数修改有效期。

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
|fileItemId *|文件项ID(fileItemId和path，必须二选一填入）|string|
|path *|文件项所在路径，使用URL编码（UTF-8），最多256个字符（fileItemId和path，必须二选一填入）|string|
|expireTime|有限期，默认3600s|string（数字型字符串）|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse«string»|
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
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/fileItems/downloadUrl?fileItemId=1938888813662976
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
  "data" : "http://10.5.67.236:9000/bf-dev-srcfile/32588123676c4905a3f0b145555a6af9?response-content-disposition=
  attachment%3Bfilename%3D%22QKYY_DK_ST_20211104.rvt%22%3Bfilename%2A%3Dutf-8%27%27QKYY_DK_ST_20211104.rvt&
  AWSAccessKeyId=bimface&Expires=1648629420&Signature=xjk58U4iy9Pbstb8BS3Q4T9r44Q%3D",
  "message" : "success"
}
```


