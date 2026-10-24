# doc-compose（按模板重组文档）

给 agent 用的技能包：你提供**模板文档 A**（定结构）和**内容文档 B**（供内容），agent 把 B 的内容梳理后填入 A 的骨架，生成最终文档。

## 依赖

| 用途 | 工具 | 安装 |
| --- | --- | --- |
| Word/Excel/PDF/PPT → Markdown（必需） | Python ≥ 3.9 + markitdown | `pip install 'markitdown[all]'` |
| Excel 兜底读取 | openpyxl | `pip install openpyxl` |
| 输出 Word/PDF（推荐） | pandoc | Windows：`winget install JohnPaddock.pandoc`；macOS：`brew install pandoc`；Linux：包管理器安装 |

Agent 开跑前会自检这些工具，缺失时会给出对应安装命令，不会瞎转。

## 安装

把整个 `doc-compose/` 文件夹复制到你的 agent 的 skills 目录：

| Agent | 路径 |
| --- | --- |
| pi | `~/.agents/skills/doc-compose/` |
| Claude Code | `~/.claude/skills/doc-compose/`（项目级：`<项目>/.claude/skills/`） |
| Codex | `~/.codex/skills/doc-compose/` |

复制完即生效，无需其它注册步骤。

## 使用

直接给 agent 两个文件路径并说明意图即可，例如：

> 按模板 A 整理文档 B，生成最终文档
> A: D:\工作\报告模板.docx　B: D:\工作\0715原始纪要.docx、B2.xlsx

执行过程中的交互节点：
1. 转换格式后解析 A 的大标题（= 待填充部分清单）
2. 把 B 的内容映射填入，缺的标 `【待补充】`，装不下的进附录
3. **输出前给你过目确认**（草稿 + 来源对照表 + 待补充清单）
4. 你选**语言**（单中文 / 单英文 / 中英双语）和**输出格式**（docx / pdf / 其它）
5. 生成最终文件

## 文件结构

```
doc-compose/
└── SKILL.md    # 全部指令（agent 读取的入口）
```

## 已知边界

- 扫描件 PDF 无法直接提取文字，agent 会要求你提供可复制版本
- 多份 B 内容冲突时，agent 会列出差异请你裁决，不擅自取舍
