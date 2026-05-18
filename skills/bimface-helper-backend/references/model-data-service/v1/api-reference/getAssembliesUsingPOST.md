# 批量获取部件属性
```
POST https://api.bimface.com/data/v2/files/{fileId}/assemblies
```
### 说明
在请求体里可以传入rvt文件中的部件ID列表来批量获取属性，单次请求的部件数量上限为1000。

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

##### Body<i class="require">*</i>
|名称|说明|类型|
|---|---|---|
|filter|过滤条件|< GroupAndKeysPair >array|
|  keys|属性名称列表|< string >array|
|  group|属性分组类型|string|
|assemblyIds|部件ID列表|< string >array|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«Property»»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< Property >array|
|assemblyId|部件ID|string|
|properties|属性列表|< PropertyGroup >array|
|items|属性数据|< PropertyItem >array|
|unit|单位|string|
|valueType|参数值类型|int32|
|value|参数值|object|
|key|特性|string|
|group|属性分组类型|string|
|message|提示消息|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/files/1938888813662976/assemblies
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "assemblyIds" : [ "264531", "264668" ],
  "filter" : [ {
    "group" : "阶段化",
    "keys":["拆除阶段"]
  }, 
  {
    "group" : "尺寸标注",
    "keys":["体积"]
  }]
}
```


### HTTP响应示例

##### 响应 200
```json
{
    "code": "success",
    "message": null,
    "data": [
        {
            "assemblyId": "264531",
            "properties": [
                {
                    "group": "基本属性",
                    "items": [
                        {
                            "key": "floor",
                            "value": "地坪"
                        }
                    ]
                },
                {
                    "group": "尺寸标注",
                    "items": [
                        {
                            "key": "体积",
                            "unit": "m³",
                            "value": "2.703969",
                            "valueType": 2
                        }
                    ]
                }
            ]
        }
    ]
}
```
