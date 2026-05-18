# 获取 Access Token

## 接口地址
```
POST https://api.bimface.com/oauth2/token
```

## 说明
所有 BIMFACE 服务端 API 调用前必须先获取 Access Token。Token 有效期为 7 天，有效期内不会改变。

## 请求参数

### Header
| 参数名 | 必填 | 说明 |
|--------|------|------|
| Authorization | 是 | 格式：`Basic {base64编码串}`，其中 base64 编码串 = Base64(`{appkey}:{appsecret}`) |

## 响应参数
```json
{
    "code": "success",
    "data": {
        "token": "your_access_token",
        "expireTime": "2026-02-09 11:00:11"
    }
}
```

## 调用后续 API
获取 Token 后，在所有业务接口请求头中添加：
```
Authorization: Bearer {accessToken}
```

## 最佳实践
- `appKey` 和 `appSecret` **严禁** 硬编码在源代码中或提交到版本控制，必须通过环境变量注入
- 全局缓存 accessToken，避免每次请求都重新获取
- 检测到 401 响应时自动刷新 Token
