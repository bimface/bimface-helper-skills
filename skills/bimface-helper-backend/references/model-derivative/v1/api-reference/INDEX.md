# 模型转换派生服务 - API 索引

> **前置条件**：文件必须先上传到 BIMFACE。
> 异步操作（转换/集成/对比）建议使用 callback 获取结果，或轮询状态直到 `success`。
> **术语说明**：业务场景中常说的"合模"对应**模型集成（Integrate）**——将多个模型文件从数据层合并为一个整体，与前端 SDK 的"合模工具"（仅调整展示位置）概念不同。

---

## 1. 文件转换 (Translate)

将上传的模型/图纸文件转换为 BIMFACE 可展示的格式。

| 场景 | 入口文档 |
|------|---------|
| 发起转换 | [translateUsingPUT_1.md](translateUsingPUT_1.md) |
| 查询转换状态 | [getTranslateStatusUsingGET.md](getTranslateStatusUsingGET.md) |
| 批量获取转换状态详情 | [getTranslatesUsingPOST.md](getTranslatesUsingPOST.md) |
| 批量获取指定文件转换状态（按项目） | [getTranslationsUsingPOST.md](getTranslationsUsingPOST.md) |
| 获取转换资源列表（按项目，含文件夹层级） | [getResourceListUsingPOST.md](getResourceListUsingPOST.md) |

## 2. 模型集成 (Integrate)

将多个模型文件合并为一个集成模型。

| 场景 | 入口文档 |
|------|---------|
| 发起模型集成 | [integrateUsingPUT.md](integrateUsingPUT.md) |
| 查询集成状态 | [integrateUsingGET.md](integrateUsingGET.md) |
| 更新集成信息（如名称） | [integrateUsingPATCH.md](integrateUsingPATCH.md) |
| 批量查询集成状态 | [getIntegratesUsingPOST.md](getIntegratesUsingPOST.md) |
| 删除集成模型 | [delIntegrateUsingDELETE.md](delIntegrateUsingDELETE.md) |

## 3. 文件对比 (Compare)

对比两个源文件或集成模型的差异。

| 场景 | 入口文档 |
|------|---------|
| 发起文件对比 | [compareUsingPOST.md](compareUsingPOST.md) |
| 获取文件对比状态 | [queryUsingGET.md](queryUsingGET.md) |
| 更新对比信息（如名称） | [compareUsingPATCH.md](compareUsingPATCH.md) |
| 批量获取文件对比状态 | [getModelComparesUsingPOST.md](getModelComparesUsingPOST.md) |
| 删除文件对比 | [deleteUsingDELETE.md](deleteUsingDELETE.md) |

## 4. 图纸拆分 (Drawing Split)

针对多张图纸合并到一个文件的场景，可按单张图纸进行拆分。

| 场景 | 入口文档 |
|------|---------|
| 发起图纸拆分 | [createDrawingSplitUsingPUT.md](createDrawingSplitUsingPUT.md) |
| 查询图纸拆分状态 | [getDrawingSplitUsingGET.md](getDrawingSplitUsingGET.md) |
