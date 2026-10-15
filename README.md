# 智能校对助手 (Obsidian AI-Proofreader)

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Obsidian 中文校对插件，通过 DeepSeek AI 引擎检测文档中的**错别字**、**语法错误**、**标点符号问题**，支持**一键修复**与**实时偏移刷新**。

## 2026-10-15 更新

- 多标签页检测与当前文档绑定，切换文件时侧栏重置，旧请求不会覆盖新文档结果。
- “当前文档”使用适配明暗主题的圆角卡片，仅显示文件名；长文件名省略，悬停查看完整名称。
- AI 使用原文短语与修正建议的 JSON 协议，位置由插件本地校验；不要求模型计算字符下标。
- 空占位记录、格式错误或无法安全定位的结果会重试并明确报错，不再冒充“未发现问题”。
- 侧栏可直接切换快速、标准、深度优化模式，自动保存；检测期间禁用切换，已有结果注明原检测模式。
- 部分条目无效时保留能够精确定位的建议，明确显示“检测不完整”；重试反馈要求保留原文引号、标点和空格，不进行模糊替换。
- 发布文件不包含测试脚本、测试专用导出及个人配置。模型建议仍需人工核对，不能保证不漏报或不误报。

## 功能

- 🔍 **AI 智能检测**：调用 DeepSeek API 进行出版级中文校对
- ✍️ **三种模式**：快速模式（错别字）→ 标准模式（+标点+语法）→ 深度模式（+表达优化）
- 📝 **固定词组词典**：内置常见错词组，支持自定义词典
- 🎯 **一键修复**：点击按钮替换错误，后续高亮实时平移
- 🔄 **实时偏移刷新**：编辑区任意修改后，检测结果自动跟随，无需重新检测
- 🎨 **分类筛选**：按错别字/标点/语法分类查看结果

## 安装

1. 下载 `main.js`、`manifest.json`、`styles.css` 三个文件
2. 放入 Obsidian vault 的 `.obsidian/plugins/obsidian-aiproofreader-plugin/` 目录
3. 重启 Obsidian → 设置 → 第三方插件 → 启用「智能校对助手」

## 配置

打开插件设置，填写：

| 配置项 | 说明 | 示例 |
|--------|------|------|
| **API 密钥** | DeepSeek API Key | `sk-xxxx` |
| **API 地址** | API 端点 | `https://api.deepseek.com/v1/chat/completions` |
| **模型名称** | 模型 ID | `deepseek-v4-flash` |
| **检测模式** | quick / standard / deep | `standard` |
| **最大段长** | 单次 API 调用的最大字符数 | `1500` |

> DeepSeek API Key 可在 [platform.deepseek.com](https://platform.deepseek.com) 获取。

## 使用

1. 打开 Markdown 文件
2. 点击左侧边栏的「拼写检查」图标（或运行命令 `中文校对：检测当前文档`）
3. 右侧面板显示检测结果，按分类筛选
4. 点击「替换」修复单处错误，或点击「全部修复」批量处理
5. 编辑区修改文字后，高亮自动刷新，无需重新检测

## 技术架构

```
CM6 EditorView.updateListener（文档变更）
  ↓ iterChanges → {fromA, toA, diff}
applyChangesToOffsets（几何平移）
  ↓ rEnd<=fromA→不动 | rStart>=toA→+diff | 重叠→移除
debounced renderResults（150ms）
```

- **全局 offset 基准**：零格式假设，对所有 Markdown 变体鲁棒
- **增量平移**：按字符增减量单位移动后续偏移，不涉及 AI 重新检测
- **restoreScroll**：替换后恢复 scrollTop，消除画面跳跃


## 许可

MIT License

## 作者
Tahacoo
