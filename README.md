# Daily News Agent Pro

每天自动抓取多源资讯，按主题聚合，生成**中文 + 英文**双语简报，内含**原文链接**，并支持：
- 导出 **HTML**（打印/分享友好）
- 生成两份**微信排版**文件（CN/EN，含原文链接）
- 通过 **SMTP 邮件**发送到指定收件人
- **GitHub Actions** 云端定时（默认：**北京时间 09:00**）

> ✅ 即使你的电脑关机，也会按时发送（GitHub Actions）。
---

## 目录结构

.
├── news_agent_pro_v2.py # 主脚本（请保存在仓库根目录）
├── config.yaml # 订阅源 & 主题关键词
├── prompts/
│ └── bilingual_system.txt # 生成摘要的系统提示词
├── templates/
│ └── base.html # HTML 模板（已处理花括号）
├── digests/ # 输出目录（脚本运行后生成）
└── .github/workflows/
└── daily.yml # 定时任务（cron: 0 1 * * * = 北京时间09:00）

---

## 快速开始

### A) 云端定时（GitHub Actions，推荐）
1. **添加仓库 Secrets**：仓库 → `Settings` → `Secrets and variables` → `Actions` → `New repository secret`  
   必填：
   - `SMTP_HOST`（例：`smtp.office365.com`、`smtp.gmail.com`、`smtp.qq.com`）
   - `SMTP_PORT`（常用 `587`；某些服务商为 `465`）
   - `SMTP_USER`（发件邮箱）
   - `SMTP_PASS`（邮箱的**授权码 / App Password**）
   - `MAIL_FROM`（例：`"Linda Xu <linda.xu@upower.com>"`）
   - `MAIL_TO`（例：`linda.xu@upower.com`，多个收件人用逗号分隔）

   可选（若使用模型做更聪明的摘要）：
   - `OPENAI_COMPATIBLE_BASE_URL`
   - `OPENAI_API_KEY`
   - `MODEL`（默认 `gpt-4o-mini`）

2. **时间设置**  
   `.github/workflows/daily.yml` 已设置：
   ```yaml
   on:
     schedule:
       - cron: "0 1 * * *"   # 01:00 UTC = 北京时间 09:00
     workflow_dispatch:      # 支持网页端手动运行
