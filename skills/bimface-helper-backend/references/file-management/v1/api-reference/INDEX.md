# 文件管理服务 - API 索引

> **层级说明**：Hub → 项目 → 文件夹 → **文件项(File Item)** → **版本文件(File/Version)**
>
> **⚠️ 关键概念区分**：
> - **`fileItem`（文件项）**：代表一个"文件实体"，是日常操作的主要对象。上传时先创建 fileItem，后续上传新版本也归属于同一个 fileItem。**不需要版本管理的话，用 fileItem 就够了**。
> - **`file/version`（版本文件）**：fileItem 下的某个具体版本。每个 fileItem 最多 100 个版本。仅在需要操作历史版本时涉及。
> - **`projectId` 与根文件夹的关系**：项目的根文件夹 ID **等于** `projectId`。要列出项目根目录下的所有文件夹和文件时，`parentId` 直接传入 `projectId` 即可，无需额外获取。
>
> 下表中 `📄 fileItem` 标识的接口操作文件项级别，`📋 version` 标识的接口操作版本级别。如果不关心版本管理，只看 `📄 fileItem` 的接口即可。

---

## 1. Hub（存储中心）管理

| 场景 | 入口文档 |
|------|---------|
| 查询已注册的存储中心列表 | [getHubListUsingGET.md](getHubListUsingGET.md) |
| 查询指定 Hub 的元信息 | [getHubMetaUsingGET.md](getHubMetaUsingGET.md) |

## 2. 项目管理

| 场景 | 入口文档 |
|------|---------|
| 查询项目列表 | [getProjectListUsingGET.md](getProjectListUsingGET.md) |
| 查询项目详情 | [getProjectMetaUsingGET.md](getProjectMetaUsingGET.md) |
| 创建项目 | [createProjectUsingPOST.md](createProjectUsingPOST.md) |
| 更新项目信息 | [updateProjectUsingPATCH.md](updateProjectUsingPATCH.md) |
| 删除项目 | [deleteProjectUsingDELETE.md](deleteProjectUsingDELETE.md) |
| 获取项目根文件夹 | [getProjectRootFolderInfoUsingGET.md](getProjectRootFolderInfoUsingGET.md) |

## 3. 文件夹管理

| 场景 | 入口文档 |
|------|---------|
| 创建文件夹 | [createFolderUsingPOST.md](createFolderUsingPOST.md) |
| 获取文件夹信息 | [getFolderUsingGET.md](getFolderUsingGET.md) |
| 获取文件夹子项列表 | [getFolderChildrenUsingPOST.md](getFolderChildrenUsingPOST.md) |
| 获取文件夹路径 | [getFolderPathByIdUsingGET.md](getFolderPathByIdUsingGET.md) |
| 获取父文件夹 | [getParentUsingGET.md](getParentUsingGET.md) |
| 文件夹重命名 | [updateFolderUsingPATCH.md](updateFolderUsingPATCH.md) |
| 删除文件夹 | [deleteFolderUsingDELETE.md](deleteFolderUsingDELETE.md) |

## 4. 文件上传

### 4a. 普通上传

| 场景 | 入口文档 | 层级 |
|------|---------|------|
| 文件流上传 | [uploadFileItemUsingPOST.md](uploadFileItemUsingPOST.md) | 📄 fileItem |
| URL 方式上传 | [uploadByUrlUsingPOST.md](uploadByUrlUsingPOST.md) | 📄 fileItem |
| Web 端直传（Policy 凭证） | [uploadByPolicyUsingPOST.md](uploadByPolicyUsingPOST.md) | 📄 fileItem |
| 获取文件直传 Policy 凭证 | [getFilePolicyUsingGET.md](getFilePolicyUsingGET.md) | 📄 fileItem（新文件项） |
| 获取新版本直传 Policy 凭证 | [getFilePolicyUsingGET_1.md](getFilePolicyUsingGET_1.md) | 📋 version（新版本） |

### 4b. 分片上传（文件项级别）

适用场景：文件 >5GB 或网络不稳定。上传完成后创建新的 fileItem。

