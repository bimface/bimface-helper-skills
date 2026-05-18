# 获取Hub Meta信息
```
GET https://api.bimface.com/bdfs/domain/v1/hubs/{hubId}
```
### 说明
通过该接口可获取指定的存储中心（hub）的元信息，用来描述Hub信息，例如名称、描述等。


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|hubId *|HubId|string|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse«hub返回值对象»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|hub返回值对象|
|createTime|hub创建时间|string|
|name|hub名字|string|
|appKey|hub对应的AppKey|string|
|updateTime|hub更新时间|string|
|id|hubId|string|
|tenantCode|hub所属的租户|string|
|info|hub说明|string|
|message|提示信息|string|

### 消耗

* `application/json`


### 生成

* `application/json`
* `*/*`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/bdfs/domain/v1/hubs/10000000000060002
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
  "data" : {
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2022-02-02 02:02:02",
    "id" : "10000000000060002",
    "info" : "BIMFACE的hub",
    "name" : "BIMFACE",
    "tenantCode" : "BIMFACE",
    "updateTime" : "2022-02-02 02:02:02"
  },
  "message" : "success"
}
```


