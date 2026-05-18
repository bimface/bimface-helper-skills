---
name: "bimface-helper-backend"
description: "BIMFACE v3 服务端 RESTful API 调用，涵盖文件管理、模型数据查询、模型转换/集成/对比等后端接口。当用户需要调用 BIMFACE 后端 API、开发后端服务、处理模型数据、上传下载文件、管理模型转换流程时调用。"
version: "1.0.0"
---

# BIMFACE 后端服务开发技能

## 使用须知
- 仅支持 BIMFACE v3 服务端 RESTful API
- **必须先获取 Access Token** 才能调用任何接口
- `references/` 下每个 API 文档即对应一个接口，编写代码时**必须读取对应文档**
- 异步操作（转换/集成/对比）建议使用 callback 获取结果
- 仅实现用户明确要求的功能

## 需求解析（编码前强制执行）

接收用户需求后，先完成以下步骤再开始编码：

1. **能力映射**：将用户需求的关键词对照 `§ 模块接口指南` 找到匹配的模块和 reference 文档
2. **范围确认**：如果需求涉及 2 个以上模块或预计调用 5 个以上接口，列出计划调用的接口清单，让用户确认方案是否正确或需要调整优先级
3. **流程完整性判断**：确认需求的端到端流程是否完整（认证 → 上传 → 转换 → 查询 → 获取 viewToken → 前端展示），如有缺失，主动询问缺失环节的意图
4. **不支持告知**：如果需求无法匹配到任何一个 reference 文档，立即告知用户"当前后端 API 能力范围不支持"，避免沉默生成错误代码或虚构接口
5. **异步操作提醒**：如果需求涉及模型转换、集成或对比，确认用户是否了解这些是异步操作，需要轮询或回调获取结果

## 通用调用流程

所有后端调用都遵循以下步骤。具体接口实现时，直接打开 `references/` 下对应模块的 API 文档即可。

