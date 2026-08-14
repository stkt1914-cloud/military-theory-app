# 军事理论学习 App（PWA）

一个专为 **iPhone / iPad（也支持 Android）** 优化的**大学《军事理论》系统学习应用**：
**12 章精讲 + 96 道练习题**，支持**离线使用**、**学习进度追踪**、**暗色模式**。

> 无需 Mac、无需 Xcode、无需上架 App Store：用 Safari 打开后"添加到主屏幕"即可像原生 App 一样全屏使用，**断网也能学**。

## ✨ 功能

| 功能 | 说明 |
| --- | --- |
| 📖 系统课程 | 12 章：中国国防 → 国家安全 → 军事思想 → 国际战略环境 → 现代战争 → 信息化装备 → 军兵种知识 → 条令条例与队列 → 轻武器与射击 → 军事地形学 → 综合训练与防护 → 国防教育与大学生使命 |
| ✍️ 要点归纳 | 每章多个"要点/流程/口诀"块（国防动员、队列要领、射击流程、按图行进等），一键复制 |
| ✏️ 习题练习 | 单选 / 多选 / 材料分析 / 填空四种题型，答完立即给解析，章节练习分数自动记录 |
| 📊 学习进度 | 已完成章节、总进度环、每章最佳练习分数 |
| 🔍 全局搜索 | 搜索章节标题、知识点、要点 |
| 🌙 暗色模式 | 跟随系统 / 手动切换 |
| 📴 离线可用 | Service Worker 预缓存全部资源，断网也能学 |

## 📱 在线使用（推荐：GitHub Pages 部署后）

部署完成后，用 iPhone **Safari** 打开 HTTPS 网址：
1. **保持联网完整浏览一遍**（自动把 12 章内容缓存进手机）；
2. 点 Safari **分享 → 添加到主屏幕**；
3. 打开「设置 → 飞行模式」验证：**断网后 App 照常可用** ✅

## 🖥 本地预览（电脑上）

```bash
cd military-theory-app
node server.js          # 或 powershell -ExecutionPolicy Bypass -File serve.ps1
# 电脑浏览器打开 http://localhost:8000
```

> ⚠️ 局域网 HTTP 地址不支持 iOS 离线缓存；要断网可用必须部署到 HTTPS（GitHub Pages / Netlify）。

## 🚀 部署到公网（免费 HTTPS，支持离线）

- **GitHub Pages**：把 `military-theory-app` 文件夹推到新仓库 → Settings → Pages → Deploy from branch（main / root）。
- **Netlify Drop**：打开 app.netlify.com/drop，直接把文件夹拖进去。

## 📂 目录结构

```
military-theory-app/
├── index.html            # 入口页面
├── manifest.webmanifest  # PWA 清单
├── sw.js                 # Service Worker（离线缓存）
├── css/app.css           # 全部样式（iOS 风格、暗色主题）
├── js/
│   ├── app.js            # 应用逻辑（路由/阅读/练习/进度/搜索）
│   └── highlight.js      # 要点块语法高亮
├── data/
│   └── chapter-01.js … chapter-12.js   # 章节内容与习题数据
├── icons/                # App 图标（180/192/512）
└── docs/
    ├── CONTENT_SPEC.md   # 章节数据格式规范（含内容合规要求）
    └── validate.js       # 数据校验脚本（node docs/validate.js）
```

## ✍️ 自定义 / 增删章节

1. 按 `docs/CONTENT_SPEC.md` 的格式新建 `data/chapter-XX.js`（参考 `data/chapter-01.js` 写法）。
2. 在 `index.html` 和 `sw.js` 的预缓存列表中加入该文件。
3. 运行 `node docs/validate.js` 校验。

## 🔧 技术要点

- 纯原生 HTML/CSS/JS，**零依赖、零 CDN**，完全离线可用。
- 进度与成绩保存在 `localStorage`（键 `military.v1`）。
- Service Worker 采用"导航请求网络优先、静态资源缓存优先"策略，更新后刷新页面即可生效。
