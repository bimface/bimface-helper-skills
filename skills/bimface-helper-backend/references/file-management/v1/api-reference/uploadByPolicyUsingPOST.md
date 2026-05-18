# 根据policy凭证在web端上传文件

### 说明
通过接口[获取文件直传的policy凭证](https://bimface.com/docs/file-management/v1/api-reference/getFilePolicyUsingGET.html)或[获取新版本直传的policy凭证](https://bimface.com/docs/file-management/v1/api-reference/getFilePolicyUsingGET_1.html)后，可以直接在前端使用表单上传方式将文件上传到BIMFACE的对象存储上。

BIMFACE 控制台就是通过这种方式来实现文件上传的，可以F12→network查看请求详情。

以获取到的policy凭证为以下数据示例：

~~~json
{
    "code": "success",
    "message": null,
    "data": {
        "host": "https://bf-prod-srcfile.oss-cn-beijing.aliyuncs.com",
        "policy": "eyJleHBpcmF0aW9uIjoiMjAxOS0wOC0xNVQwNjowOTozMC41NTFaIiwiY29uZGl0aW9ucyI6W1siY29udGVudC1sZW5ndGgtcmFuZ2UiLDAsMTA3Mzc0MTgyNF0seyJzdWNjZXNzX2FjdGlvbl9zdGF0dXMiOiIyMDAifSxbInN0YXJ0cy13aXRoIiwiJGtleSIsIiJdXX0=",
        "accessId": "5nGlEwOIzrwCVaDZ",
        "signature": "QGMtcEzm2KrVg7cr266CVMc9syM=",
        "expire": 1565849370,
        "callbackBody": "eyJjYWxsYmFja1VybCI6Imh0dHBzOi8vZmlsZS5iaW1mYWNlLmNvbS9vc3MvcmVjZWl2ZSIsImNhbGxiYWNrSG9zdCI6ImZpbGUuYmltZmFjZS5jb20iLCJjYWxsYmFja0JvZHkiOiJvYmplY3RcdTAwM2Qke29iamVjdH1cdTAwMjZzaXplXHUwMDNkJHtzaXplfVx1MDAyNmV0YWdcdTAwM2Qke2V0YWd9XHUwMDI2bmFtZVx1MDAzZHVwbG9hZFRlc3RfMjAxOTA1MTYucnZ0XHUwMDI2ZmlsZUlkXHUwMDNkMTY3MTk0ODkzMjkwODQ0OFx1MDAyNmFwcGtleVx1MDAzZHJPNzdrQXd1V3BXajZTMlRMVzdmSDJSS1NOWWNBcmxFXHUwMDI2c291cmNlSWRcdTAwM2RcdTAwMjZmaWxlQnVja2V0XHUwMDNkYmYtcHJvZC1zcmNmaWxlIiwiY2FsbGJhY2tCb2R5VHlwZSI6ImFwcGxpY2F0aW9uL3gtd3d3LWZvcm0tdXJsZW5jb2RlZCJ9",
        "objectKey": "af20e3d3a0e44377b2260129a5e90402",
        "sourceId": null
    }
}
~~~

通过表单方式上传时，请求的构造方式为：

|名称|对应值|
|---|---|
|请求url|返回体中的host|
|name|请求体中的name|
|key|返回体中的objectKey|
|policy|返回体中的policy|
|OSSAccessKeyId|返回体中的accessId|
|callback|返回体中的callbackBody|
|signature|返回体中的signature|
|success_action_status|200|
|file|需要上传文件的binary数据流|

### 请求示例

![](https://static.bimface.com/attach/4d1d03e7c9d140a2b5c27d7a39323dbf_web_policy_request.png)

```json
POST / HTTP/1.1
Host: bf-prod-srcfile.oss-cn-beijing.aliyuncs.com
User-Agent: PostmanRuntime/7.15.2
Accept: */*
Postman-Token: 0c31cbde-ee57-4528-be60-7468bddf49b1,d7fb57d1-9df2-439a-bfe2-c6081365c21f
Host: bf-prod-srcfile.oss-cn-beijing.aliyuncs.com
Accept-Encoding: gzip, deflate
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Length: 5125934
Connection: keep-alive

Content-Disposition: form-data; name="name"
uploadTest_20190516.rvt
------WebKitFormBoundary7MA4YWxkTrZu0gW--

Content-Disposition: form-data; name="key"
af20e3d3a0e44377b2260129a5e90402
------WebKitFormBoundary7MA4YWxkTrZu0gW--

Content-Disposition: form-data; name="policy"
eyJleHBpcmF0aW9uIjoiMjAxOS0wOC0xNVQwNjowOTozMC41NTFaIiwiY29uZGl0aW9ucyI6W1siY29udGVudC1sZW5ndGgtcmFuZ2UiLDAsMTA3Mzc0MTgyNF0seyJzdWNjZXNzX2FjdGlvbl9zdGF0dXMiOiIyMDAifSxbInN0YXJ0cy13aXRoIiwiJGtleSIsIiJdXX0=
------WebKitFormBoundary7MA4YWxkTrZu0gW--

Content-Disposition: form-data; name="OSSAccessKeyId"
5nGlEwOIzrwCVaDZ
------WebKitFormBoundary7MA4YWxkTrZu0gW--

Content-Disposition: form-data; name="success_action_status"
200
------WebKitFormBoundary7MA4YWxkTrZu0gW--

Content-Disposition: form-data; name="callback"
eyJjYWxsYmFja1VybCI6Imh0dHBzOi8vZmlsZS5iaW1mYWNlLmNvbS9vc3MvcmVjZWl2ZSIsImNhbGxiYWNrSG9zdCI6ImZpbGUuYmltZmFjZS5jb20iLCJjYWxsYmFja0JvZHkiOiJvYmplY3RcdTAwM2Qke29iamVjdH1cdTAwMjZzaXplXHUwMDNkJHtzaXplfVx1MDAyNmV0YWdcdTAwM2Qke2V0YWd9XHUwMDI2bmFtZVx1MDAzZHVwbG9hZFRlc3RfMjAxOTA1MTYucnZ0XHUwMDI2ZmlsZUlkXHUwMDNkMTY3MTk0ODkzMjkwODQ0OFx1MDAyNmFwcGtleVx1MDAzZHJPNzdrQXd1V3BXajZTMlRMVzdmSDJSS1NOWWNBcmxFXHUwMDI2c291cmNlSWRcdTAwM2RcdTAwMjZmaWxlQnVja2V0XHUwMDNkYmYtcHJvZC1zcmNmaWxlIiwiY2FsbGJhY2tCb2R5VHlwZSI6ImFwcGxpY2F0aW9uL3gtd3d3LWZvcm0tdXJsZW5jb2RlZCJ9
------WebKitFormBoundary7MA4YWxkTrZu0gW--

Content-Disposition: form-data; name="signature"
QGMtcEzm2KrVg7cr266CVMc9syM=
------WebKitFormBoundary7MA4YWxkTrZu0gW--

Content-Disposition: form-data; name="file";
filename="/C:/Users/jianqq/Downloads/b (1).rvt
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

### HTTP响应示例

##### 响应 200

```json
{
    "code": "success",
    "message": null,
    "data": {
        "fileId": 1671948932908448,
        "name": "uploadTest_20190516.rvt",
        "status": "success",
        "etag": "85BECD325859F9F715F9FE9E4C3FBD04",
        "suffix": "rvt",
        "length": 5124105,
        "createTime": "2019-08-15 14:06:21"
    }
}
```


