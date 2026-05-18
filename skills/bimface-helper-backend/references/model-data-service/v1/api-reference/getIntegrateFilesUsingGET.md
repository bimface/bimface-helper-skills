# 获取参与集成的子文件列表
```
GET https://api.bimface.com/data/v2/integrations/{integrateId}/files
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
|integrateId *|集成模型ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|includeDrawingSheet|是否将文件下转换出的图纸数量一起返回|boolean|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«IntegrateFileData»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< IntegrateFileData >array|
|integrateId|集成模型ID|int64|
|specialtySort|集成时对该子文件设置的专业排序权重|float|
|databagId|databag的ID|string|
|fileName|子文件名称|string|
|specialty|集成时对该子文件设置的专业|string|
|floorSort|集成时对该子文件设置的楼层排序权重|float|
|linkedBy|链接映射关系列表|< string >array|
|floor|集成时对该子文件设置的楼层|string|
|fileType|子文件类型|string|
|drawingSheetCount|文件下转换后的图纸数量|int32|
|fileId|子文件ID|int64|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/integrations/1738888866720224/files
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
      "databagId": "6f34dd8678c65f6f5da5cee04e964718",
      "drawingSheetCount": null,
      "fileId": 1938888813662976,
      "fileName": "BIMFACE示例.rvt",
      "fileType": "rvt",
      "floor": null,
      "floorSort": null,
      "integrateId": 1738888866720224,
      "linkedBy": [
        "1938888813662976"
      ],
      "specialty": null,
      "specialtySort": null
    },
    {
      "databagId": "d4649ee227e345c8b7f0022342247dec",
      "drawingSheetCount": null,
      "fileId": 1656504297012502,
      "fileName": "sample.rvt",
      "fileType": "rvt",
      "floor": null,
      "floorSort": null,
      "integrateId": 1738888866720224,
      "linkedBy": [
       "1656504297012502"
      ],
      "specialty": null,
      "specialtySort": null
    }
  ]
}
```
