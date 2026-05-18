# 依据坐标信息生成拉伸空间
```
POST https://api.bimface.com/feature-management/v1/spaces/new-space/extrusion
```

### 说明

生成拉伸空间。至少需要输入boundary和height信息。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|boundary *|边界条件|EssentialBoundary|
|  outer *|外部边界|< EssentialPoint >array|
|x|样例: "x"|string|
|y|样例: "y"|string|
|z|样例: "z"|string|
|  inner|内部边界|< array >array|
|integrateId *|集成ID（fileId和integrateId选填一项）|int64|
|unit|单位，m或mm|string|
|levelId|空间所属楼层id|string|
|name|空间名称|string|
|description|空间描述|string|
|fileId *|文件ID（fileId和integrateId选填一项）|int64|
|height *|空间拉伸高度|string|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«InternalEssentialSpaceResponse»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "code"|string|
|data|返回数据|InternalEssentialSpaceResponse|
|integrateId|集成文件ID|int64|
|spaceId|空间id|string|
|createTime|创建时间|int64|
|fileId|文件ID|int64|
|message|提示信息|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/feature-management/v1/spaces/new-space/extrusion
```


##### 请求 header
```json
"Authorization: Bearer dc671840-bacc-4dc5-a134-97c1918d664b"
```


##### 请求 body
```json
{
    "fileId": "10000731425271",
    "name": "BIMFACE房间",
    "unit": "m",
    "description": "for test ",
    "height": 4.5,
    "boundary": {
        "outer": [
            {
                "x": 71.5,
                "y": 94.2,
                "z": -4.5
            },
            {
                "x": -0.1,
                "y": 94.2,
                "z": -4.5
            },
            {
                "x": -0.1,
                "y": -0.76,
                "z": -4.5
            },
            {
                "x": 71.5,
                "y": -0.76,
                "z": -4.5
            }
        ],
        "inner": [[            
            {
                "x": 7.15,
                "y": 9.42,
                "z": -0.45
            },
            {
                "x": -0.1,
                "y": 9.42,
                "z": -0.45
            },
            {
                "x": -0.1,
                "y": -0.76,
                "z": -0.45
            },
            {
                "x": 7.15,
                "y": -0.76,
                "z": -0.45
            }]

        ]
    }
}
```


### HTTP响应示例

##### 响应 200
```json
{
    "code": "bimfaceservice-0000",
    "message": null,
    "data": {
        "createTime": "2023-01-05T08:25:06.929Z",
        "fileId": 10000731425271,
        "integrateId": null,
        "spaceId": "b772ba1b9b3242b19796693994b56455"
    }
}
```