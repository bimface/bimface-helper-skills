# 创建项目
```
POST https://api.bimface.com/bdfs/domain/v1/hubs/{hubId}/projects
```

### 说明
用Access Token创建项目，对文件分项目进行管理，创建项目之前需要获取应用所属的hubId。


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|hubId *|hubId|string|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|name *|项目名称|string|
|thumbnail|项目缩略图url，若不填则使用默认缩略图|string|
|info|项目描述|string|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse«ProjectDTO»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|ProjectDTO|
|hubId|hubId|string|
|thumbnail|项目缩略图|string|
|createTime|项目创建时间|string|
|name|项目名称|string|
|appKey|appKey|string|
|updateTime|项目更新时间|string|
|id|项目ID(projectId)|string|
|tenantCode|hub所在的租户|string|
|type|项目类型|string|
|info|项目信息|string|
|message|提示信息|string|

### 消耗

* `application/json`


### 生成

* `application/json`
* `*/*`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/bdfs/domain/v1/hubs/10000000000060002/projects
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "info" : "BIMFACE的项目",
  "name" : "BIMFACE的默认项目",
  "thumbnail" : "https://static.bimface.com/bdfs/project/thumbnail/8e21fc91481e42ce805f0db938e05555_200X150.png"
}
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : {
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2022-02-02 02:02:02",
    "hubId" : "10000000000060002",
    "id" : "10000000006016",
    "info" : "BIMFACE的项目",
    "name" : "BIMFACE的默认项目",
    "tenantCode" : "BIMFACE",
    "thumbnail" : "https://static.bimface.com/bdfs/project/thumbnail/8e21fc91481e42ce805f0db938e04958_200X150.png",
    "type" : "NORMAL",
    "updateTime" : "2022-02-02 02:02:02"
  },
  "message" : "success"
}
```


