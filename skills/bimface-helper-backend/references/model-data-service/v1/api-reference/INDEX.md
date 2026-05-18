# 模型数据服务 - API 索引

> **前置条件**：文件必须先完成转换才能查询数据。
> **标识说明**：`(单文件)` = 操作单个文件模型；`(集成)` = 操作集成模型；`(DSL)` = 使用 DSL 查询语法。

---

## 1. DSL 构件查询

支持 `match` / `contain` / `in` / `boolAnd` / `boolOr` 等查询语法。DSL 语法说明详见 [dsl-query.md](../../../dsl-query.md)。

| 场景 | 入口文档 |
|------|---------|
| DSL 查询符合条件的构件ID列表 | [getElementsUsingPOST_2.md](getElementsUsingPOST_2.md) |
| 查询指定模型构件属性的所有值 (DSL) | [getPropertyValuesUsingGET.md](getPropertyValuesUsingGET.md) |
| 生成分页查询的 ContextId (DSL) | [getPaginationQueryIdUsingGET.md](getPaginationQueryIdUsingGET.md) |

## 2. 构件属性查询

| 场景 | 入口文档 |
|------|---------|
| 获取单构件属性 (单文件) | [getElementUsingGET_1.md](getElementUsingGET_1.md) |
| 获取单构件属性 (集成) | [getElementUsingGET_2.md](getElementUsingGET_2.md) |
| 获取子文件/链接内指定构件的属性 | [getElementUsingGET_3.md](getElementUsingGET_3.md) |
| 批量获取构件属性 (单文件，最多 1000 个) | [getElementsUsingPOST.md](getElementsUsingPOST.md) |
| 批量获取构件属性 (集成，最多 1000 个) | [getElementsUsingPOST_1.md](getElementsUsingPOST_1.md) |
| 查询满足条件的构件 (单文件) | [getElementIdsUsingGET.md](getElementIdsUsingGET.md) |
| 查询满足条件的构件 (集成) | [getElementIdsUsingGET_1.md](getElementIdsUsingGET_1.md) |
| 获取构件材质 (单文件) | [getElementMaterialUsingGET.md](getElementMaterialUsingGET.md) |
| 获取构件材质 (集成) | [getElementMaterialUsingGET_1.md](getElementMaterialUsingGET_1.md) |
| 获取模型所有属性字段 (单文件) | [getPropertyKeysUsingGET.md](getPropertyKeysUsingGET.md) |
| 获取模型所有属性字段 (集成) | [getPropertyKeysUsingGET_2.md](getPropertyKeysUsingGET_2.md) |
| 查询指定构件属性的所有值 (集成) | [getPropertyValuesUsingGET_2.md](getPropertyValuesUsingGET_2.md) |
| 获取多个构件的共同属性 (单文件) | [getCommonElementPropertiesUsingGET.md](getCommonElementPropertiesUsingGET.md) |
| 获取多个构件的共同属性 (集成) | [getCommonElementPropertiesUsingPOST.md](getCommonElementPropertiesUsingPOST.md) |
| 批量获取部件 (Assembly) 属性 | [getAssembliesUsingPOST.md](getAssembliesUsingPOST.md) |

## 3. 楼层与分类树

| 场景 | 入口文档 |
|------|---------|
| 获取单模型楼层信息 | [getFloorsUsingGET_1.md](getFloorsUsingGET_1.md) |
| 获取多模型楼层信息 | [getFloorsUsingGET.md](getFloorsUsingGET.md) |
| 获取楼层列表 (集成) | [getFloorsUsingGET_2.md](getFloorsUsingGET_2.md) |
| 获取构件分类树 (单文件) | [getTreeUsingPOST.md](getTreeUsingPOST.md) |
| 获取分类树 (集成) | [getTreeUsingPOST_1.md](getTreeUsingPOST_1.md) |
| 计算指定构件列表的包围盒 | [getIntegrationFloorUsingGET.md](getIntegrationFloorUsingGET.md) |

## 4. 房间 / 面积 / 空间

| 场景 | 入口文档 |
|------|---------|
| 获取房间列表 (单文件，按楼层/构件ID) | [getRoomsUsingGET.md](getRoomsUsingGET.md) |
| 获取房间列表 (集成，按楼层/构件ID) | [getRoomsUsingGET_1.md](getRoomsUsingGET_1.md) |
| 获取单个房间信息 (单文件) | [getRoomUsingGET.md](getRoomUsingGET.md) |
| 获取指定房间属性 (集成) | [getRoomPropertyUsingGET.md](getRoomPropertyUsingGET.md) |
| 获取楼层对应面积分区列表 (单文件) | [getAreasByFloorIdUsingGET.md](getAreasByFloorIdUsingGET.md) |
| 获取楼层对应面积分区列表 (集成) | [getAreasUsingGET.md](getAreasUsingGET.md) |
| 获取单个面积分区信息 (单文件) | [getAreaUsingGET.md](getAreaUsingGET.md) |
| 获取单个面积分区信息 (集成) | [getAreaUsingGET_1.md](getAreaUsingGET_1.md) |
| 查询模型空间 ID 列表 | [getSpaceIdsUsingPOST.md](getSpaceIdsUsingPOST.md) |
| 查询单个空间属性 | [getSpacePropertiesUsingPOST.md](getSpacePropertiesUsingPOST.md) |
| 批量查询空间属性 | [batchGetSpacePropertiesUsingPOST.md](batchGetSpacePropertiesUsingPOST.md) |
| 依据坐标生成拉伸空间 | [newSpaceExtrusionUsingPOST.md](newSpaceExtrusionUsingPOST.md) |
| 删除指定空间 | [deleteSpaceUsingDELETE.md](deleteSpaceUsingDELETE.md) |

