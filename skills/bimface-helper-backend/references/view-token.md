# 获取 View Token

## 接口地址
```
GET https://api.bimface.com/view/token
```

## 说明
View Token 是对单个模型、集成模型、模型对比、场景的临时访问凭证，只能访问对应的模型数据。获取 View Token 后将其传入前端 JavaScript 组件，即可加载和浏览文件所包含的三维模型或二维图纸。

View Token 有效期为 12 小时，每次使用 View Token 都会重置有效期为 12 小时。

## 请求参数

### Header
| 参数名 | 必填 | 说明 |
|--------|------|------|
| Authorization | 是 | `Bearer {accessToken}` |

### Query（以下 ID 参数选填一项）
| 参数名 | 类型 | 说明 |
|--------|------|------|
| fileId | int64 | 文件转换 ID |
| integrateId | int64 | 集成模型 ID |
| compareId | int64 | 模型对比 ID |
| sceneId | int64 | 场景 ID |
| submodelId | int64 | 子模型 ID |
| clashDetectiveId | int64 | 碰撞检测 ID |
| moduleDataId | int64 | 组件数据 ID |

## 响应参数
```json
{
    "code": "success",
    "data": "your_view_token"
}
```

## 典型用途
- 文件转换完成后，用 `fileId` 获取 View Token，传给前端加载单模型
- 模型集成完成后，用 `integrateId` 获取 View Token，传给前端加载集成模型
- 模型对比完成后，用 `compareId` 获取 View Token，传给前端显示对比结果

## 前端集成

获取到 View Token 后，需要将其传递给前端 SDK 进行模型/图纸加载：

### 3D 模型（参考 [bimface-helper-v3-3d](../bimface-helper-v3-3d/SKILL.md)）
```javascript
// 前端 SDK 初始化时传入 viewToken
let config = new BimfaceSDKLoaderConfig();
config.viewToken = viewToken;  // 后端返回的 viewToken 字符串
BimfaceSDKLoader.load(config, successCallback, failureCallback);
```

### 2D 图纸（参考 [bimface-helper-v3-2d](../bimface-helper-v3-2d/SKILL.md)）
```javascript
// 前端 SDK 初始化时传入 viewToken
let config = new BimfaceSDKLoaderConfig();
config.viewToken = viewToken;  // 后端返回的 viewToken 字符串
BimfaceSDKLoader.load(config, successCallback, failureCallback);
```

> View Token 仅在前端 SDK 初始化时使用，不应被终端用户直接获取或篡改。后端不应将 Access Token 代替 View Token 传给前端。
