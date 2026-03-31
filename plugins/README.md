# Plugins and Marketplaces

Plugins 可以为 Claude Code 扩展新的工具和能力。本文只讲安装；如果想了解何时使用、为什么使用，请看[完整文章](https://x.com/affaanmustafa/status/2012378465664745795)。

---

## Marketplaces

Marketplace 是可安装插件的仓库集合。

### 添加 Marketplace

```bash
# 添加 Anthropic 官方 marketplace
claude plugin marketplace add https://github.com/anthropics/claude-plugins-official

# 添加社区 marketplace
claude plugin marketplace add https://github.com/mixedbread-ai/mgrep
```

### 推荐 Marketplaces

| Marketplace | 来源 |
|-------------|------|
| claude-plugins-official | `anthropics/claude-plugins-official` |
| claude-code-plugins | `anthropics/claude-code` |
| Mixedbread-Grep | `mixedbread-ai/mgrep` |

---

## 安装 Plugins

```bash
# 打开插件浏览器
/plugins

# 或者直接安装
claude plugin install typescript-lsp@claude-plugins-official
```

### 推荐 Plugins

**开发：**
- `typescript-lsp`：TypeScript 智能提示
- `pyright-lsp`：Python 类型检查
- `hookify`：以对话方式创建 hooks
- `code-simplifier`：代码重构

**代码质量：**
- `code-review`：代码审查
- `pr-review-toolkit`：PR 自动化
- `security-guidance`：安全检查

**搜索：**
- `mgrep`：增强搜索（比 ripgrep 更强）
- `context7`：实时文档查询

**工作流：**
- `commit-commands`：Git 工作流
- `frontend-design`：UI 模式
- `feature-dev`：功能开发

---

## 快速设置

```bash
# 添加 marketplaces
claude plugin marketplace add https://github.com/anthropics/claude-plugins-official
claude plugin marketplace add https://github.com/mixedbread-ai/mgrep

# 打开 /plugins 并安装所需插件
```

---

## 插件文件位置

```
~/.claude/plugins/
|-- cache/                    # 下载的插件
|-- installed_plugins.json    # 已安装列表
|-- known_marketplaces.json   # 已添加的 marketplaces
|-- marketplaces/             # Marketplace 数据
```
