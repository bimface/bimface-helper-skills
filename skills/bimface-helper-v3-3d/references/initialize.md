# BIMFACE初始化

## 行为约束
- 预留一个div作为模型显示容器，占据页面的大部分区域。必须将DOM容器的ID需传递给BIMFACE SDK初始化函数，并确保HTML与JS调用时的ID完美匹配。
- 要在ViewAdded事件触发后，再进行Model3D等与BIMFACE功能相关对象的初始化。

## 使用步骤

### 1. 引入BIMFACE SDK
html中引入script:
```html
<script src="https://static.bimface.com/api/BimfaceSDKLoader/BimfaceSDKLoader@latest-release.js"
        charset="utf-8"></script>
```

### 2. 初始化BIMFACE SDK
根据用户输入的viewToken，在js中初始化BIMFACE SDK:
```javascript
// 初始化BIMFACE SDK
let viewer3D,  // 3D查看器对象
    app3D,   // BIMFACE Web应用对象
    model3D  // 3D模型对象

function initBimfaceApp() {
    try {
        const viewToken = "your-view-token"; // 从用户输入参数中获取viewToken，替换到此处
        // 初始化显示组件
        let options = new BimfaceSDKLoaderConfig();
        options.viewToken = viewToken;
        BimfaceSDKLoader.load(options, successCallback, failureCallback);
        function successCallback(viewMetaData) {
            if (viewMetaData.viewType == "3DView") {
                // ======== 判断是否为3D模型 ========        
                // 获取DOM元素，ID根据前端页面预留的div容器ID设置
                let dom4Show = document.getElementById('bimface-container');
                let webAppConfig = new Glodon.Bimface.Application.WebApplication3DConfig();
                webAppConfig.domElement = dom4Show;
                // 创建WebApplication
                app3D = new Glodon.Bimface.Application.WebApplication3D(webAppConfig);
                // 从WebApplication获取viewer3D对象
                viewer3D = app3D.getViewer();
                // 添加待显示的模型
                viewer3D.loadModel({
                    viewToken: viewToken,
                });
                // 监听添加view完成的事件
                viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded, function () {
                    // 渲染3D模型
                    viewer3D.render();
                    // 获取模型对象
                    model3D = viewer3D.getModel();
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
- 如果用户没有指定ViewToken的获取方式，则需要用户输入一个临时的token字符串，确保有模型可以加载

### 2. LoadModel
```JavaScript
// 加载模型
viewer3D.loadModel({
    viewToken: viewToken,  // viewToken必传
    modelId: modelId,  // modelId可选，默认值为该模型的FileId。该ID用于后续获取该模型对象并进行操作。
});
```

### 3. 设置语言

在 `BimfaceSDKLoaderConfig` 中指定 SDK 界面语言，不设置时默认为中文。

```JavaScript
let options = new BimfaceSDKLoaderConfig();
options.viewToken = viewToken;
options.language = BimfaceLanguageOption.en_GB;  // 英文
// options.language = BimfaceLanguageOption.sv_SE;  // 瑞典语
BimfaceSDKLoader.load(options, successCallback, failureCallback);
```

### 4. 自定义线框显示

在 `WebApplication3DConfig` 中按构件类别筛选显示线框。

```JavaScript
webAppConfig.borderLineVisibility = [{ categoryId: -2000151 }];
```