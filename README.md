# leisdeal-free-studio

LEISDEAL 免费 AI 工作室：免费档工具的每日保活与状态汇总。

- 定时任务：`.github/workflows/daily-keepalive.yml`，每天 01:30 UTC 运行一次（美西时间前一日 17:30 PST / 18:30 PDT），也可手动触发（Actions → Daily keepalive + summary → Run workflow）。
- 保活工具：Supabase（项目 URL 存于仓库 Actions secret `SUPABASE_URL`，未设置时只记一条警告，不报错）。
- 每日汇总：`logs/YYYY-MM-DD.md`，记录日期与各工具保活状态。

公开仓库，不存放任何密钥、密码或个人信息。密钥只走仓库 secret，不写进代码；不绑卡、不产生费用。
