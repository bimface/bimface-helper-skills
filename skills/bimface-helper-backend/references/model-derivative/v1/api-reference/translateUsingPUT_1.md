# 发起转换
```
PUT https://api.bimface.com/v2/translate
```

### 说明
源文件上传成功后，即可发起对该文件的转换，默认会转换为流式加载V3.0的数据。由于转换不能立即完成，BIMFACE支持在文件转换完成以后，通过[Callback机制](https://bimface.com/docs/model-derivative/v1/developers-guide/callback.html)通知应用；另外，应用也可以通过接口查询转换状态。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|callback|回调url|string|
|source|进行转换的资源|TranslateSource|
|  rootName|主文件名|string|
|  compressed|是否为压缩包资源|boolean|
|  fileId|文件ID|int64|
|config|转换配置项|Map< string, string >|

>说明：config为转换引擎自定义参数，不同的转换引擎支持不同的config格式。具体参数配置可参考[自定义文件转换](https://bimface.com/docs/model-derivative/v1/developers-guide/file-translate.html)。

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«FileTranslateBean»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|FileTranslateBean|
|databagId|databag的ID|string|
|reason|转换失败原因|string|
|thumbnail|缩略图|< object >array|
|createTime|创建时间|string|
|name|文件名称|string|
|errorCode|错误码|string|
|priority|转换优先级|int32|
|projectId|项目ID|int64|
|fileId|文件ID|int64|
|status|转换状态|string|
|message|提示消息|string|

### 消耗

* `application/json`

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/v2/translate
```

##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

##### 请求 body
```json
{
  "callback" : "https://api.glodon.com/viewing/callback?authCode=iklJk0affae&signature=2ef131395fb6442eb99abd83d45c3214",
  "config" : {
    "toBimtiles" : true
  },
  "source" : {
    "compressed" : true,
    "fileId" : 1938888813662976,
    "rootName" : "BIMFACE示例.rvt"
  }
}
```

### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : {
    "createTime" : "2020-12-25 17:23:46",
    "databagId" : "9b711803a43b92d871cde346b63e5075",
    "errorCode" : null,
    "fileId" : 1938888813662976,
    "name" : "BIMFACE示例.rvt",
    "priority" : 2,
    "projectId" : 10000000006016,
    "reason" : null,
    "status" : "success",
    "thumbnail" : [ "https://m.bimface.com/9b711803a43b92d871cde346b63e5019/thumbnail/96.png", "https://m.bimface.com/9b711803a43b92d871cde346b63e5019/thumbnail/256.png" ]
  },
  "message" : null
}
```

另外几种发起转换的请求体示例:

1.DWG文件转换

（1）DWG文件转换成矢量图纸
```json
{
  "source":{
    "fileId":2078231483722272,
    "compressed":false
  },
  "callback":"http://www.app.com/receive",
  "config":null
}
```

（2）DWG文件转换成图片
```json
{
  "source":{
    "fileId":2078231483722272,
    "compressed":false
  },
  "callback":"http://www.app.com/receive",
  "config":{
    "exportDrawing":false
  }
}
```

（3）DWG文件解析轴网信息
```json
{
  "source":{
    "fileId":2078231483722272,
    "compressed":false
  },
  "callback":"http://www.app.com/receive",
  "config":{
    "exportAxisGrids": {
      "gridLines": [
        "A1-WC$0$DOTE",
        "A2-WC$0$DOTE"
      ],
      "gridBubbles": [
        "A1-WC$0$AXIS_TEXT",
        "A2-WC$0$AXIS_TEXT"
      ]
    }
  }
}
```

2.RVT文件转换

（1）RVT文件转换成真实模式的效果
```json
{
  "source":{
    "fileId":1938888813662976,
    "compressed":false
  },
  "callback":"http://www.app.com/receive",
  "config":{
    "texture":true
  }
}
```

3.其它三维模型文件转换

（1）其他三维模型文件包括RVT格式文件，需要转换出引用的外部材质场景、贴图等（上传的文件必须为压缩包，压缩包内同级目录包含模型文件和关联的所有材质文件，转换时必须指定rootName为主文件）
```json
{
  "source":{
    "fileId":1938888813662976,
    "compressed":true,
    "rootName":"BIMFACE示例.rvt"
  },
  "callback":"http://www.app.com/receive",
  "config":{
    "texture":true
  }
}
```
