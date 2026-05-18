# 获取视图信息
```
GET https://api.bimface.com/data/v2/integrations/{integrateId}/fileViews
```

### 说明
根据viewType筛选结果集，viewType可选7个值：FloorPlan（楼层俯视二维视图），ThreeD（三维视图），CeilingPlan（天花板仰视二维视图），Elevation（轴侧二维视图），EngineeringPlan（结构平面视图），Rendering（渲染视图），DrawingSheet（图纸视图）。当不指定viewType值，则返回所有集合。

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
|viewType|视图类型|string|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«FileViews»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< FileViews >array|
|views|视图列表|< View >array|
|elevation|标高|double|
|preview|缩略图信息|Preview|
|path|缩略图对应路径|string|
|width|缩略图宽度|int32|
|height|缩略图高度|int32|
|outline|外框线|< object >array|
|viewPoint|视点|ViewPoint|
|origin|原始视点|< double >array|
|viewDirection|视点方向|< double >array|
|scale|缩放比例|int32|
|upDirection||< double >array|
|rightDirection||< double >array|
|levelId|对应楼层ID|string|
|name|楼层名称|string|
|viewType|视图类型|string|
|cropBox|包围盒平面尺寸|< object >array|
|id|楼层ID|string|
|thumbnails|缩略图|< object >array|
|fileId|文件ID|int64|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/integrations/1738888866720224/fileViews
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
    "fileId" : 1938888813662976,
    "views" : [ {
      "cropBox" : [ -12147.804809235151, -19279.554054815613, -30480.0, 22637.545576143948, 6805.089759789783, 30480.0 ],
      "elevation" : 0.0,
      "id" : "312",
      "levelId" : "312",
      "name" : "Level 1",
      "outline" : [ -146.52900292249365, -215.01048476685295, 240.3331231070219, 110.78415780710446 ],
      "preview" : {
        "height" : 0,
        "path" : "path",
        "width" : 0
      },
      "thumbnails" : [ "m.bimface.com/9b711803a43b92d871cde346b63e5019/resource/thumbnails/312/312.96x96.png" ],
      "viewPoint" : {
        "origin" : [ 0.0 ],
        "rightDirection" : [ 0.0 ],
        "scale" : 0,
        "upDirection" : [ 0.0 ],
        "viewDirection" : [ 0.0 ]
      },
      "viewType" : "FloorPlain"
    } ]
  } ],
  "message" : null
}
```
