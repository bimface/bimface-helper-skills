# 获取MEP系统信息
```
GET https://api.bimface.com/data/v2/files/{fileId}/MEPSystem
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
|fileId *|模型ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|systemCategory|希望获取的系统类别|string|
|systemType|希望获取的系统类型|string|

### 响应

| HTTP代码 | 说明         | 类型                             |
| -------- | ------------ | -------------------------------- |
| **200**  | OK           | GeneralResponse«List«MEPSystem»» |
| **401**  | Unauthorized | -                                |
| **403**  | Forbidden    | -                                |
| **404**  | Not Found    | -                                |


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< MEPSystem >array|
|baseEquipment|MEP系统设备|string|
|name|系统名称|string|
|systemType|MEP系统类型|string|
|systemCategory|MEP系统类别|string|
|id|系统ID|string|
|terminals|MEP系统连接构件列表|< string >array|
|network|MEP系统连接|< NetworkNode >array|
|id|构件ID|string|
|type|构件类型|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/files/1938888813662976/MEPSystem
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
  "message": null,
  "data" : [{
      "baseEquipment": "",
      "id": "901594",
      "name": "SF 239",
       "network": [
           "id": "901647",
            "type": "风管"
       ],
     "systemCategory": "风管系统",
     "systemType": "SupplyAir",
     "terminals":["903649"]
  }]
}
```


