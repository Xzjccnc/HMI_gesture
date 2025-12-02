项目名称：Gesture（车载手势控制 UI 演示）

概述
- 基于 Uni-App 的 UVue 页面，演示使用 MediaPipe Hands 进行手势识别，结合 Three.js 渲染 3D 背景。
- 支持实时手部跟踪、捏合值控制（调节音量/地图缩放）、左右挥动手势切换屏幕。
- 通过 renderjs 模块与视图层通信，加载外部 CDN 脚本并接管 WebGL 渲染与摄像头视频帧处理。

功能特性
- 3D 背景：星空点阵与地面网格滚动（Three.js）。
- 手势识别：MediaPipe Hands，单手检测、食指与拇指距离计算、手腕横向位移识别。
- 手势映射：
  - 捏合值：在音乐页面调节音量，在地图页面调节缩放。
  - 左右挥动：切换屏幕（仪表盘 / 地图 / 音乐）。
- 手势光标：根据食指位置在屏幕上渲染光标，捏合态有视觉反馈。

目录结构
- pages/index/index.uvue：主界面与逻辑桥接（renderjs）。

技术栈
- Uni-App（UVue 页面）
- RenderJS 模块（H5/App-Vue 环境）
- Three.js r128（CDN 引入）
- MediaPipe Hands、camera_utils、control_utils（CDN 引入）

运行环境与前置条件
- 推荐使用 HBuilderX（3.9+，支持 UVue 与 renderjs）。
- 需开启摄像头权限：
  - H5：必须在 HTTPS 或本机 localhost 下访问，浏览器需允许摄像头。
  - App：打包时需声明摄像头权限。
- 设备需可访问外网以加载 CDN 资源（cdnjs、jsDelivr）。

启动与预览
- 使用 HBuilderX：
  1. 将本项目导入 HBuilderX。
  2. 运行到浏览器或运行到 Android 真机。
  3. 首屏点击“启动引擎（开启摄像头）”，授予摄像头权限后进入系统。
- 使用 H5 本地预览（可选）：
  - 在具备 HTTPS 的本地服务或通过 HBuilderX 内置服务运行。
  - 确保浏览器允许 `getUserMedia`。

使用说明
- 首屏按钮：启动引擎并初始化摄像头与 3D 场景。
- 屏幕切换：左右挥动手势（检测手腕 X 轴位移）。
- 捏合控制：
  - 音乐页：捏合越紧，音量越低/高（范围 0–100%）。
  - 地图页：捏合映射到缩放（范围约 50%–350%）。
- 光标跟随：食指位置映射到屏幕坐标；捏合时光标缩小并填充颜色。

关键代码位置
- 外部依赖加载：pages/index/index.uvue:230–234
- Three.js 场景初始化：pages/index/index.uvue:244–295
- MediaPipe Hands 配置与回调：pages/index/index.uvue:296–369
- 手势判定：挥动阈值与节流 pages/index/index.uvue:339–347；捏合值归一化 pages/index/index.uvue:351–353
- 视图层交互：手势状态与提示 pages/index/index.uvue:166–195；页面逻辑 pages/index/index.uvue:172–189

常见问题
- 摄像头无法启动：检查 HTTPS/权限/是否有其他程序占用摄像头。
- CDN 加载失败：确认网络可访问 cdnjs 与 jsDelivr；必要时替换为本地依赖或企业镜像。
- 性能问题：低端设备上可降低星点数量或关闭雾化效果。
- H5 手势不灵敏：在光线充足背景简单的环境下使用，提升识别稳定性。

自定义与扩展
- 调整挥动灵敏度：修改阈值与节流 pages/index/index.uvue:339–341。
- 调整捏合映射：修改归一化计算 pages/index/index.uvue:351。
- 背景效果：在 initThreeJS 中增删对象或修改材质与动画速度。

许可
- 未指定许可。请在分发前补充适用的开源或商业许可文本。