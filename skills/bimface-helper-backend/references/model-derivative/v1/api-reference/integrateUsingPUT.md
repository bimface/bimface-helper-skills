# 发起模型集成
```
PUT https://api.bimface.com/v2/integrate
```

### 说明
对于参与集成的文件来说，当单个文件转换成功以后，可以将多个文件集成，生成一个全专业/楼层模型，默认会集成为流式加载V3.0的数据。由于集成不能立即完成，BIMFACE支持在模型集成完成以后，通过[Callback机制](https://bimface.com/docs/model-derivative/v1/developers-guide/callback.html)通知调用方；另外，调用方也可以通过接口查询集成状态。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|sources|待集成的文件列表|< IntegrateSource >array|
|  elevation|标高|int32|
|  specialtySort|专业排序|float|
|  fileName|文件名称|string|
|  specialty|专业|string|
|  transform|模型坐标变换矩阵,平移参数单位必须为毫米|< double >array|
|  floorSort|楼层排序|float|
|  floor|楼层|string|
|  building|建筑|string|
|  fileId|文件ID|int64|
|propertyOverrides|属性重写|< ElementPropertyOverride >array|
|  targetFileIds|目标文件ID|< object >array|
|  valueOverrides|重写属性值|< ElementPropertyValueOverride >array|
|valueToMatch|原有属性值|string|
|valueToOverride|重写属性值|string|
|  keyToMatch|原有特性|string|
|  keyToOverride|重写特性|string|
|internalConfigMap||Map< string, string >|
|parentIntegrateId||int64|
|floorMapping||< FloorMappingItem >array|
|  projectFloorName|项目楼层名称|string|
|  projectFloorId|项目楼层ID|string|
|  fileFloorId|文件楼层ID|string|
|name|模型名称|string|
|callback|回调函数|string|
|config|配置参数|string|
|ruleFileIds|样例: [1232134213412]|< object >array|

>说明：config为发起模型集成的配置，具体参数配置可参考[模型集成](https://bimface.com/docs/model-derivative/v1/developers-guide/file-integrate.html)。

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«FileIntegrateBean»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|FileIntegrateBean|
|integrateId|集成ID|int64|
|sourceId|调用方的文件源ID|string|
|databagId|数据包ID|string|
|reason||string|
|thumbnail|缩略图|< object >array|
|errorCode|错误代码|string|
|priority|优先级|int32|
|type|文件类型|string|
|createTime|创建时间|string|
|name|集成文件名|string|
|appKey|appKey|string|
|projectId|项目ID|int64|
|status|任务状态|string|
|message|提示消息|string|

### 消耗

* `application/json`

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/v2/integrate
```

##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

##### 请求 body
```json
{
  "callback" : "https://api.glodon.com/viewing/callback?authCode=6kj0Jk0affae&signature=2ef131395fb6442eb99abd83d45c6016",
  "config" : "config",
  "floorMapping" : [ {
    "fileFloorId" : "pj1101",
    "projectFloorId" : "pj11",
    "projectFloorName" : "firstfloor"
  } ],
  "floorSort" : [ "5" ],
  "internalConfigMap" : {
    "string" : "string"
  },
  "name" : "model.rvt",
  "parentIntegrateId" : 0,
  "priority" : 2,
  "projectId" : 0,
  "propertyOverrides" : [ {
    "keyToMatch" : "system_type",
    "keyToOverride" : "specialty",
    "targetFileIds" : [ "1468861829161440", "1468862035943904" ],
    "valueOverrides" : [ {
      "valueToMatch" : "water_support_pipe",
      "valueToOverride" : "water_support"
    } ]
  } ],
  "ruleFileIds" : [ 1232134213412 ],
  "sourceId" : "hduf2w3ho21nowr23rqwjrn2o3",
  "sources" : [ {
    "building" : "GlodonBuilding",
    "databagId" : "h2h2312223",
    "elevation" : 0,
    "fileId" : 12311221,
    "fileName" : "model.rvt",
    "floor" : "F01",
    "floorSort" : 0.1,
    "specialty" : "AR",
    "specialtySort" : 0.1,
    "transform" : [ 1.23 ]
  } ],
  "specialtySort" : [ "2" ]
}
```

### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : {
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2017-12-25 17:25:25",
    "databagId" : "c444c81bc1bf4b048dfb759e0dc842a8",
    "errorCode" : "errorCode",
    "integrateId" : 1248789977538784,
    "name" : "integrate-x",
    "priority" : 2,
    "projectId" : 0,
    "reason" : "reason",
    "sourceId" : "123156522123",
    "status" : "success",
    "thumbnail" : [ "https://m.bimface.com/dc6aa5e35b6a269972b005b4b2aac8ce/thumbnail/96.png", "https://m.bimface.com/dc6aa5e35b6a269972b005b4b2aac8ce/thumbnail/256.png" ],
    "type" : "type"
  },
  "message" : null
}
```

另外几种发起模型集成的请求体示例:

1. 带链接关系的rvt模型集成（需要指定integrate-with-links）
```json
{
    "sources": [
        {
            "fileId": 9220020202001
        },
        {
            "fileId": 9220020202002
        }
        ],
    "sourceId":"123456",
    "name":"我的合并模型",
    "callback": "http://www.app.com/receive",
    "config":{
        "integrate-with-links": true
        }
}
```

2. 设置集成模型中单文件的楼层名称、楼层排序、楼层标高（需要指定floor，floorSort，elevation，不适用链接集成的情况）
```json
{
    "sources": [
        {
            "fileId": 1552501392454592,
            "floor":"F01",
            "floorSort":0.5,
            "elevation":4500
        },
        {
            "fileId": 1552501367034816,
            "floor":"F01",
            "floorSort":0.5,
            "elevation":9000
        }
    ],
    "sourceId":"123456",
    "name":"我的合并模型",
    "callback": "http://www.app.com/receive",
    "config":{
    }
}
```

3. 自定义集成模型的构件树（可以在config中组织模型文件的层级关系）
```json
{
    "sources": [
		{
			"fileId": 1552501392454592
		},
		{
			"fileId": 1552501367034816
		}
	],
	"sourceId": "123456",
	"name": "我的合并模型",
	"callback": "http://www.app.com/receive",
	"config": {
		"customizedTree": [
			{
				"name": "一期工程",
				"type": "group",
				"items": [
					{
						"name": "建筑",
						"type": "group",
						"items": [
							{
								"name": "建筑模型.rvt",
								"type": "model",
								"fileId": "1552501392454592"
							}
						]
					},
					{
						"name": "场地",
						"type": "group",
						"items": [
							{
								"name": "场地模型.rvt",
								"type": "model",
								"fileId": "1552501367034816"
							}
						]
					}
				]
			}
		]
	}
}
```

### 失败返回
```json
{
    "code": "authentication.failed",
    "message": "Token was not recognized."
}
```

### 错误码
|code|说明|
|---|---|
|system.error|BIMFACE系统异常|
|authentication.failed|API访问合法性校验失败|
|input.parameter.error|输入的参数有误|
|url.invalid|回调地址不是正确的URL|
|file.not.found|文件不存在|
|file.type.not.support|不支持当前文件发起集成，目前仅支持revit文件|
|file.has.not.translated|文件没有发起转换，不能集成|
|file.is.translating|文件正在转换中，不能集成|
|file.translate.failed|文件转换失败，不能集成|
