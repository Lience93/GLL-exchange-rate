# 咕辘辘跨境电商汇率换算工具 — 双工具协作约定

本仓库由 **Codex** 与 **DSH** 两个 AI 工具并行维护，采用 git 分支隔离，禁止直接互相覆盖文件。

## 项目边界（重要）

- **GitHub 仓库只跟踪 `index.html`** —— 单页网页汇率换算工具。
- **Chrome 插件文件（`background.js`、`content.css`、`content.js`、`lib.js`、`manifest.json`、`popup.html`、`popup.js`、`images/`、zip）是本地测试版本**，已被 `.gitignore` 排除，不会推送到 GitHub。插件只存在于 `~/Documents/汇率换算` 主工作区，`~/Documents/汇率换算-dsh` 工作区没有这些文件。
- 任何提交只应涉及 `index.html`（及协作文档），不要 `git add` 插件文件。

## 分支与工作区

| 工具 | 分支 | 工作目录 |
|---|---|---|
| Codex | `main` | `~/Documents/汇率换算`（本目录） |
| DSH | `dsh-work` | `~/Documents/汇率换算-dsh`（git worktree） |

- Codex 只在本目录、`main` 分支上修改。
- DSH 只在 `~/Documents/汇率换算-dsh`、`dsh-work` 分支上修改。
- 任何一方都不要在对方的目录里改文件，也不要动对方的分支（合并除外）。

## 协作规则

1. **高频提交**：每完成一个可描述的小改动就 commit（如 `fix: 支持 XXX`），不要让工作区长期处于未提交状态。
2. **改文件前先同步**：动手前若对方分支有新增提交，先合并对方分支再改，避免基于过期代码修改：
   - Codex 侧：`git merge dsh-work`
   - DSH 侧：`git -C ~/Documents/汇率换算-dsh merge main`
3. **避免同时改同一文件**：改动前先看对方分支是否动过该文件。若确实要改同一文件，先完成第 2 步的合并，改完立即提交。
4. **合并方向**：需要合入主分支时，在 Codex 侧执行 `git merge dsh-work`，解决冲突后提交到 `main`，再 `git push origin main` 同步到 GitHub。

## 项目速览

`index.html` — 咕辘辘跨境电商汇率换算工具：支持人民币/美元/韩元/墨西哥比索/巴西雷亚尔/阿根廷比索换算，含农历日期、计算器、币种选择页等（单文件、内联样式与脚本）。
