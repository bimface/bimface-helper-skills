# 获取转换状态
```
GET https://api.bimface.com/translate
```


### 说明
应用发起转换以后，可以通过该接口查询转换状态。


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|fileId *|文件ID|integer (int64)|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«FileTranslateBean»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|FileTranslateBean|
|databagId|databag的ID|string|
|reason|转换失败原因|string|
|thumbnail|缩略图|< object >array|
|createTime|创建时间|string|
|name|文件名称|string|
|errorCode|错误码|string|
|priority|转换优先级|int32|
|projectId|项目ID|int64|
|fileId|文件ID|int64|
|status|转换状态|string|
|progressPercent|转换进度|int32|
|config|转换配置项|Map< string, string >|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/translate?fileId=1938888813662976
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
    "data": {
        "appKey": "s5nbdcsxZzDYKysjYasjIkL0JAAAAA",
        "compressed": false,
        "createTime": "2021-09-07 14:38:12",
        "databagId": "b32076a584e6f606462855c945e03875",
        "config":{
            "toBimtiles" : true,
            "texture":true
        },
        "errorCode": null,
        "fileId": 10000682106551,
        "name": "主文件.rvt",
        "outputFormat": "bimtiles",
        "priority": 1,
        "projectId": "10000620765434",
        "reason": null,
        "status": "success",
        "progressPercent": 100,
        "thumbnail": [
            "https://m.bimface.com/b32076a584e6f606462855c945e03875/thumbnail/96.png",
            "https://m.bimface.com/b32076a584e6f606462855c945e03875/thumbnail/256.png",
            "https://m.bimface.com/b32076a584e6f606462855c945e03875/thumbnail/512.png"
        ]
    }
}
```


