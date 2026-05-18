# 获取图纸拆分结果
```
GET https://api.bimface.com/data/v2/files/{fileId}/frames
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
|**200**|OK|GeneralResponse«List«DrawingSplitLayout»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|< DrawingSplitLayout >array|
|frames|图框列表|< DrawingFrame >array|
|boundingBox|包围盒信息|BoundingBox2D|
|min|最小坐标点|Coordinate2D|
|x||double|
|y||double|
|max|最大坐标点|Coordinate2D|
|x||double|
|y||double|
|number|图号|string|
|name|图纸名称|string|
|id|图纸ID|int64|
|name|视图名称|string|
|id|视图ID|int64|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/files/2078231483722272/frames
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
      "frames": [
        {
          "boundingBox": {
            "max": {
              "x": 15835.0,
              "y": -2398.0
            },
            "min": {
              "x": 15239.0,
              "y": -2820.0
            }
          },
          "id": 1,
          "name": "一层平面图",
          "number": "JZ-001"
        }
      ],
      "id": 0,
      "name": "Model"
    }
  ]
}
```