## 5. 碰撞检测

分 v1 和 v2 两个版本，v2 推荐用于新项目。

| 场景 | 入口文档 |
|------|---------|
| 发起碰撞检测 (v1) | [createClashDetectiveUsingPOST.md](createClashDetectiveUsingPOST.md) |
| 查询碰撞检测状态 (v1) | [queryClashDetectiveUsingGET.md](queryClashDetectiveUsingGET.md) |
| 获取碰撞检测结果 (v1) | [getClashDetectiveResultUsingGET.md](getClashDetectiveResultUsingGET.md) |
| 查询碰撞检测ID列表 (v1) | [queryClashDetectiveByModelIdUsingGET.md](queryClashDetectiveByModelIdUsingGET.md) |
| 发起碰撞检测 (v2) | [../../v2/api-reference/createClashDetectiveUsingPOST.md](../../v2/api-reference/createClashDetectiveUsingPOST.md) |
| 查询碰撞检测状态 (v2) | [../../v2/api-reference/queryClashDetectiveUsingGET.md](../../v2/api-reference/queryClashDetectiveUsingGET.md) |
| 获取碰撞检测结果 (v2) | [../../v2/api-reference/getClashDetectiveResultUsingGET.md](../../v2/api-reference/getClashDetectiveResultUsingGET.md) |
| 查询碰撞检测ID列表 (v2) | [../../v2/api-reference/queryClashDetectiveByModelIdUsingGET.md](../../v2/api-reference/queryClashDetectiveByModelIdUsingGET.md) |

## 6. 模型对比

| 场景 | 入口文档 |
|------|---------|
| 分页获取模型对比结果 | [pageGetModelCompareResultUsingGET.md](pageGetModelCompareResultUsingGET.md) |
| 获取模型构件对比差异 | [getElementChangeUsingGET.md](getElementChangeUsingGET.md) |
| 获取模型对比构件分类树 | [getTreeUsingGET.md](getTreeUsingGET.md) |
| 分页获取图纸对比结果 | [pageGetDrawingCompareResultUsingGET.md](pageGetDrawingCompareResultUsingGET.md) |

## 7. 视图与图纸

| 场景 | 入口文档 |
|------|---------|
| 获取视图信息 (集成，按 viewType 筛选) | [getFileViewsUsingGET.md](getFileViewsUsingGET.md) |
| 获取三维视点或二维视图列表 (单文件) | [getViewsUsingGET.md](getViewsUsingGET.md) |
| 获取图纸拆分结果 | [getDrawingFramesUsingGET.md](getDrawingFramesUsingGET.md) |
| 获取图纸列表 | [singleModelGetDrawingSheetsUsingGET.md](singleModelGetDrawingSheetsUsingGET.md) |

## 8. 链接 / MEP / 族

| 场景 | 入口文档 |
|------|---------|
| 获取模型链接信息 (单文件) | [getLinksUsingGET.md](getLinksUsingGET.md) |
| 获取模型链接信息 (集成，有向无环图) | [getLinkGraphUsingGET.md](getLinkGraphUsingGET.md) |
| 获取 MEP 系统信息 | [getMEPSystemUsingGET.md](getMEPSystemUsingGET.md) |
| 族类型列表 | [getFamilyTypesUsingGET.md](getFamilyTypesUsingGET.md) |
| 族类型属性 key 列表 | [getFamilyPropertyNamesUsingGET.md](getFamilyPropertyNamesUsingGET.md) |
| 族类型属性列表 | [getFamilyTypePropertyUsingGET_1.md](getFamilyTypePropertyUsingGET_1.md) |
| 获取参与集成的子文件列表 | [getIntegrateFilesUsingGET.md](getIntegrateFilesUsingGET.md) |

## 9. 属性修改（覆盖）

| 场景 | 入口文档 |
|------|---------|
| 修改单模型指定构件属性 (单文件) | [updateUsingPUT.md](updateUsingPUT.md) |
| 修改指定构件属性 (集成) | [updateUsingPUT_1.md](updateUsingPUT_1.md) |
| 删除单模型指定构件属性 (单文件) | [deleteUsingDELETE.md](deleteUsingDELETE.md) |
| 删除指定构件属性 (集成) | [deleteUsingDELETE_1.md](deleteUsingDELETE_1.md) |

## 10. 分享

| 场景 | 入口文档 |
|------|---------|
| 生成分享链接 | [createUsingPOST_1.md](createUsingPOST_1.md) |
| 获取分享链接信息 | [getShareLinkUsingGET.md](getShareLinkUsingGET.md) |
| 获取分享列表 | [shareListUsingGET.md](shareListUsingGET.md) |
| 更新分享链接 | [updateUsingPATCH_1.md](updateUsingPATCH_1.md) |
| 取消分享链接 | [deleteUsingDELETE_2.md](deleteUsingDELETE_2.md) |
| 批量取消分享链接 | [batchDeleteUsingDELETE_1.md](batchDeleteUsingDELETE_1.md) |

## 11. 离线数据包

| 场景 | 入口文档 |
|------|---------|
| 创建单文件的离线数据包 | [offlineFiles.md](offlineFiles.md) |
| 数据包大小 / 资源查询 | [translateOtherFiles.md](translateOtherFiles.md) |
