# 获取三维视点或二维视图列表
```
GET https://api.bimface.com/data/v2/files/{fileId}/views
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
|**200**|OK|GeneralResponse«List«View»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< View >array|
|elevation|标高|double|
|preview||Preview|
|path|缩略图对应路径|string|
|width|缩略图尺寸|int32|
|height|缩略图尺寸|int32|
|outline|外框线|< object >array|
|viewPoint|视点信息|ViewPoint|
|origin|原始视点|< double >array|
|viewDirection|观察方向|< double >array|
|scale|缩放比例|int32|
|upDirection|样例:[0.0,1.0,0.0]|< double >array|
|rightDirection|样例:[0.0,1.0,0.0]|< double >array|
|levelId|对应楼层ID|string|
|name|楼层名称|string|
|viewType|样例: "FloorPlain"|string|
|cropBox|包围盒平面尺寸|< object >array|
|id|楼层ID|string|
|thumbnails|缩略图|< object >array|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/files/1938888813662976/views
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
    "cropBox" : [ -12147.80, -19279.55, -30480.0, 22637.54, 6805.08, 30480.0 ],
    "elevation" : 0.0,
    "id" : "312",
    "levelId" : "312",
    "name" : "Level 1",
    "outline" : [ -146.52, -215.015, 240.33, 110.78 ],
    "preview" : {
      "height" : 0,
      "path" : "path",
      "width" : 0
    },
    "thumbnails" : [ "m.bimface.com/9b711803a43b92d871cde346b63e5019/resource/thumbnails/312/312.96x96.png" ],
    "viewPoint" : {
      "origin" : [ 0.0,0.0,0.0 ],
      "rightDirection" : [ 0.0,1.0,0.0 ],
      "scale" : 0,
      "upDirection" : [ 0.0,0.0,1.0 ],
      "viewDirection" : [ 0.0,0.0,1.0 ]
    },
    "viewType" : "FloorPlain"
  } ],
  "message" : ""
}
```


