# BIMFACE初始化（矢量图纸）

## 行为约束
- 预留一个div作为图纸显示容器，占据页面的大部分区域。DOM容器的ID需传递给BIMFACE SDK初始化函数，并确保HTML与JS调用时的ID匹配。
- 要在图纸 `Loaded` 事件触发后，再进行 `drawing2D` 等与图纸功能相关对象的初始化。

## 使用步骤

### 1. 引入BIMFACE SDK
html中引入script:
```html
<script src="https://static.bimface.com/api/BimfaceSDKLoader/BimfaceSDKLoader@latest-release.js"
        charset="utf-8"></script>
```

### 2. 初始化BIMFACE SDK（矢量图纸）
根据用户输入的viewToken，在js中初始化BIMFACE SDK:
```javascript
let viewer2D,  // 二维图纸查看器对象
    app2D,     // WebApplicationDrawing应用对象
    drawing2D  // 图纸对象

function initBimfaceApp() {
    try {
        const viewToken = "your-view-token"; // 从用户输入参数中获取viewToken，替换到此处
        let options = new BimfaceSDKLoaderConfig();
        options.viewToken = viewToken;
        BimfaceSDKLoader.load(options, successCallback, failureCallback);
        function successCallback(viewMetaData) {
            if (viewMetaData.viewType == "dwgView") {
                // ======== 判断是否为二维矢量图纸 ========
                let dom4Show = document.getElementById('bimface-container');
                let webAppConfig = new Glodon.Bimface.Application.WebApplicationDrawingConfig();
                webAppConfig.domElement = dom4Show;
                // 创建WebApplicationDrawing
                app2D = new Glodon.Bimface.Application.WebApplicationDrawing(webAppConfig);
                // 从WebApplicationDrawing获取viewer2D对象
                viewer2D = app2D.getViewer();
                // 加载图纸
                viewer2D.loadDrawing({ viewToken: viewToken });
                // 监听图纸加载完成事件
                viewer2D.addEventListener(Glodon.Bimface.Viewer.ViewerDrawingEvent.Loaded, function () {
                    // 渲染图纸
                    viewer2D.render();
                    // 获取图纸对象
                    drawing2D = viewer2D.getDrawing();
                });
            }
        }
        function failureCallback(error) {
            console.log(error);
        }
    } catch (error) {
        console.error(error);
    }
}
initBimfaceApp();
```

## 接口说明

### 1. 如何获取ViewToken
- 需通过BIMFACE的后端接口获取ViewToken
- 如果用户没有指定ViewToken的获取方式，则需要用户输入一个临时的token字符串，确保有图纸可以加载

### 2. loadDrawing
```javascript
// 加载图纸
viewer2D.loadDrawing({
    viewToken: viewToken,  // viewToken必传
    modelId: modelId,      // modelId可选，默认为fileId，用于后续获取图纸对象
    frameId: frameId       // frameId可选，加载拆分后的子图时传入
});
```

### 3. 加载带sheetId的rvt导出图纸
```javascript
viewer2D.loadDrawing({
    viewToken: viewToken,
    sheetId: sheetId  // rvt模型导出的图纸ID
});
```

### 4. 设置语言

在 `BimfaceSDKLoaderConfig` 中指定 SDK 界面语言，不设置时默认为中文。

```JavaScript
let options = new BimfaceSDKLoaderConfig();
options.viewToken = viewToken;
options.language = BimfaceLanguageOption.en_GB;  // 英文
// options.language = BimfaceLanguageOption.sv_SE;  // 瑞典语
BimfaceSDKLoader.load(options, successCallback, failureCallback);
```
