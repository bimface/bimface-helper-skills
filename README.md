# BIMFACE Helper Skills

BIMFACE Helper 是一套面向 BIMFACE 平台的开发辅助技能系列，帮助开发者更高效地调用 BIMFACE 后端 API 和前端 JSSDK，快速构建模型/图纸展示与交互的 Web 应用。

---

## 安装方式

将本仓库中 `skills` 下的对应技能目录复制到你项目的 `skills` 路径中即可：

```
你的项目/
└── skills/
        ├── bimface-helper-backend/    # 后端 API 技能
        ├── bimface-helper-v3-2d/      # 2D 矢量图纸技能
        └── bimface-helper-v3-3d/      # 3D 模型技能
```

> 确保每个技能目录下的 `SKILL.md` 和 `references/` 目录保持完整。

---

## 技能描述

| 技能名称 | 适用场景 | 核心能力 |
|---------|---------|---------|
| **bimface-helper-backend** | BIMFACE v3 服务端 RESTful API 调用 | 文件管理（上传/下载/版本管理）、模型转换/集成/对比、模型数据查询（构件属性/楼层/分类树/碰撞检测）、View Token 获取 |
| **bimface-helper-v3-2d** | 二维矢量图纸前端展示与交互（当前适配 JSSDK v3） | 图纸浏览与控制、图元操作（选中/高亮/隐藏）、图层管理、图纸标注与测量、截图导出 |
| **bimface-helper-v3-3d** | 三维模型前端展示与交互（当前适配 JSSDK v3） | 场景交互（漫游/旋转/视图控制）、构件操作（着色/隔离/强调/冻结）、效果呈现（剖切/扫描/天气/光照/动画）、空间分析、注记与标签 |

---

## 使用方法

安装后在对话中用自然语言描述需求即可调用对应技能。注意在提示词中明确提到 **BIMFACE**，有助于准确匹配到对应的技能。以下是一些示例：

**后端开发（bimface-helper-backend）：**
> 帮我用 BIMFACE 后端 API 写一个 Node.js 脚本：上传一个 Revit 模型文件，发起模型转换，轮询等待转换完成后获取 View Token，并查询模型的楼层列表和构件分类树

**2D 图纸展示（bimface-helper-v3-2d）：**
> 帮我基于 BIMFACE 写一个展示 DWG 图纸的 Web 页面，初始化后加载图纸，支持鼠标缩放和平移，能点击图元查看属性，还能测量两点之间的距离

**3D 模型展示（bimface-helper-v3-3d）：**
> 帮我用 BIMFACE 做一个 3D 模型浏览页面，支持鼠标旋转缩放、点击构件高亮并显示属性面板、按楼层隔离查看构件、以及开启剖切盒查看内部结构

**前后端协作（backend + 3d）：**
> 帮我用 BIMFACE 实现一个完整的 BIM 模型查看页面：后端通过 API 上传模型文件、发起转换、获取 View Token 返回给前端；前端页面加载模型后支持构件按分类筛选着色、框选构件、以及路径漫游

### 跨技能协作建议

- 后端接口返回的 `fileId` / `integrateId` / `compareId` 是前端获取 View Token 的前置条件，注意在前后端之间传递这些 ID
- View Token 是后端与前端的桥梁：后端通过 `GET /view/token` 获取后传递给前端，前端 JSSDK 通过 View Token 即可加载模型/图纸。后端应通过代理方式下发 View Token，**切勿将 Access Token 直接暴露给前端**
- 如果页面需要同时加载模型和图纸，2D 和 3D 技能可以配合使用
