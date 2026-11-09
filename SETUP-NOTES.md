# leisdeal-free-studio 接通备忘（2026-11-09）

## 状态：仓库未建成，workflow 文件已备好待上传

## 卡点（已实测）

- GitHub 连接器 `create_repository` 返回 403 `Resource not accessible by integration`：
  GitHub App token 不能在个人账号下建库（`POST /user/repos` 需要用户 OAuth token）。
- 本机 `gh` CLI 未登录，环境里也没有 `GH_TOKEN` / `GITHUB_TOKEN`。
- 仓库名已核验可用：全站搜索 `leisdeal-free-studio` 结果为 0。

## 已备好的文件（本目录）

- `daily-keepalive.yml` — workflow 正文，YAML 已校验通过（cron `30 1 * * *`，每天 01:30 UTC；
  含 workflow_dispatch；Supabase 未配 secret 时只记警告、不报错；每日汇总写 `logs/YYYY-MM-DD.md` 并提交回库）。
- `README.md` — 仓库说明。

## 接通步骤（二选一建库）

1. 父代理开浏览器任务（用户 GitHub 已登录会话）：github.com → New repository →
   名 `leisdeal-free-studio`，Public，勾选 Add a README → Create。
   或用户本人在网页上手动建，1 分钟。
2. 库建好后，用 GitHub 连接器 `create_or_update_file` 把本目录两个文件传上去：
   - `.github/workflows/daily-keepalive.yml`（内容 = `daily-keepalive.yml`）
   - `README.md`（内容 = `README.md`）
   分支 `main`。注意：若 Muse GitHub App 是按“指定仓库”安装的，需先在
   github.com/settings/installations 里把新库加进授权范围，否则写文件会 403。
3. 手动触发：连接器没有 workflow_dispatch 工具，须在仓库 Actions 页点
   “Daily keepalive + summary → Run workflow”，或登录后的 `gh workflow run`。
   首次手动跑通后，在 Actions 日志里确认 Supabase 一行是 `not_configured` 警告、
   `logs/<当日>.md` 已提交回库。
4. 定时：建库当天起，每天 01:30 UTC 自动跑（美西前一日 17:30 PST / 18:30 PDT）。
   若 2026-11-09 当天建库，首次定时运行为 2026-11-10 01:30 UTC（11-09 17:30 PST）。

## 待填 secret（Supabase URL 确定后）

- 仓库 Settings → Secrets and variables → Actions → New repository secret，
  名 `SUPABASE_URL`，值填 Supabase 项目 URL。未填之前 workflow 每次只记警告、正常退出。
