# 获取模型对比构件分类树
```
GET https://api.bimface.com/data/v2/comparisons/{compareId}/tree
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
|compareId *|模型对比ID|integer (int64)|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«Tree»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|Tree|
|root|根节点|string|
|items|树节点数据|< TreeNode >array|
|actualName|真实名称|string|
|data|节点数据|object|
|name|节点名称|string|
|elementCount|构件数量|int32|
|id|节点ID|string|
|type|节点类型|string|
|items|该节点下的信息|< TreeNode >array|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/comparisons/2077707858585728/tree
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
    "items" : [ {
      "actualName" : "幕墙竖梃",
      "data" : null,
      "elementCount" : 1,
      "id" : "259456",
      "items" : [ {
        "actualName" : "矩形竖梃",
        "data" : null,
        "elementCount" : 0,
        "id" : "23dd510dbdb44fe088b11b4fa08e95dc",
        "items" : [ ],
        "name" : "矩形竖梃",
        "type" : "type"
      } ],
      "name" : "幕墙竖梃",
      "type" : "category"
    } ],
    "root" : "楼层"
  },
  "message" : null
}
```
