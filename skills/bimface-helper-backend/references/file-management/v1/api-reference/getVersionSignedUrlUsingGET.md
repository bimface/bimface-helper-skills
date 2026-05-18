# 下载指定版本文件
```
GET https://api.bimface.com/bdfs/data/v1/projects/{projectId}/files/{fileId}/downloadUrl
```

### 说明

通过该接口可获取指定版本文件的下载地址，下载地址有效时间为60分钟，可通过参数修改有效期。

### 参数

##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|fileId *|文件ID|string|
|projectId *|项目ID|string|

*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|expireTime|有限期，默认3600s (默认值:3600)|int32|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«string»|
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
https://api.bimface.com/bdfs/data/v1/projects/10000022120160/files/10000022200514/downloadUrl
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
  "data": "https://bf-local-srcfile.oss-cn-shanghai.aliyuncs.com/19f974c24d4b4749be0b7da6a6ae3e37?Expires=1677557417&OSSAccessKeyId=QLYNXu7B9OTjErYR&Signature=QWgNyCDcrhbIxY1rp6F%2BWs7pgxs%3D&response-content-disposition=attachment%3Bfilename%3D%22BIMFACE%25E7%25A4%25BA%25E4%25BE%258B%25E6%2596%2587%25E4%25BB%25B6%252B%2528%25E6%25B5%2581%25E5%25BC%258F%25E5%258A%25A0%25E8%25BD%25BDV3.0%2529.rvt%22%3Bfilename%2A%3Dutf-8%27%27BIMFACE%25E7%25A4%25BA%25E4%25BE%258B%25E6%2596%2587%25E4%25BB%25B6%252B%2528%25E6%25B5%2581%25E5%25BC%258F%25E5%258A%25A0%25E8%25BD%25BDV3.0%2529.rvt"
}
```


