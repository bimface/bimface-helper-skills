# 模型爆炸（Explosion）

## 使用约束与说明

- 爆炸效果通过模型对象 `model.setExplosion()` 和 `model.clearExplosion()` 控制
- 必须在模型加载完成后调用
- 支持两种爆炸类型：`Floor`（按楼层分离）和 `Center`（从中心点发散）
- `extent` 参数范围 `[0, 3]`，值越大分离程度越大
- `direction` 仅对 `Floor` 类型有效，用于指定分离方向
- 爆炸效果是纯视觉变换，不影响构件的实际坐标

---

## 爆炸类型

| 类型 | 值 | 说明 |
|------|------|------|
| 楼层爆炸 | `"Floor"` | 构件按所属楼层进行分层分离 |
| 中心爆炸 | `"Center"` | 构件从模型中心点向外发散 |

---

## setExplosion —— 设置爆炸效果

### Floor 楼层爆炸

```javascript
// 按 Z 轴方向（默认）进行楼层分离，分离程度 2
model.setExplosion({
  type: "Floor",
  extent: 2,
  direction: { x: 0, y: 0, z: 1 }
});

// 简化写法（使用默认方向 {x:0, y:0, z:1}）
model.setExplosion({
  type: "Floor",
  extent: 1.5
});
```

### Center 中心爆炸

```javascript
// 从模型中心点向外发散，分离程度 1.5
model.setExplosion({
  type: "Center",
  extent: 1.5
});

// 最大分离程度
model.setExplosion({
  type: "Center",
  extent: 3
});
```

---

## clearExplosion —— 清除爆炸效果

```javascript
model.clearExplosion();
```

---

## 完整示例

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
viewer3D.addModel(viewToken);
const model = viewer3D.getModel();

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  // === 楼层爆炸 ===
  document.getElementById("btnFloorExplosion").addEventListener("click", () => {
    // 按 Z 轴方向，分离程度 2（最大值 3）
    model.setExplosion({
      type: "Floor",
      extent: 2,
      direction: { x: 0, y: 0, z: 1 }
    });
  });

  // 按 Y 轴方向楼层分离
  document.getElementById("btnFloorExplosionY").addEventListener("click", () => {
    model.setExplosion({
      type: "Floor",
      extent: 1.5,
      direction: { x: 0, y: 1, z: 0 }
    });
  });

  // 使用滑块动态调整爆炸程度
  document.getElementById("rangeExtent").addEventListener("input", (e) => {
    const extent = parseFloat(e.target.value); // 0 ~ 3
    model.setExplosion({
      type: "Floor",
      extent: extent,
      direction: { x: 0, y: 0, z: 1 }
    });
  });

  // === 中心爆炸 ===
  document.getElementById("btnCenterExplosion").addEventListener("click", () => {
    model.setExplosion({
      type: "Center",
      extent: 1.5
    });
  });

  // 最大中心爆炸
  document.getElementById("btnMaxCenterExplosion").addEventListener("click", () => {
    model.setExplosion({
      type: "Center",
      extent: 3
    });
  });

  // === 清除爆炸 ===
  document.getElementById("btnClearExplosion").addEventListener("click", () => {
    model.clearExplosion();
  });
});
```

---

## 注意事项

- `extent` 有效范围为 `[0, 3]`，超出范围可能导致不可预期的效果
- `Floor` 类型依赖模型中的楼层信息（`levelName` 等），如果模型没有楼层数据则效果可能不明显
- `Center` 类型不需要 `direction` 参数
- `clearExplosion()` 会将模型恢复到原始状态
- 爆炸效果是全局的，无法对单个构件独立设置爆炸
