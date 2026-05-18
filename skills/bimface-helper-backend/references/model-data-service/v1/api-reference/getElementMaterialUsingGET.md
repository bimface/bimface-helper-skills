# 获取构件材质
```
GET https://api.bimface.com/data/v2/files/{fileId}/elements/{elementId}/materials
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

### 响应

| HTTP代码 | 说明         | 类型                                |
| -------- | ------------ | ----------------------------------- |
| **200**  | OK           | GeneralResponse«List«MaterialInfo»» |
| **401**  | Unauthorized | -                                   |
| **403**  | Forbidden    | -                                   |
| **404**  | Not Found    | -                                   |


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< MaterialInfo >array|
|name|材料名称|string|
|id|构件ID|string|
|parameters|构件材质参数|< PropertyGroup >array|
|items|属性数据|< PropertyItem >array|
|extension|拓展属性|object|
|unit|单位|string|
|code|状态代码|string|
|orderNumber|样例: 0|int32|
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
https://api.bimface.com/data/v2/files/1938888813662976/elements/618987/materials
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
            "id": "268711",
            "name": "金属 - 铝",
            "parameters": [
                {
                    "group": "标识数据",
                    "items": [
                        {
                            "key": "型号",
                            "unit": "",
                            "value": "",
                            "valueType": 3
                        }
                    ]
                }
            ]
        }
    ]
}
```


