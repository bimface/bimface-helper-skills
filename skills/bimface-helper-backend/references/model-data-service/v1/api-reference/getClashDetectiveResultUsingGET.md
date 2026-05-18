# 获取碰撞检测结果
```
GET https://api.bimface.com/data/clashDetective/{clashDetectiveId}/result
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
|code|样例: "success"|string|
|data|返回数据|ClashDetectiveResult|
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
https://api.bimface.com/data/clashDetective/2265157542875776/result
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": {
    "clashType": "Hard",
    "tolerance": null,
    "selectionA": {
      "fileId": 1938888813662976,
      "objectData": [{
        "categoryId": "-2001330"
      }],
      "objectIds": ["272828"]
    },
    "selectionB": {
      "fileId": 1938888813662976,
      "objectData": [{
        "family": "混凝土-矩形梁-C30"
      }]
    },
    "results": [{
      "clashId": "0",
      "objectIdA": "272828",
      "objectIdB": "295610",
      "position": {
        "x": -4938.068482562385,
        "y": -3201.59397858169,
        "z": 0.0
      }
    }]
  }
}
```


