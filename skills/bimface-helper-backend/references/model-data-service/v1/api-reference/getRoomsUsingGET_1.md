# 根据楼层ID或构件ID获取对应房间列表
```
GET https://api.bimface.com/data/v2/integrations/{integrateId}/rooms
```

### 说明
当前支持两种方式查询房间列表：1）使用楼层ID查询属于给定楼层的房间列表 2）使用构件ID在空间中计算查询包含该构件的房间列表，这两种方式只能取其一，楼层ID优先。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|integrateId *|集成ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|elementId|构件ID|string|
|fileIdHash|子文件ID|string|
|floorId|楼层ID|string|
|roomToleranceXY|XY方向的误差容许程度 (默认值:"ORDINARY")|enum (STRICT, ORDINARY, LENIENT)|
|roomToleranceZ|Z方向的误差容许程度 (默认值:"STRICT")|enum (STRICT, ORDINARY, LENIENT)|

>说明：roomBoundaryExtensionXY及roomToleranceZ为构件坐标与room坐标的包含关系，有STRICT,ORDINARY,LENIENT三种状态，具体说明可见[构件空间关系计算](https://bimface.com/docs/model-data-service/v1/developers-guide/spatial-relationship.html)。

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«Room»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data||< Room >array|
|area|面积|double|
|boundary|边界坐标|string|
|maxPt|最大坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|levelId|楼层ID|string|
|minPt|最小坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|perimeter|room底部周长|double|
|bboxMin|最小包围盒坐标|Coordinate|
|x||double|
|y||double|
|z||double|
|name|room名称|string|
|bboxMax|最大包围盒坐标|Coordinate|
|x||double|
|y||double|
|z||double|
|id|room的ID|string|
|boundarySegments|附着的构件信息|< string >array|
|properties|属性列表|< PropertyGroup >array|
|items|属性数据|< PropertyItem >array|
|extension|拓展属性|object|
|unit|单位|string|
|code|状态代码|string|
|orderNumber||int32|
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
https://api.bimface.com/data/v2/integrations/1738888866720224/rooms
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
  "data" : [ {
    "area" : 7.25E7,
    "bboxMax" : {
      "x" : -4938.06,
      "y" : -3201.59,
      "z" : 0.0
    },
    "bboxMin" : {
      "x" : -4938.06,
      "y" : -3201.59,
      "z" : 0.0
    },
    "boundary" : "{"version":"2.0","loops":[[[{"z":3499.99,"y":4167.71,"x":2238.78},{"z":3499.99,"y":4167.71,"x":398.78}],[{"z":3499.99,"y":4167.71,"x":398.78},{"z":3499.99,"y":1847.71,"x":398.78}],[{\"z\":3499.99,"y":1847.71,"x":398.78},{"z":3499.99,"y":1847.71,"x":2238.78}],[{"z":3499.99,"y":1847.71,"x":2238.78},{"z":3499.99,"y":4167.71,"x":2238.78}]]]}",
    "boundarySegments" : [
        "260878",
        "386585",
        "386311",
        "300892"],
    "id" : "313137",
    "levelId" : "11",
    "maxPt" : {
      "x" : -4938.06,
      "y" : -3201.59,
      "z" : 0.0
    },
    "minPt" : {
      "x" : -4938.06,
      "y" : -3201.59,
      "z" : 0.0
    },
    "name": "卫生间 10",
    "perimeter": 8320.0,
    "properties": null,
  "message" : ""
}
```
