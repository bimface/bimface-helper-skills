# 获取Hub列表
```
GET https://api.bimface.com/bdfs/domain/v1/hubs
```
### 说明
通过该接口可查询您的账号已注册哪些存储中心（Hub），您可以将文件上传到已注册的存储空间里。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|dateTimeFrom|开始时间，筛选时间范围内创建的Hub，格式为：yyyy-MM-dd HH:mm:ss|string|
|dateTimeTo|终止时间，筛选时间范围内创建的Hub，格式为：yyyy-MM-dd HH:mm:ss|string|
|info|描述信息|string|
|name|Hub名称|string|
|tenantCode|产品所属的租户（默认值:"BIMFACE"）|string|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse«List«hub返回值对象»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< hub返回值对象 >array|
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
https://api.bimface.com/bdfs/domain/v1/hubs
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
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2022-02-02 02:02:02",
    "id" : "10000000000060002",
    "info" : "BIMFACE的hub",
    "name" : "BIMFACE",
    "tenantCode" : "BIMFACE",
    "updateTime" : "2022-02-02 02:02:02"
  } ],
  "message" : "success"
}
```


