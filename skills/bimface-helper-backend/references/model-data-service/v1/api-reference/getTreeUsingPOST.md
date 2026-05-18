# 获取构件分类树
```
POST https://api.bimface.com/data/v2/files/{fileId}/tree
```


### 说明
单模型构件分类树, treeType接受两个值：default和customized,默认为default.  v参数用来区别treeType为default时返回树的格式, customized总是返回格式2.0的构件树.  当treeType为"customized"时：
 
- desiredHierarchy表示了筛选树的层次,可选值有building,systemType,specialty,floor,category,family,familyType，如:desiredHierarchy=specialty,systemtype ；
- customizedNodeKeys: 用来指定筛选树每个维度用id或者是name作为唯一标识, 如"floor":"id"。


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

##### Query
|名称|说明|类型|
|---|---|---|
|treeType|分类树的类型 (默认值:"default")|string|
|v|格式|string|

##### Body
|名称|说明|类型|
|---|---|---|
|customizedNodeKeys|自定义维度筛选标识|Map< string, string >|
|desiredHierarchy|构件树筛选层次|< object >array|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«object»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|object|
|message|提示消息|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/files/1938888813662976/tree
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
  "desiredHierarchy" : [ "category", "family" ]
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
            "categoryId": "-2000011",
            "categoryName": "墙",
            "families": [
                {
                    "family": "基本墙",
                    "familyTypes": [
                        "内部IWALL - 砌块墙 100",
                        "内部墙-290mm",
                        "外部墙01-290mm",
                        "外部墙02-290mm",
                        "常规EWALL - 290mm 2",
                        "常规EWALL - 290mm-stone 2"
                    ]
                },
                {
                    "family": "幕墙",
                    "familyTypes": [
                        "幕墙",
                        "幕墙 EWINDOW"
                    ]
                }
            ]
        }
    ]
}
```


