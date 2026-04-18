## AgriField-Mapper —— 植保无人机作业地块自动识别程序

[![Vue 2](https://img.shields.io/badge/Vue-2.6.11-brightgreen.svg)](https://v2.vuejs.org/)
[![AMap](https://img.shields.io/badge/高德地图-API-blue)](https://lbs.amap.com/)
[![Baidu AI](https://img.shields.io/badge/百度AI-图像分割-orange)](https://ai.baidu.com/)

> 基于 Vue2 与高德地图的植保无人机地块智能识别系统（学位论文实现）。

## 📖 项目简介

本项目面向精准农业中的**植保无人机作业规划**需求，实现了一套**农田地块自动识别与边界提取**的可视化工具。用户可通过地图交互或图像上传，利用百度 AI 自定义分割模型识别农田区域，并基于凸包 / 凹包算法生成规整的作业地块边界，为无人机航线规划提供精准的地理围栏数据。

**核心流程：**
1. **地图选点 / 图像上传** → 获取待识别区域的影像。
2. **百度 AI 分割** → 调用定制化图像分割接口，提取农田掩膜。
3. **几何处理** → 通过 `concaveman` 与 `monotone-convex-hull-2d` 计算地块轮廓。
4. **结果展示与导出** → 在高德地图上绘制地块边界，并支持截图保存。

## ✨ 主要功能

- 🗺️ **高德地图集成**：使用 `@amap/amap-jsapi-loader` 动态加载地图 SDK，支持卫星图、路网图切换。
- 🧠 **AI 农田分割**：通过开发服务器代理调用百度 AI 开放平台的自定义分割模型（`DIKUAIV2`），识别作物与裸土区域。
- 📐 **边界计算**：
  - **凸包**（Convex Hull）—— 快速生成外接多边形。
  - **凹包**（Concave Hull）—— 更精确地贴合不规则地块边界。
- 📸 **截图工具**：基于 `html2canvas` 实现地图视图的一键截图，便于结果存档。
- ⚙️ **Vuex 状态管理**：统一管理地图实例、识别结果等全局状态。

## 🛠 技术栈

| 类别 | 技术 |
|------|------|
| 前端框架 | Vue 2.6 + Vuex 3.4 |
| 地图引擎 | 高德地图 JSAPI 2.0 (Loader 方式) |
| AI 能力 | 百度 AI 开放平台 - 定制图像分割 |
| 几何计算 | concaveman、monotone-convex-hull-2d |
| 图像处理 | html2canvas |
| 构建工具 | Vue CLI 4.5 + Webpack 4 |
| 代码规范 | ESLint + babel-eslint |

## 📁 项目结构
.
├── public/ # 静态资源
├── src/
│ ├── components/ # Vue 组件（地图容器、控制面板等）
│ ├── store/ # Vuex 状态管理（地图实例、识别结果）
│ ├── App.vue # 根组件
│ └── main.js # 入口文件
├── .eslintrc.js # ESLint 配置（已声明 AMap 全局变量）
├── babel.config.js # Babel 配置
├── vue.config.js # Vue CLI 配置（含开发代理）
├── package.json # 依赖清单
└── README.md # 项目说明

## 🚀 快速开始

### 环境要求

- Node.js 10.x 及以上
- npm 或 yarn

### 安装依赖

```bash
# 克隆仓库
git clone https://github.com/your-username/AgriField-Mapper.git
cd AgriField-Mapper

# 安装依赖包
npm install
开发模式
bash
npm run serve
启动后访问 http://localhost:6200（端口可在 vue.config.js 中修改）。

生产构建
bash
npm run build
构建产物输出至 dist/ 目录。

代码检查
bash
npm run lint
⚙️ 关键配置说明
高德地图全局变量
.eslintrc.js 中已声明 AMap 相关全局变量，避免 ESLint 报错：

js
globals: {
  AMap: true,
  AMapUI: true,
  Loca: true
}
百度 AI 代理配置
为避免跨域问题，开发环境下通过 vue.config.js 中的 devServer.proxy 将请求转发至百度 AI 服务：

js
proxy: {
  '/access': {
    target: 'https://aip.baidubce.com/oauth/2.0',
    changeOrigin: true,
    pathRewrite: { '^/access': '' }
  },
  '/api': {
    target: 'https://aip.baidubce.com/rpc/2.0/ai_custom/v1/segmentation/DIKUAIV2',
    changeOrigin: true,
    pathRewrite: { '^/api': '' }
  }
}
⚠️ 注意：实际部署时需替换为你的百度 AI 模型 ID 及 API Key / Secret Key 鉴权逻辑。

📦 核心依赖说明
包名	用途
@amap/amap-jsapi-loader	动态加载高德地图 JS SDK
vue / vuex	渐进式框架与状态管理
concaveman	计算点集的凹包（Concave Hull）
monotone-convex-hull-2d	计算点集的凸包（Convex Hull）
html2canvas	将地图容器转为图片，实现截图功能
core-js	为旧浏览器提供 Polyfill
🎓 学术背景
本项目为学位论文的工程实现部分，旨在探索计算机视觉与地理信息系统在精准农业中的应用。系统通过深度学习分割模型与计算几何算法的结合，降低了植保无人机作业地块测绘的人力成本，为后续航线自动规划提供了可靠的数据基础。

📄 许可证
本项目仅用于学术研究与论文展示。如需商用或二次开发，请遵守相关依赖的开源协议并咨询作者。
