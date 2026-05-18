# 获取多模型楼层信息
```
GET https://api.bimface.com/data/v2/files/{fileIds}/fileIdfloorsMappings
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
|fileIds *|多个模型的文件ID|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|includeArea|是否将楼层中的面积分区ID、名称一起返回|boolean|
|includeRoom|是否将楼层中的房间ID、名称一起返回|boolean|

### 响应

| HTTP代码 | 说明         | 类型            |
| -------- | ------------ | --------------- |
| **200**  | OK           | GeneralResponse |
| **401**  | Unauthorized | -               |
| **403**  | Forbidden    | -               |
| **404**  | Not Found    | -               |


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|object|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/files/1938888813662976,1938888813662977/fileIdfloorsMappings
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
            "fileId":"10000465399387",
            "floors":[
                {
                    "archElev": -450.0,
                    "areas": null,
                    "elevation": -450.0,
                    "height": 450.0,
                    "id": "259664",
                    "miniMap": "m.bimface.com/6a8ca9e2b53c2c5f017db8fe70a9777b/resource/model/maps/259664.png",
                    "name": "地坪",
                    "rooms": [
                {
                    "boundary":{"loops":[[[{"x":-2845.05,"y":5899.48,"z":0.0},{"x":-7745.05,"y":5899.48,"z":0.0}],[{"x":-7745.05,"y":5899.48,"z":0.0},{"x":-7745.05,"y":1399.48,"z":0.0}],[{"x":-7745.05,"y":1399.48,"z":0.0},{"x":-2845.05,"y":1399.48,"z":0.0}],[{"x":-2845.05,"y":1399.48,"z":0.0},{"x":-2845.05,"y":5899.48,"z":0.0}]]],"version":"2.0"} ,
                    "boundarySegments": [
                        "299019",
                        "299441",
                        "299475",
                        "301468",
                        "301578"],
                    "id": "305074",
                    "maxPt": {
                        "x": 108.78,
                        "y": 6067.71,
                        "z": 0.0},
                    "minPt": {
                        "x": -5571.21,
                        "y": 1847.71,
                        "z": 0.0},
                    "name": "餐厅 1"}],
                    "structElev": -450.0}]
                },
        {
            "fileId":"10000465399387",
            "floors":[
                {
                    "archElev": -450.0,
                    "areas": null,
                    "elevation": -450.0,
                    "height": 450.0,
                    "id": "259664",
                    "miniMap": "m.bimface.com/6a8ca9e2b53c2c5f017db8fe70a9777b/resource/model/maps/259664.png",
                    "name": "地坪",
                    "rooms": [{
                    "boundary":{"loops":[[[{"x":-2845.05,"y":5899.48,"z":0.0},{"x":-7745.05,"y":5899.48,"z":0.0}],[{"x":-7745.05,"y":5899.48,"z":0.0},{"x":-7745.05,"y":1399.48,"z":0.0}],[{"x":-7745.05,"y":1399.48,"z":0.0},{"x":-2845.05,"y":1399.48,"z":0.0}],[{"x":-2845.05,"y":1399.48,"z":0.0},{"x":-2845.05,"y":5899.48,"z":0.0}]]],"version":"2.0"} ,
                    "boundarySegments":[
                        "299019",
                        "299441",
                        "299475",
                        "301468",
                        "301578"],
                    "id": "305074",
                    "maxPt": {
                        "x": 108.78,
                        "y": 6067.71,
                        "z": 0.0},
                    "minPt": {
                        "x": -5571.21,
                        "y": 1847.71,
                        "z": 0.0},
                    "name": "餐厅 1"}],
            "structElev": -450.0}
            ]}
        ]
}
```


