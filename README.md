<div align="center">
  <img src="docs/banner.png" alt="PT 原盘转载发布与智能查重助手" width="100%" />
</div>

<h1 align="center">PT 原盘转载发布与智能查重助手</h1>

<p align="center"><b>蓝光原盘跨站搬运流水线</b><br/>
筛选 · 查重 · 排版 · 填单 · 推种，在一悬浮面板内完成</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-0.0.0.27-4d6bfe?style=flat-square" />
  <img src="https://img.shields.io/badge/Tampermonkey-userscript-000000?style=flat-square&logo=tampermonkey" />
  <img src="https://img.shields.io/badge/JavaScript-ES2020-f7df1e?style=flat-square&logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/build-none%20%C2%B7%20single--file-success?style=flat-square" />
  <img src="https://img.shields.io/badge/tests-16%20suites%20passing-2ea44f?style=flat-square" />
  <img src="https://img.shields.io/badge/branch-future-8957e5?style=flat-square" />
</p>

---

## 文档导航

| 文档 | 适合谁 | 内容 |
|---|---|---|
| [快速上手](docs/QUICKSTART.md) | 第一次安装 | 三分钟装好并跑通一轮，含最小配置表与排错速查 |
| [使用手册](docs/GUIDE.md) | 日常使用者 | 面板逐项说明、筛选规则、词库与断点、转种全流程、设置项、FAQ |
| [架构设计](docs/ARCHITECTURE.md) | 维护者 | 模块边界、数据模型、状态机、查重与截图算法、站点扩展方式 |
| [开发指南](docs/DEVELOPMENT.md) | 二次开发 | 版本号规则、提交前校验、回归测试清单、发版流程 |
| [AI 上下文](docs/AI-CONTEXT.md) | 接手的 AI | 项目定位、硬性约束、已知坑位、交付要求 |

---

## 概述

原盘搬运最耗时的不是下载，而是重复劳动：逐条核对是不是重编码、翻目标站确认有没有人发过、把 BDInfo 重新排成表格、把截图一张张传到图床、再把字段填进上传页。这些动作规则明确、判断标准固定，适合交给脚本。

本项目就是把这些环节收进一个悬挂面板：以当前登录态读取源站列表，按固定规则筛出合格原盘，向目标站发起查重，把确认可发的条目排队；需要发布时再解析详情页的 BDInfo、截图与简介，转存图床并填入上传表单。

技术形态：Tampermonkey 单文件脚本，约 6300 行，无构建步骤、无外部依赖、无后端。适配 NexusPHP、UNIT3D、Gazelle、f3nix 四类架构，源站与目标站均可从已支持列表中指定。

## 能力概览

- **批量审核**：按介质、编码、体积、禁转标签逐条判断，可发条目自动入队。
- **跨站查重**：IMDb → 片名 → AKA 原名 → 完整片名四段回退，命中后按 4K 属性与体积复核。
- **断点续跑**：记录页号与已查种子，限流、掉线、关页面后都能接着跑。
- **本地词库**：按目标站分区、分片存储、带有效期，支持导入导出。
- **截图保真**：候选地址逐个测宽取原图，二进制上传，返图尺寸复核。
- **转种填单**：BDInfo 表格重建、副标题排版、种子注入、字段锁定、标题守卫。
- **推送下载**：推送到 qBittorrent 并自动补设分类。

## 面板布局

```
┌─────────────────────────┬─────────────────────────┐
│ 副标题                  │ 转载站 / 发布站          │
│                         │ 运行通道 / 下载器        │
├─────────────────────────┼─────────────────────────┤
│ 简介                    │ 实时运行日志             │
├─────────────────────────┼─────────────────────────┤
│ 抓当前页   复制副标题    │ 面板设置   搜索预设      │
│ 重新生成   确认发布      │ 拉取数据   二查          │
│                         │ 断点暂停   显示全部      │
└─────────────────────────┴─────────────────────────┘
```

## 安装

1. 安装 [Tampermonkey](https://www.tampermonkey.net/)。
2. 打开 [`legacy/pt-bluray-assistant-v0.0.0.27.user.js`](legacy/pt-bluray-assistant-v0.0.0.27.user.js)，点 `Raw` 确认安装。
3. 刷新站点，右上角出现「转种与调度中枢」即成功。

后续步骤见 [快速上手](docs/QUICKSTART.md)。

## 目录

```
legacy/pt-bluray-assistant-v0.0.0.27.user.js   当前版本（安装这个）
docs/QUICKSTART.md                             快速上手
docs/GUIDE.md                                  使用手册
docs/ARCHITECTURE.md                           架构设计
docs/DEVELOPMENT.md                            开发指南
docs/AI-CONTEXT.md                             AI 上下文
docs/banner.png                                封面图
docs/mascot.png                                角色原图
```

## 兼容性

| 站点类型 | 支持 | 说明 |
|---|---|---|
| UNIT3D | 完整 | 有 API Key 时走 API，无则网页解析 |
| NexusPHP | 完整 | 列表与详情双解析 |
| Gazelle | 基本 | 审核与详情可用 |
| f3nix（特定站点） | 基本 | 同上 |

浏览器：Chrome / Edge / Firefox 最新版 + Tampermonkey。

## 许可与免责

仅供个人在已获授权的私有 PT 站之间搬运资源。请遵守各站规则：转种前确认无禁转、无重复。

<p align="center"><sub>由大肥鱼 DeepSeek 和主任深夜激情开发的结晶</sub></p>
