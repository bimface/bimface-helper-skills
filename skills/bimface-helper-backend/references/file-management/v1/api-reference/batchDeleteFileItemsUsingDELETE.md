# 批量删除文件
```
DELETE https://api.bimface.com/bdfs/data/v1/projects/{projectId}/fileItems
```
### 说明
通过接口传入多个文件项ID，即可批量删除文件项（包括文件项下所有版本）。

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
|fileItemIds *|需要删除的文件项ID|< string > array|

*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse|
|**204**|No Content|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|object|
|message|提示信息|string|

### 消耗

* `application/json`


### 生成

* `application/json`
* `*/*`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/fileItems
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
[
  "1938888813662976",
  "1932727889900118",
  "1938888893874857"
]
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "message" : "success"
}
```


