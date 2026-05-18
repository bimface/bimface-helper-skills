# 批量取消分享链接
```
DELETE https://api.bimface.com/shares
```

### 说明
若不希望继续分享，可以根据sourceId批量取消对应的分享链接，使之失效。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|projectId *|项目ID|string|
|sourceIds|sourceId列表|< integer (int64) > array(multi)|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«BatchDeleteResultBean«long»»|
|**204**|No Content|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|BatchDeleteResultBean«long»|
|nonexistence|不存在的资源列表|< int64 >array|
|deleted|删除列表|< int64 >array|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/shares?sourceIds=1938888813662976&projectId=10000052111111
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
    "deleted" : [ 1938888813662976 ],
    "nonexistence" : []
  },
  "message" : null
}
```
