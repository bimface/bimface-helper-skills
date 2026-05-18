# 批量获取指定文件转换状态
```
POST https://api.bimface.com/v1/project/{projectId}/file-translations
```


### 说明
应用发起转换以后，可以通过该接口批量查询指定文件的转换状态，一次查询的文件数量最多为50。


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|fileIds *|文件ID列表，文件数量不超过50|array|
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
|progressPercent|转换进度|int32|
|thumbnail|缩略图|< object >array|
|createTime|创建时间|string|
|name|文件名称|string|
|errorCode|错误码|string|
|priority|转换优先级|int32|
|projectId|项目ID|int64|
|fileId|文件ID|int64|
|status|转换状态|string|
|config|转换配置项|Map< string, string >|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/v1/project/10000620765434/file-translations
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
        "list": [
            {
                "appKey": "odatvZYUSAWMbdUjTU8HoZXB12345683",
                "cost": 349,
                "createTime": "2021-08-09 17:24:35",
                "databagId": "6f34dd8678c65f6f5da5cee8888888",
                "errorCode": null,
                "fileId": 1938888813662976,
                "length": 6459392,
                "name": "BIMFACE示例模型.rvt",
                "offlineDatabagStatus": "success",
                "outputFormat": "bmd",
                "priority": null,
                "projectId": "10000000006016",
                "realFileType": "rvt",
                "reason": null,
                "progressPercent": 50,
                "retry": true,
                "shareToken": null,
                "shareUrl": null,
                "sourceId": null,
                "status": "processing",
                "supportOfflineDatabag": true,
                "thumbnail": null,
                "type": "rvt-translate"
            }
        ]
    }
}
```


