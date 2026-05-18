# Callback 回调机制

## 说明
发起模型转换、集成、对比等异步操作时，通过传入 `callback` 参数启用回调机制。BIMFACE 处理完成后以 GET 请求通知回调地址。

## 回调请求参数（GET URL 参数）
| 字段 | 类型 | 描述 |
|------|------|------|
| fileId | Number | 文件 ID（与 integrateId/compareId/sceneId 四选一） |
| integrateId | Number | 集成模型 ID |
| compareId | Number | 对比 ID |
| sceneId | Number | 场景 ID |
| thumbnail | String | 缩略图地址，多个以 "," 分隔 |
| status | String | 处理状态：`success`（完成），`failed`（失败） |
| reason | String | 失败原因 |
| nonce | String | 回调随机数 |
| signature | String | 签名，用于验证回调来源 |

## 签名验证
收到回调后需验证签名以确认消息来自 BIMFACE：

```
signature = MD5(appKey + ":" + appSecret + ":" + compareId + ":" + status + ":" + nonce)
```

其中 `compareId` 对应回调参数中的模型标识（fileId / integrateId / compareId / sceneId 中的实际返回字段）。

## 回执要求
应用收到回调后必须返回 HTTP STATUS 200 作为回执。

## 示例
BIMFACE 发送的回调请求：
```
GET https://my.app.com/callback?fileId=1938888813662976&status=success&thumbnail=...&signature=99a6fccb1894dfdb4cce48fd5ec58110&nonce=123abc
```
