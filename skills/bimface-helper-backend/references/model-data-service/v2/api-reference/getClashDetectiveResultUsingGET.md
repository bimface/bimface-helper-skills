# 获取碰撞检测结果
```
GET https://api.bimface.com/data/v2/clash-detective/{clash-detective-id}/result
```

### 说明
支持获取碰撞检测结果的json文件，返回签名URL，有效期为60分钟。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|clashDetectiveId *|碰撞检测ID|integer (int64)|
*为必填项

### 响应

| HTTP代码 | 说明         | 类型                                  |
| -------- | ------------ | ------------------------------------- |
| **200**  | OK           | GeneralResponse«ClashDetectiveResult» |
| **401**  | Unauthorized | -                                     |
| **403**  | Forbidden    | -                                     |
| **404**  | Not Found    | -                                     |


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回URL链接|string|
|clashType|碰撞类型，"Hard"为硬碰撞，"Clearance"为间隙碰撞|string|
|tolerance|碰撞公差|double|
|selectionA|选择集A|ClashDetectiveSource|
|fileId|文件ID|int64|
|integrateId|集成ID|int64|
|objectData|构件筛选条件|< Map«string,string» >array|
|objectIds|构件ID的数组|< string >array|
|selectionB|选择集B|ClashDetectiveSource|
|fileId|文件ID|int64|
|integrateId|集成ID|int64|
|objectData|构件筛选条件|< Map«string,string» >array|
|objectIds|构件ID的数组|< string >array|
|results|碰撞结果|< ClashDetectiveResultItem >array|
|clashId|碰撞ID|string|
|objectIdA|选择集A的构件ID|string|
|objectIdB|选择集B的构件ID|string|
|position|碰撞位置|Coordinate|
|x||double|
|y||double|
|z||double|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/clash-detective/2265157542875776/result
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "bimfaceservice-0000",
  "message": null,
  "data":  "https://bf-prod-databag.oss-cn-beijing.aliyuncs.com/837371783ca711a6d2a8cf091b055603/data/clashdetectice.json?Expires=1635064156&OSSAccessKeyId=LTAI4FeTn1BWYhirPskmAUWm&Signature=%2BauPVhf8Xm5y8OSMzO2AAAAAAAAD"
}
```


