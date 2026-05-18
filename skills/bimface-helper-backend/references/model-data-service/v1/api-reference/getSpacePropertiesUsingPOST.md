# 依据ID查询模型单个空间属性
```
POST https://api.bimface.com/data/v1/feature-management/spaces/{space-id}/properties
```

### 说明

查询单个空间相关信息。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|space-id *|空间id|string|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|integrateId*|集成ID（fileId和integrateId选填一项）|int64|
|fileId*|文件ID（fileId和integrateId选填一项）|int64|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«EssentialSpaceInfoResponse»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "code"|string|
|data|返回数据|< EssentialSpaceInfoResponse >array|
|area|面积|string|
|boundary|边界|EssentialBoundary|
|outer|外部边界|< EssentialPoint >array|
|x|样例: "x"|string|
|y|样例: "y"|string|
|z|样例: "z"|string|
|inner|内部边界|< array >array|
|spaceType|样例: "spaceType"|string|
|description|空间描述|string|
|bboxMax|包围盒|Coordinate|
|x|样例: -4938.068482562385|double|
|y|样例: -3201.59397858169|double|
|z|样例: 0.0|double|
|boundarySegments|包围构件|< string >array|
|spaceVersionId|空间版本id|int64|
|spaceId|空间id|string|
|unit|长度单位（m或mm）|string|
|levelId|空间所在楼层id|string|
|perimeter|空间底面周长|string|
|bboxMin||Coordinate|
|x|样例: -4938.068482562385|double|
|y|样例: -3201.59397858169|double|
|z|样例: 0.0|double|
|name|样例: "name"|string|
|spaceVersion|空间版本|int32|
|geometryType|空间几何类型，目前支持extrusion|string|
|height|空间高度|double|
|message|样例: "message"|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v1/feature-management/spaces/string/properties
```


##### 请求 header
```json
"Authorization: Bearer dc671840-bacc-4dc5-a134-97c1918d664b"
```


##### 请求 body
```json
{
    "fileId": "10000731425271"
}
```


### HTTP响应示例

##### 响应 200
```json
{
    "code": "bimfaceservice-0000",
    "message": null,
    "data": {
        "area": "14.073148872759932",
        "bboxMax": {
            "x": 2.3838,
            "y": -4.2743,
            "z": 5.9384
        },
        "bboxMin": {
            "x": -2.1162,
            "y": -7.6443,
            "z": 3.5
        },
        "boundary": {
            "inner": null,
            "outer": [
                {
                    "x": "-2.1162",
                    "y": "-6.8393",
                    "z": "3.5"
                },
                {
                    "x": "-2.1162",
                    "y": "-7.6443",
                    "z": "3.5"
                },
                {
                    "x": "-1.3712",
                    "y": "-7.6443",
                    "z": "3.5"
                },
                {
                    "x": "1.6388",
                    "y": "-7.6443",
                    "z": "3.5"
                },
                {
                    "x": "2.3838",
                    "y": "-7.6443",
                    "z": "3.5"
                },
                {
                    "x": "2.3838",
                    "y": "-6.8393",
                    "z": "3.5"
                },
                {
                    "x": "1.6388",
                    "y": "-6.8393",
                    "z": "3.5"
                },
                {
                    "x": "1.6388",
                    "y": "-6.5493",
                    "z": "3.5"
                },
                {
                    "x": "2.2388",
                    "y": "-6.5493",
                    "z": "3.5"
                },
                {
                    "x": "2.2388",
                    "y": "-5.5493",
                    "z": "3.5"
                },
                {
                    "x": "2.2388",
                    "y": "-4.2743",
                    "z": "3.5"
                },
                {
                    "x": "-1.9712",
                    "y": "-4.2743",
                    "z": "3.5"
                },
                {
                    "x": "-1.9712",
                    "y": "-5.2493",
                    "z": "3.5"
                },
                {
                    "x": "-1.9712",
                    "y": "-6.5493",
                    "z": "3.5"
                },
                {
                    "x": "-1.3712",
                    "y": "-6.5493",
                    "z": "3.5"
                },
                {
                    "x": "-1.3712",
                    "y": "-6.8393",
                    "z": "3.5"
                },
                {
                    "x": "-2.1162",
                    "y": "-6.8393",
                    "z": "3.5"
                }
            ]
        },
        "boundarySegments": [
            "264056",
            "281803",
            "264129",
            "306006",
            "383434",
            "305845",
            "375325",
            "305935"
        ],
        "description": null,
        "geometryType": null,
        "height": 2.4383999999999997,
        "levelId": "694",
        "name": "健身房 9",
        "perimeter": "18.14",
        "spaceId": "306376",
        "spaceType": "room",
        "spaceVersion": null,
        "spaceVersionId": null,
        "unit": "m"
    }
}
```


