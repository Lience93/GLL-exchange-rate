# 咕辘辘跨境电商汇率换算工具 — 双工具协作约定

本仓库由 **Codex** 与 **DSH** 两个 AI 工具并行维护，采用 git 分支隔离，禁止直接互相覆盖文件。

## 项目边界

- **GitHub 仓库只跟踪 `index.html`**（及本协作文档、`.gitignore`）—— 单页网页汇率换算工具，也就是线上网站本身。
- **不要提交其他文件**：`index.html` 是唯一产物，改动只应发生在它身上。
- **旧的 Chrome 插件已弃用**：插件文件（`background.js`、`content.js`、`popup.*`、`lib.js`、`manifest.json`、`images/`、zip）已归档到 `~/Documents/汇率换算/_archive-plugin/`，被 `.gitignore` 排除，不再维护、不推送、不要修改。

## 分支与工作区

| 工具 | 分支 | 工作目录 |
|---|---|---|
| Codex | `main` | `~/Documents/汇率换算`（主目录） |
| DSH | `dsh-work` | `~/Documents/汇率换算-dsh`（git worktree） |

- Codex 只在主目录、`main` 分支上修改。
- DSH 只在 `~/Documents/汇率换算-dsh`、`dsh-work` 分支上修改。
- 任何一方都不要在对方的目录里改文件，也不要动对方的分支（合并除外）。

## 协作规则

1. **高频提交**：每完成一个可描述的小改动就 commit（如 `fix: 支持 XXX`），不要让工作区长期处于未提交状态。
2. **改文件前先同步**：动手前若对方分支有新增提交，先合并对方分支再改，避免基于过期代码修改：
   - Codex 侧：`git merge dsh-work`
   - DSH 侧：`git -C ~/Documents/汇率换算-dsh merge main`
3. **避免同时改同一文件**：改动前先看对方分支是否动过该文件。若确实要改同一文件，先完成第 2 步的合并，改完立即提交。
4. **合并方向**：需要合入主分支时，在主目录执行 `git merge dsh-work`，解决冲突后提交到 `main`，再 `git push origin main` 同步到 GitHub。

## 部署上线流程（每次改动后执行）

GitHub Pages 网站（https://lience93.github.io/GLL-exchange-rate/ ）从 **`main` 分支**构建，只推送 `dsh-work` 不会上线。任何一方的改动要上线，必须完成以下步骤（DSH 已验证完整链路，Codex 或 DSH 做均可）：

1. **提交并推送**：在各自分支 commit 后 `git push origin <分支名>`。
2. **合并到 main**：在 `~/Documents/汇率换算`（main 工作区）执行 `git merge dsh-work`（或反向），解决冲突后 `git push origin main`。
3. **等待 Pages 构建**：GitHub 自动构建，约 1-2 分钟。可用 API 查询状态：
   `curl -H "Authorization: Bearer <PAT>" https://api.github.com/repos/Lience93/GLL-exchange-rate/pages/builds/latest`
   状态为 `built` 即完成。
4. **验证上线**：抓取 https://lience93.github.io/GLL-exchange-rate/ ，确认新内容已生效（注意浏览器缓存，必要时强制刷新 `Cmd+Shift+R`）。

## 项目速览

`index.html` — 咕辘辘跨境电商汇率换算工具（单文件、内联样式与脚本）：

- **8 种货币**：人民币 CNY、美元 USD、韩元 KRW、墨西哥比索 MXN、巴西雷亚尔 BRL、阿根廷比索 ARS、日元 JPY、俄罗斯卢布 RUB（选择页最多勾选 4 种）
- **实时汇率**：`api.exchangerate-api.com/v4/latest/CNY`，每 5 分钟刷新，汇率条显示 4 位小数（日元/韩元金额不带小数）
- **主表**：人民币 50~1000 元换算成所选货币；点击任意金额打开兑换弹窗（金额输入框点击自动全选）
- **走势图**：选择页每种货币（人民币除外）的 📈 图标，查看近半年兑人民币折线图，含最高/最低/涨跌统计与悬停提示
  - 数据源：`api.frankfurter.dev`（欧洲央行参考汇率，每日，覆盖 USD/KRW/MXN/BRL/JPY）
  - 回退源：`@fawazahmed0/currency-api`（jsDelivr CDN，周采样，用于欧洲央行不含的 ARS/RUB）
- **其他**：农历日期、时区显示、计算器
