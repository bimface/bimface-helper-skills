# 获取分类树
```
POST https://api.bimface.com/data/v2/integrations/{integrateId}/tree
```

### 说明
集成模型默认楼层分类树的treeType可选三个值：floor（楼层）, specialty（专业）和customized（自定义）。当treeType为"customized"时, desiredHierarchy表示了筛选树的层次，如：desiredHierarchy=specialty,systemtype。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|integrateId *|集成模型ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|desiredHierarchy|分类树的层次结构|< string > array(multi)|
|treeType|分类树类型 ，默认值:"floor"|string|

##### Body
|名称|说明|类型|
|---|---|---|
|fileIdElementIds|由文件ID及构件ID构成对象组成的列表|< ElementIdWithFileId >array|
|  elementId|构件ID|string|
|  fileId|构件所属的模型ID|string|
|sortedNamesHierarchy|排序名称层级|< array >array|
|sorts|分类树节点排列数组|< TreeNodeSort >array|
|  sortedValues|排序值|< string >array|
|  sortBy|排序方式|string|
|  nodeType|节点类型|string|
|customizedNodeKeys|自定义节点属性|Map< string, string >|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«Tree»|
|**201**|Created|-|
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

### 消耗

* `application/json`

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/integrations/1738888866720224/tree
```

##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

##### 请求 body
```json
{
  "customizedNodeKeys" : {
    "string" : "string"
  },
  "fileIdElementIds" : [ {
    "fileId" : "1938888813662976",
    "elementId" : "313052"
  } ],
  "sortedNamesHierarchy" : [ [ "string" ] ],
  "sorts" : [ {
    "nodeType" : "3",
    "sortBy" : "SORT_BY_NAME",
    "sortedValues" : [ "string" ]
  } ]
}
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