### 1. 认证
参考 [references/authentication.md](references/authentication.md)
- `POST /oauth2/token` 获取 Access Token（有效期7天）
- 后续所有请求 Header 添加 `Authorization: Bearer {accessToken}`
- **`appKey` / `appSecret` 绝对禁止硬编码**，详见下方[安全规范](#安全规范)

### 2. 异步操作回调
参考 [references/callback.md](references/callback.md)
- 调用方传入 `callback` 参数，BIMFACE 完成后 GET 通知
- 收到回调后须验证 `signature` 签名
- 必须返回 HTTP 200 作为回执

### 3. 获取 View Token
参考 [references/view-token.md](references/view-token.md)
- `GET /view/token`，通过 `fileId` / `integrateId` / `compareId` 等获取模型访问凭证
- View Token 有效期为 12 小时，每次使用自动续期
- 将 View Token 传给前端 JSSDK 即可加载模型

### 4. 构件 DSL 查询
- 作用：通过 DSL 语法精确筛选构件，支持 `match` / `contain` / `in` / `boolAnd` / `boolOr`
- `targetType` 可选 `file`（单文件）或 `integration`（集成模型）
- **先从 [references/dsl-query.md](references/dsl-query.md) 学习 DSL 语法，再到具体 API 文档中调用**

## 模块接口指南

### 文件管理服务

核心能力：文件上传/下载、版本管理、文件夹结构。支持 **Hub → 项目 → 文件夹 → 文件项 → 版本文件** 五层管理。

> **⚠️ 关键概念区分（务必理解）**
> - **`fileItem`（文件项）**：代表一个"文件实体"，是日常操作的主要对象。上传文件时先创建 fileItem，后续上传新版本也归属于同一个 fileItem。**如果不需要版本管理，用 fileItem 就够了**。
> - **`file/version`（版本文件）**：是 fileItem 下的某个具体版本。每个 fileItem 最多 100 个版本，多数操作默认针对"当前版本"。只有需要操作历史版本（查旧版本、下载特定版本、删除旧版本）时才涉及此概念。
> - **`projectId` 与根文件夹的关系**：项目的根文件夹 ID **等于** `projectId`。要列出项目根目录下的所有文件夹和文件时，`parentId` 直接传入 `projectId` 即可，无需额外获取。
>
> 接口命名规律：API 路径含 `/fileItems` 的操作文件项级别，含 `/files/{fileId}` 的操作版本级别。

常用接口速查：

| 场景 | 入口文档 |
|------|---------|
| 上传文件（文件流） | [uploadFileItemUsingPOST.md](references/file-management/v1/api-reference/uploadFileItemUsingPOST.md) |
| 上传文件（URL 方式） | [uploadByUrlUsingPOST.md](references/file-management/v1/api-reference/uploadByUrlUsingPOST.md) |
| 分片上传（>5GB 大文件） | [initMultipartUploadUsingPOST.md](references/file-management/v1/api-reference/initMultipartUploadUsingPOST.md) |
| 获取文件项信息 | [getFileItemUsingGET.md](references/file-management/v1/api-reference/getFileItemUsingGET.md) |
| 下载文件 | [getVersionSignedUrlUsingGET.md](references/file-management/v1/api-reference/getVersionSignedUrlUsingGET.md) |
| 上传新版本 | [createVersionUsingPOST.md](references/file-management/v1/api-reference/createVersionUsingPOST.md) |

> **完整 API 索引（48 个接口，按场景分组）** → [INDEX.md](references/file-management/v1/api-reference/INDEX.md)

### 模型数据服务

核心能力：构件属性查询、楼层/分类树、房间空间、碰撞检测。
**前置条件**：文件必须先完成转换。

| 场景 | 入口文档 |
|------|---------|
| 按条件查询构件ID (DSL) | [getElementsUsingPOST_2.md](references/model-data-service/v1/api-reference/getElementsUsingPOST_2.md) |
| 获取构件详情 | [getElementUsingGET_1.md](references/model-data-service/v1/api-reference/getElementUsingGET_1.md) |
| 获取构件材质 | [getElementMaterialUsingGET.md](references/model-data-service/v1/api-reference/getElementMaterialUsingGET.md) |
| 获取楼层列表 | [getFloorsUsingGET_1.md](references/model-data-service/v1/api-reference/getFloorsUsingGET_1.md) |
| 获取构件分类树 | [getTreeUsingPOST.md](references/model-data-service/v1/api-reference/getTreeUsingPOST.md) |
| 碰撞检测 | [createClashDetectiveUsingPOST.md](references/model-data-service/v1/api-reference/createClashDetectiveUsingPOST.md) |
| 模型对比结果 | [pageGetModelCompareResultUsingGET.md](references/model-data-service/v1/api-reference/pageGetModelCompareResultUsingGET.md) |

> **完整 API 索引（67 个接口，按 11 大场景分组）** → [INDEX.md](references/model-data-service/v1/api-reference/INDEX.md)

### 模型转换派生服务

核心能力：文件转换（Translate）、模型集成（Integrate）、模型对比（Compare）。
**前置条件**：文件必须先上传到 BIMFACE。

> **术语说明**：业务场景中常说的"合模"对应后端的**模型集成（Integrate）**——将多个模型文件从数据层合并为一个整体。注意不要与前端 [bimface-helper-v3-3d](../bimface-helper-v3-3d/SKILL.md) 中的"合模工具"（调整多模型展示位置）混淆，两者概念不同。

| 场景 | 入口文档 |
|------|---------|
| 发起转换 | [translateUsingPUT_1.md](references/model-derivative/v1/api-reference/translateUsingPUT_1.md) |
| 查询转换状态 | [getTranslateStatusUsingGET.md](references/model-derivative/v1/api-reference/getTranslateStatusUsingGET.md) |
| 发起集成 | [integrateUsingPUT.md](references/model-derivative/v1/api-reference/integrateUsingPUT.md) |
| 查询集成状态 | [integrateUsingGET.md](references/model-derivative/v1/api-reference/integrateUsingGET.md) |
| 发起对比 | [compareUsingPOST.md](references/model-derivative/v1/api-reference/compareUsingPOST.md) |
| 查询对比状态 | [queryUsingGET.md](references/model-derivative/v1/api-reference/queryUsingGET.md) |

> **完整 API 索引（17 个接口，按 4 大场景分组）** → [INDEX.md](references/model-derivative/v1/api-reference/INDEX.md)

## 端到端流程速览

```
认证 → 上传 → 转换 → 获取 viewToken ──→ 前端 SDK 加载模型/图纸
```

各环节对应模块：
- **认证**：参考 [authentication.md](references/authentication.md)
- **上传**：文件管理模块
- **转换**：模型转换派生模块
- **查询数据**：模型数据服务模块
- **获取 viewToken**：参考 [view-token.md](references/view-token.md)，根据场景传入 `fileId` / `integrateId` / `compareId`

### viewToken：后端与前端（3D/2D SDK）的桥梁

后端通过 `GET /view/token` 获取的 viewToken 字符串，是前端加载模型/图纸的**唯一凭证**：

| 前端场景 | 使用技能 | viewToken 参数来源 |
|---------|---------|-------------------|
| 3D 模型展示与交互 | [bimface-helper-v3-3d](../bimface-helper-v3-3d/SKILL.md) | `fileId`（单模型）/ `integrateId`（集成模型） |
| 2D 图纸展示与交互 | [bimface-helper-v3-2d](../bimface-helper-v3-2d/SKILL.md) | `fileId`（单图纸） |

调用链路：`后端 API → 返回 viewToken 字符串 → 前端通过 SDK 传入 viewToken → 加载模型/图纸`

> ⚠️ viewToken 有效期为 12 小时（每次使用自动续期），前端无需关心刷新逻辑。

## 安全规范

`appKey` 和 `appSecret` 是 BIMFACE 应用的根凭证，泄漏等同于将应用下的所有模型数据和文件权限拱手让人。**必须严格遵守以下规范：**

### 凭据保护
- 🔴 **绝对禁止** 将 `appKey` / `appSecret` 硬编码写入源代码
- 🔴 **绝对禁止** 将 `appKey` / `appSecret` 提交到版本控制系统（Git）
- 🟢 **必须** 通过环境变量或密钥管理服务注入凭据
- 🟢 **必须** 将包含敏感信息的配置文件（如 `.env`）添加到 `.gitignore`
- 🟢 **推荐** 生产环境使用密钥管理服务

### 环境变量配置示例
```bash
# .env（已加入 .gitignore，绝不提交）
BIMFACE_APP_KEY=your_app_key_here
BIMFACE_APP_SECRET=your_app_secret_here
```

> 不同技术栈读取环境变量的方式不同（如 `process.env`、`System.getenv()`、`os.environ` 等），请按项目所用语言的标准做法读取。

### Access Token 管理
- Access Token 有效期为 7 天，全局缓存复用即可，无需每次重新获取
- Token 不应暴露给前端——前端应通过后端代理获取 View Token

### PaaS 调用架构与终端用户鉴权

BIMFACE 是 PaaS 产品，开发者在 BIMFACE 之上构建自己的应用，再交付给**终端用户**使用。**正确的调用链路**如下：

```
❌ 错误架构（禁止）：
   终端用户 → 前端 → BIMFACE API（直接暴露 Access Token）

✅ 正确架构（必须）：
   终端用户 → 开发者的应用 → 开发者后端认证鉴权 → 开发者后端调用 BIMFACE API（带 Access Token）→ 返回结果给终端用户
```

**核心原则**：
- 🔴 **绝对禁止** 将 Access Token 直接透传给终端用户的前端或客户端。Token 是开发者应用的认证凭证，不是终端用户的凭证
- 🟢 **必须** 由开发者后端对终端用户进行身份认证和授权，确认该用户有权操作后方可代为调用 BIMFACE 接口
- 🟢 **必须** 将 BIMFACE API 调用封装在开发者后端服务中，终端用户不应直接知晓 BIMFACE 接口地址
- 🟢 前端需要展示模型时，应由后端代理获取 View Token（详见 [view-token.md](references/view-token.md)）下发给前端，而非传递 Access Token

## 防呆检查

- ⚠️ 调用任何 API 前，确认已获取 Access Token 并在 Header 中正确设置
- ⚠️ `fileId` / `integrateId` 等数值型参数在 JS 中可能超出 `Number.MAX_SAFE_INTEGER`，用字符串传递
- ⚠️ 异步操作（转换/集成/对比）必须在回调或轮询中确认状态为 `success` 后才能进行下一步
- ⚠️ DSL 查询时 `targetType` 必须与 `targetIds` 匹配：`file` 对应文件ID，`integration` 对应集成模型ID
- ⚠️ 回调处理务必验证 `signature`，防止伪造回调
- ⚠️ 获取 View Token 时，`fileId` / `integrateId` / `compareId` 等参数选填且仅填一项，对应关系必须正确

## 命名约定

| 变量 | 说明 |
|------|------|
| `accessToken` | 全局缓存的访问令牌，响应 401 时自动刷新 |
| `instance` | axios 实例，通过拦截器统一注入 `Authorization` 头 |

> 所有 API 文档中 `{accessToken}` 即指代此变量。