| 场景 | 入口文档 | 层级 |
|------|---------|------|
| 创建分片上传任务（第 1 步） | [initMultipartUploadUsingPOST.md](initMultipartUploadUsingPOST.md) | 📄 fileItem |
| 获取分片上传 URL（第 2 步） | [getMultipartSignedUrlUsingPOST.md](getMultipartSignedUrlUsingPOST.md) | 📄 fileItem |
| 查询已上传分片列表 | [listMultiPartUploadUsingGET.md](listMultiPartUploadUsingGET.md) | 📄 fileItem |
| 合并分片生成文件（第 3 步） | [completeMultiPartUploadUsingPOST.md](completeMultiPartUploadUsingPOST.md) | 📄 fileItem |
| 终止分片上传任务 | [abortMultiPartUploadUsingPOST.md](abortMultiPartUploadUsingPOST.md) | 📄 fileItem |

### 4c. 分片上传（版本文件级别）

适用场景：在已有 fileItem 中上传大文件新版本。

| 场景 | 入口文档 | 层级 |
|------|---------|------|
| 创建版本文件分片上传任务（第 1 步） | [initMultipartUploadUsingPOST_1.md](initMultipartUploadUsingPOST_1.md) | 📋 version |
| 获取分片上传 URL - 版本（第 2 步） | [getMultipartSignedUrlUsingPOST_1.md](getMultipartSignedUrlUsingPOST_1.md) | 📋 version |
| 查询已上传分片列表 - 版本 | [listMultiPartUploadUsingGET_1.md](listMultiPartUploadUsingGET_1.md) | 📋 version |
| 合并分片生成版本文件（第 3 步） | [completeMultiPartUploadUsingPOST_1.md](completeMultiPartUploadUsingPOST_1.md) | 📋 version |
| 终止分片上传任务 - 版本 | [abortMultiPartUploadUsingPOST_1.md](abortMultiPartUploadUsingPOST_1.md) | 📋 version |

### 4d. 断点续传

适用场景：网络不稳定、大文件上传易中断时。

| 场景 | 入口文档 | 层级 |
|------|---------|------|
| 创建追加文件（初始化，第 1 步） | [createAppendFileUsingPOST.md](createAppendFileUsingPOST.md) | 📄 fileItem |
| 追加上传文件（第 2 步） | [appendUploadUsingPOST.md](appendUploadUsingPOST.md) | 📄 fileItem |
| 查询追加文件信息 | [getAppendFileUsingGET.md](getAppendFileUsingGET.md) | 📄 fileItem |

## 5. 文件操作（fileItem 级别）

> 日常文件管理使用本节接口，**不需要理解版本概念**。

| 场景 | 入口文档 |
|------|---------|
| 获取文件项信息 | [getFileItemUsingGET.md](getFileItemUsingGET.md) |
| 获取文件上传状态 | [getFileItemUploadStatusUsingGET.md](getFileItemUploadStatusUsingGET.md) |
| 获取文件路径 | [getFileItemPathByIdUsingGET.md](getFileItemPathByIdUsingGET.md) |
| 文件项重命名 | [fileRenameUsingPATCH.md](fileRenameUsingPATCH.md) |
| 移动文件位置 | [moveFileUsingPATCH.md](moveFileUsingPATCH.md) |
| 复制文件 | [copyFileUsingPOST.md](copyFileUsingPOST.md) |
| 批量删除文件项 | [batchDeleteFileItemsUsingDELETE.md](batchDeleteFileItemsUsingDELETE.md) |

## 6. 版本管理（version 级别）

> 仅在需要操作历史版本时使用本节接口。如果你只关心文件当前内容，**不需要使用这些接口**。

| 场景 | 入口文档 |
|------|---------|
| 上传版本文件（文件流） | [createVersionUsingPOST.md](createVersionUsingPOST.md) |
| 获取指定版本信息 | [getVersionUsingGET.md](getVersionUsingGET.md) |
| 获取所有版本列表 | [getAllVersionsUsingGET.md](getAllVersionsUsingGET.md) |
| 版本文件重命名 | [renameFileUsingPATCH.md](renameFileUsingPATCH.md) |
| 删除指定版本文件 | [deleteVersionUsingDELETE.md](deleteVersionUsingDELETE.md) |

## 7. 文件下载

| 场景 | 入口文档 | 层级 |
|------|---------|------|
| 下载文件项当前版本 | [getFileItemSignedUrlUsingGET.md](getFileItemSignedUrlUsingGET.md) | 📄 fileItem |
| 下载指定版本文件 | [getVersionSignedUrlUsingGET.md](getVersionSignedUrlUsingGET.md) | 📋 version |
| 打包下载多个文件 | [downloadFilesUsingGET.md](downloadFilesUsingGET.md) | 📄 fileItem |
