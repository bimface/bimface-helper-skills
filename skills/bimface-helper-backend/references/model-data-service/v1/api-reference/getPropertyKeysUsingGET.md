# 获取模型所有属性字段
```
GET https://api.bimface.com/data/v2/files/{fileId}/propertyKeys
```

### 说明
查询指定模型所包含的所有构件属性字段。


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|fileId *|文件ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|includeOverrides|是否根据修改后的属性值查询，默认为false|boolean|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«PropertyKeysResp»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|PropertyKeysResp|
|properties|构件属性|< GroupedPropertyKey >array|
|items|属性数据|< string >array|
|group|属性分组类型|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/files/1938888813662976/propertyKeys?includeOverrides=false
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
  "data" : {
    "properties" : [ 
        {
            "group": "基本属性",
            "items": [
                "specialty",
                "categoryName",
                "roomId",
                "building",
                "floorId",
                "boundingBox.max.x",
                "systemType",
                "familyGuid",
                "floor",
                "boundingBox.max.y",
                "roomType",
                "boundingBox.max.z",
                "familyType",
                "boundingBox.min.y",
                "boundingBox.min.z",
                "roomName",
                "boundingBox.min.x",
                "buildingId",
                "familyId",
                "familyTypeId",
                "specialtyId",
                "name",
                "guid",
                "systemTypeId",
                "family",
                "categoryId"
            ]
        }
    ]
  },
  "message" : null
}
```


