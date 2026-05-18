# 获取图纸列表
```
GET https://api.bimface.com/data/v2/files/{fileId}/drawingsheets
```


### 说明
如果请求参数elementId为null，返回所有图纸，否则返回包含该构件的所有图纸 


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|fileId *|代表该单模型的文件ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|elementId|构件ID|string|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«DrawingSheet»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< DrawingSheet >array|
|viewInfo|视图信息|View|
|elevation|标高|double|
|preview|缩略图信息|Preview|
|path|缩略图对应路径|string|
|width|缩略图尺寸|int32|
|height|缩略图尺寸|int32|
|outline|视图的边界|< object >array|
|viewPoint|视点|ViewPoint|
|origin|屏幕原点|< double >array|
|viewDirection|视图方向|< double >array|
|scale|缩放比例|int32|
|upDirection|方向朝向屏幕的上侧|< double >array|
|rightDirection|方向朝向屏幕的右侧|< double >array|
|levelId|对应楼层ID|string|
|name|楼层名称|string|
|viewType|视图类型|string|
|cropBox|应用于视图的裁剪框|< object >array|
|id|楼层ID|string|
|thumbnails|缩略图|< object >array|
|portsAndViews||< PortAndView >array|
|elevation|标高|double|
|outline|视图的边界|< double >array|
|viewId|样例: "f36e055b996f4290b9bc36d86b82a382"|string|
|viewPoint|视点|ViewPoint|
|origin|屏幕原点|< double >array|
|viewDirection|视图方向|< double >array|
|scale|缩放比例|int32|
|upDirection|方向朝向屏幕的上侧|< double >array|
|rightDirection|方向朝向屏幕的右侧|< double >array|
|viewport|视口|< double >array|
|viewType|视图类型|string|
|fileId|文件ID|int64|
|message|提示信息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/files/1938888813662976/drawingsheets
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
    "fileId" : 0,
    "portsAndViews" : [ {
      "elevation" : 0.0,
      "outline" : [ 0.0 ],
      "viewId" : "9d314aa4ee044691bc0bb4a69b3b04e4",
      "viewPoint" : {
        "origin" : [ 0.0 ],
        "rightDirection" : [ 0.0 ],
        "scale" : 0,
        "upDirection" : [ 0.0 ],
        "viewDirection" : [ 0.0 ]
      },
      "viewType" : "viewType",
      "viewport" : [ 0.0 ]
    } ],
    "viewInfo" : {
      "cropBox" : [ -12147.80, -19279.55, -30480.0, 22637.54, 6805.08, 30480.0 ],
      "elevation" : 0.0,
      "id" : "312",
      "levelId" : "312",
      "name" : "Level 1",
      "outline" : [ -146.52, -215.01, 240.33, 110.78 ],
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
    }
  } ],
  "message" : ""
}
```


