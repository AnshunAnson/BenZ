# UE4 项目加载界面优化计划

## 需求分析
1. 将 UE 窗口设置为全屏，去除前端画布包裹
2. 参考 JS 的 UE 资源 loading 逻辑，设计一个酷炫的加载界面

## 仓库研究结论

### 现有加载逻辑分析
- **入口文件**: `BenZ-HTML5-Shipping.UE4.js`
- **进度跟踪函数**:
  - `taskProgress(taskId, progress)` - 显示当前加载任务进度
  - `reportDownloadProgress(url, downloadedBytes, totalBytes, finished)` - 报告文件下载进度
  - `loadTasks` 数组 - 定义了四个加载阶段:
    - TASK_DOWNLOADING (0) - "Downloading"
    - TASK_COMPILING (1) - "Compiling WebAssembly"
    - TASK_SHADERS (2) - "Building shaders"
    - TASK_MAIN (3) - "Launching engine"

- **进度数据存储**: `Module.assetDownloadProgress` 对象存储每个文件的下载进度
- **现有 UI**: 简单的文本消息在 `#compilingmessage` 元素中显示

### 现有 CSS 结构
- `.wrapper` - 画布容器
- `.emscripten` - canvas 样式
- `#canvas` - 游戏画布

## 需要修改的文件

1. `index.html` - 添加加载界面 HTML 结构
2. `BenZ-HTML5-Shipping.html` - 同步修改（保持一致）
3. `BenZ-HTML5-Shipping.css` - 添加加载界面和全屏样式
4. `BenZ-HTML5-Shipping.UE4.js` - 集成新的加载进度更新逻辑

## 修改步骤

### 步骤 1: 修改 HTML 文件
- 添加酷炫的加载界面 DOM 结构，包括：
  - 加载动画
  - 进度条
  - 百分比显示
  - 当前任务文本
- 优化 canvas 容器结构，去除不必要的包裹

### 步骤 2: 修改 CSS 文件
- 添加全屏样式：`html, body, #canvas` 设置为 `width: 100vw; height: 100vh; margin: 0; padding: 0;`
- 隐藏原始的 `#compilingmessage` 和 `#buttonarea`
- 添加加载界面的动画样式：
  - 渐变背景
  - 旋转加载图标
  - 平滑的进度条动画
  - 科技感的设计元素

### 步骤 3: 修改 JS 文件
- 修改 `taskProgress` 函数，更新新加载界面的进度
- 修改 `reportDownloadProgress` 函数，集成新界面
- 添加加载完成后隐藏加载界面的逻辑

## 潜在依赖和考虑

1. **兼容性**: 确保新样式在主流浏览器中正常工作
2. **性能**: 加载动画不应影响游戏加载性能
3. **Z-index 层级**: 确保加载界面在正确的层级
4. **过渡效果**: 加载完成后平滑过渡到游戏

## 风险处理

- 如遇样式冲突，使用 `!important` 或更具体的选择器
- 确保 canvas 始终保持正确的尺寸
- 保留原始加载逻辑作为备用方案

## 设计方案

### 加载界面设计
- 深色科技感背景（渐变）
- 中心旋转的 3D 风格加载图标
- 动态进度条（带发光效果）
- 百分比数字显示
- 当前任务状态文本
- 加载完成后淡出效果

### 全屏模式
- Canvas 占据整个视口
- 移除所有边距和填充
- 隐藏所有额外的 UI 元素
