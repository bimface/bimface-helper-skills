# 分页获取模型对比结果
```
GET https://api.bimface.com/data/v2/comparisons/{compareId}/diff
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

##### Query
|名称|说明|类型|
|---|---|---|
|elementName|构件名称|string|
|family|族|string|
|page|页码|integer (int32)|
|pageSize|每页记录数|integer (int32)|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«Pagination«ModelCompareDiff»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|Pagination«ModelCompareDiff»|
|total|条目总数|int32|
|data|对比结果数据|< ModelCompareDiff >array|
|elementId|构件ID|string|
|specialty|专业|string|
|followingFileId|对应文件ID|string|
|diffType|差异类型|string|
|id|对比差异构件来源构件ID|string|
|family|族|string|
|categoryName|对比差异构件所属类别名称|string|
|categoryId|对比差异构件所属类别ID|string|
|previousFileId|对比差异构件来源文件ID|string|
|elementName|对比差异构件名称|string|
|page|页码|int32|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/comparisons/2077707858585728/diff
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
    "data" : [ {
      "categoryId" : "-2001320",
      "categoryName" : "framework",
      "diffType" : "CHANGE",
      "elementId" : "296524",
      "elementName" : "250 x 600 mm",
      "family" : "framework 1",
      "followingFileId" : "1136893002033344",
      "id" : "0213154515478",
      "previousFileId" : "1136239003943104",
      "specialty" : "civil"
    } ],
    "page" : 2,
    "total" : 10
  },
  "message" : null
}
```
