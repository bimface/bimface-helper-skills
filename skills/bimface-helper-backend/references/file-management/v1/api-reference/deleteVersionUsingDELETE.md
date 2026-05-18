# 删除指定版本文件
```
DELETE https://api.bimface.com/bdfs/data/v1/projects/{projectId}/files
```

### 说明

支持批量删除多个版本的文件。

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
|fileIds *|需要删除的版本文件ID|< string > array|

*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«string»|
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
https://api.bimface.com/bdfs/data/v1/projects/10000022120160/files
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
[
    "10000022200514",
    "10000022320357"
]
```


### HTTP响应示例

##### 响应 200
```json
{
    "code": "success",
    "message": null,
    "data": "success"
}
```


