<div align="center">

# [网腾无限AI - 早期教育与亲子启蒙专家]

**[一个支持早教智慧积木打卡与五种特色早教流派的早期教育与亲子启蒙工具，具备深色玻璃拟态自适应交互与微信端 H5 体验]**

[Vue 3] · [TypeScript] · [Vite] · [Vanilla CSS] · [开源协议 MIT]

[![GitHub stars](https://img.shields.io/github/stars/WT-Agent/ai-youjiao?style=social)](https://github.com/WT-Agent/ai-youjiao)
[![GitHub license](https://img.shields.io/github/license/WT-Agent/ai-youjiao)](https://github.com/WT-Agent/ai-youjiao/blob/main/LICENSE)

[在线演示](#在线演示) · [快速启动](#快速启动) · [参与贡献](#参与贡献) · [支持一下](#支持一下)

</div>

## 关于我们

团队成员均来自 C9 等顶尖学府，在字节、腾讯、阿里的工程师组成，全职创业研发开源 AI 应用产品，让所有人感受 AI 的魅力。

本项目旨在为语言发音、大运动体能、感官统合、情绪社交及全脑逻辑群体提供高品质的早期教育与亲子启蒙服务。用户只需输入宝宝月龄与启蒙诉求，AI 即可根据多维科学度看板自动输出月龄发育特征与启蒙目标、亲子互动游戏与DIY教具、行为习惯与感统训练建议及观察记录与发育评估指导。页面内置了支持木质积木搭塔音效的“智慧积木”印章，供用户在早教体验时增加仪式感。

**我们不搞概念，不卖课，只写能跑起来的代码。**

欢迎 Star、Fork、提 Issue，一起让这个项目变得更好用。

核心特性：
- **极简自适应交互**：提供毛玻璃质感的深色玻璃拟态自适应 Web 界面，高度适配移动端 H5 微信浏览器与 PC 体验。
- **早教智慧积木打卡印章 (Building Block Stamp)**：基于前端 Web Audio API 动态合成木质积木敲击与搭积木连音声效，点击印章即可累积早教打卡次数并伴随渐隐上升动画。
- **五大早教流派**：
  - **蒙台梭利感官探索与自理流**：强调教具感官操作、自主纠错、精细动作。
  - **游戏化亲子互动与语言启蒙流**：以故事、儿歌、角色扮演促语言爆发。
  - **感觉统合与大运动体能流**：专注前庭觉、本体觉与触觉训练，提升平衡。
  - **情绪管理与社交性格培育流**：注重共情、情绪识别、同伴分享规则。
  - **启发式全脑开发与数学思维流**：通过形状、分类、排序与空间探索启发数理。
- **AI 早教质量看板**：自动提取 AI 回复中的共识数据，以简洁的单轨进度条在前端直观展示发育契合度、亲子互动度、感官统合度、习惯养成度及趣味吸引度。
- **演示案例与分享卡片**：内置 30 条不同主题的精选早教启蒙精彩演示样例，并支持一键卡片化截图分享。
- **一键零成本部署**：纯前端静态网页结构，支持零成本部署于 Vercel、GitHub Pages 或 CDN/OSS 静态托管服务。
- **安全开发代理**：本地开发支持使用个人 API 密钥发起代理请求，密钥由 Vite 服务器中转，无需担心前端泄露。
- **裂变解锁与留存**：内置微信朋友圈扫码分享拦截与额度重置机制，提升流量转化与留存。

## 快速启动

### 1. 克隆项目
```bash
git clone https://github.com/WT-Agent/ai-youjiao.git
cd ai-youjiao
```

### 2. 安装依赖
项目强制使用 pnpm 作为包管理器：
```bash
pnpm install
```

### 3. 配置本地开发环境变量
复制并修改环境变量配置文件：
```bash
cp .env.example .env
```
根据微应用的功能类型，在 `.env` 中配置您的开发者密钥：
- `DEEPSEEK_API_KEY`: 您的 DeepSeek 开发者 API 密钥（用于文本生成任务）
- `DASHSCOPE_API_KEY`: 您的通义千问/通义万相开发者 API 密钥（用于多模态与生图任务）

### 4. 启动本地开发服务
```bash
pnpm dev
```
启动成功后在浏览器访问控制台输出的地址即可。

### 5. 生产构建打包
```bash
pnpm build
```
打包后生成的 `dist` 目录即为纯静态网页资源，可直接上传部署。

## 脚手架集成说明

本模板由私有总控仓库 `ai.wuxian.xyz` 中的 `@wuxian/cli` 脚手架统一管理，支持以下批量运维操作：

### 初始化或更新单个子项目

```bash
node bin/cli.js ai-youjiao
```

脚手架将自动：
1. 读取子仓库的 `README.md` 首行作为 Prompt 主题。
2. 注入 Vue 3 静态页面结构及标准配置文件。
3. 保留原有的 `.git` 配置与 `README.md`，不覆盖个性化内容。

### 批量同步所有子项目

```bash
node bin/cli.js all
```

将模板 the latest 变更（如 SSO 逻辑、额度控制）一键同步至全部 31 个子项目。

### Agent 配置维护接口

```bash
# 读取子项目配置
node bin/cli.js get ai-youjiao

# 写入/更新配置（支持热更新 prompt、model、title、temperature 等）
node bin/cli.js set ai-youjiao prompt "你是一位资深早教育儿专家、0-6岁儿童全脑开发..."
node bin/cli.js set ai-youjiao model deepseek-chat
```

## 联系方式

- GitHub Issues: [提交反馈](https://github.com/WT-Agent/ai-youjiao/issues)
- 邮箱: us@wuxian.xyz

## 打赏支持

如果本项目对您有帮助，欢迎请作者喝杯咖啡。您的支持是持续维护与更新的动力。

<div align="center">

**微信支付** | **支付宝**
:---:|:---:
<img src="./asset/tenpay.png" width="200" alt="微信支付"> | <img src="./asset/alipay.png" width="200" alt="支付宝">

</div>

## 版权与许可

本项目基于 MIT License 开源协议。

Copyright (c) 2026. All rights reserved.
