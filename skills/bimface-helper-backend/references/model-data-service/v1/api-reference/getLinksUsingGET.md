# 获取模型链接信息
```
GET https://api.bimface.com/data/v2/files/{fileId}/links
```


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|fileId *|文件ID|integer (int64)|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«Link»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< Link >array|
|transform|链接模型位置变换矩阵|string|
|name|链接模型名称|string|
|guid|全局唯一标识符|string|
|id|链接模型id|int64|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/files/1211223382064960/links
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
    "data": [
        {
            "guid": "facbc22f-1f1f-4590-bb74-8b8a516eaa00-00060d16",
            "id": 396566,
            "name": "BIMFACE示例模型.rvt : 1 : 位置 <未共用>",
            "transform": "[1,0,0,0,0,1,0,0,0,0,1,0,0,38781.31,163.58,1]"
        }
    ]
}
```


