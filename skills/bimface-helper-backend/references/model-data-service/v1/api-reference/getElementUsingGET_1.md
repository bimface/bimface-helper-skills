# 获取构件属性
```
GET https://api.bimface.com/data/v2/files/{fileId}/elements/{elementId}
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
|elementId *|构件ID|string|
|fileId *|文件ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|includeOverrides|是否查询修改的属性|boolean|

### 响应

| HTTP代码 | 说明         | 类型                      |
| -------- | ------------ | ------------------------- |
| **200**  | OK           | GeneralResponse«Property» |
| **401**  | Unauthorized | -                         |
| **403**  | Forbidden    | -                         |
| **404**  | Not Found    | -                         |


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|Property|
|elementId|构件ID|string|
|boundingBox|包围盒信息|BoundingBox|
|min|最小坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|max|最大坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|name|构件族名称|string|
|guid|全局唯一标识符|string|
|familyGuid|族标识符|string|
|properties|构件属性|< PropertyGroup >array|
|items|属性数据|< PropertyItem >array|
|extension|拓展属性|object|
|unit|单位|string|
|code|状态代码|string|
|orderNumber|样例：0|int32|
|valueType|参数值类型|int32|
|value|参数值|object|
|key|特性|string|
|group|属性分组类型|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/files/1938888813662976/elements/281278
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
  "message": null,
  "data" : {
    "boundingBox" : {
      "max" : {
        "x" : -4938.06,
        "y" : -3201.59,
        "z" : 0.0
      },
      "min" : {
        "x" : -4938.06,
        "y" : -3201.59,
        "z" : 0.0
      }
    },
    "elementId" : "313052",
    "familyGuid" : "",
    "guid" : "79d547c1-5dbf-4e6a-811d-951cf37b29da-0004c6dc",
    "name" : "常规EWALL - 290mm-stone 2",
    "properties" : [ {
      "group" : "基本属性",
      "items" : [ {

                        "key": "specialty",
                        "unit":"",
                        "value": "",
                        "valueType":3
                    },
                    {
                        "key": "无连接高度",
                        "unit": "mm",
                        "value": "10905.300154",
                        "valueType": 2
                    }]
    } ]
  }
}
```


