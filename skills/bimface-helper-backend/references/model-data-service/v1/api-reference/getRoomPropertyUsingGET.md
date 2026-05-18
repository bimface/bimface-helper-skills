# 获取单个房间信息
```
GET https://api.bimface.com/data/v2/files/{fileId}/rooms/{roomId}
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
|roomId *|房间ID|string|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«Room»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|Room|
|area|面积|double|
|boundary|边界坐标|string|
|maxPt|最大坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|levelId|room对应的楼层ID|string|
|minPt|最小坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|perimeter|房间底面周长|double|
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
https://api.bimface.com/data/v2/files/1938888813662976/rooms/857279
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
        "area": 2.396E7,
        "bboxMax": {
            "x": 108.782,
            "y": 6067.715,
            "z": 2438.399
        },
        "bboxMin": {
            "x": -5571.216,
            "y": 1847.715,
            "z": 0.0
        },
        "boundary": "{"loops":[[[{"x":-2845.05,"y":5899.48,"z":0.0},{"x":-7745.05,"y":5899.48,"z":0.0}],[{"x":-7745.05,"y":5899.48,"z":0.0},{"x":-7745.05,"y":1399.48,"z":0.0}],[{"x":-7745.05,"y":1399.48,"z":0.0},{"x":-2845.05,"y":1399.48,"z":0.0}],[{"x":-2845.05,"y":1399.48,"z":0.0},{"x":-2845.05,"y":5899.48,"z":0.0}]]],"version":"2.0"}",
        "boundarySegments": [
            "299019",
            "299441",
            "299475",
            "301468",
            "301578"
        ],
        "id": "305074",
        "maxPt": {
            "x": 108.782,
            "y": 6067.715,
            "z": 0.0
        },
        "minPt": {
            "x": -5571.216,
            "y": 1847.715,
            "z": 0.0
        },
        "name": "餐厅 1",
        "perimeter": 19799.999,
        "properties": [
            {
                "group": "尺寸标注",
                "items": [
                    {
                        "key": "周长",
                        "unit": "mm",
                        "value": "19800.0",
                        "valueType": 2
                    },
                    {
                        "key": "面积",
                        "unit": "m²",
                        "value": "23.969",
                        "valueType": 2
                    }
                ]
            }
        ]
    }
}
```


