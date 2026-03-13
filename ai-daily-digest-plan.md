# AI Daily Digest - 自动化系统设计

## 目标

每日自动收集优质 AI 创作者的推文，整理成报告，提交到 GitHub 仓库。

## 数据源

X (Twitter) 推文：https://x.com/AI_Jasonyu/status/2030166779096658161
- 包含多位 AI 领域优质创作者列表
- 需要定期更新创作者列表

## 系统架构

```
┌─────────────────┐
│  定时触发器     │  (每日 UTC 00:00 / 北京时间 08:00)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  数据收集脚本   │
│  - 读取创作者列表│
│  - 获取当天推文  │
│  - 过滤和清洗   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  报告生成       │
│  - Markdown格式 │
│  - 分类整理     │
│  - 添加元数据   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Git 提交       │
│  - 创建分支     │
│  - Commit      │
│  - Push        │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  通知发送       │
│  - Telegram 通知│
│  - 报告摘要     │
└─────────────────┘
```

## 技术栈

### 1. 数据收集
- **X API / Web 抓取**
  - 使用 `xurl` skill (如果有 API access)
  - 或 `baoyu-danger-x-to-markdown` (逆向 API)
- **创作者列表存储**
  - JSON 配置文件：`creators.json`

### 2. 定时任务
- **OpenClaw Cron**
  - 使用 `openclaw cron` 命令
  - 配置文件：`~/.openclaw/cron.yaml`
- **或系统 Cron**
  - macOS: `crontab -e`
  - 添加：`0 8 * * * /path/to/script.sh`

### 3. 报告格式

```markdown
# AI Daily Digest - YYYY-MM-DD

> 每日精选 AI 领域创作者的优质内容

## 📊 今日概览
- 收集推文数：XX
- 创作者数：XX
- 主要话题：[话题1, 话题2, ...]

## 🔥 热门内容

### [创作者名称](@username)
**主题：** [主题]
**内容：** [推文内容]
**链接：** [推文链接]
**发布时间：** YYYY-MM-DD HH:MM

---

## 📝 分类内容

### 技术突破
- ...

### 产品发布
- ...

### 行业观点
- ...

### 教程/资源
- ...

## 🔗 创作者列表
- [@username1](链接) - 简介
- [@username2](链接) - 简介
- ...
```

## 仓库结构

```
ai-daily-digest/
├── README.md                    # 仓库介绍
├── creators.json                # 创作者列表配置
├── scripts/
│   ├── collect.ts              # 数据收集脚本
│   ├── generate-report.ts      # 报告生成
│   └── publish.sh              # Git 提交脚本
├── reports/
│   ├── 2026/
│   │   ├── 03/
│   │   │   ├── 2026-03-08.md
│   │   │   ├── 2026-03-09.md
│   │   │   └── ...
│   │   └── ...
│   └── archive/                # 历史归档
├── templates/
│   └── daily-report.md         # 报告模板
└── .github/
    └── workflows/
        └── daily-digest.yml    # GitHub Actions (可选)
```

## creators.json 格式

```json
{
  "version": "1.0",
  "lastUpdated": "2026-03-08",
  "creators": [
    {
      "username": "AIUsername",
      "name": "创作者名称",
      "bio": "简介",
      "category": "researcher|developer|educator|founder",
      "priority": "high|medium|low",
      "active": true
    }
  ],
  "keywords": ["AI", "LLM", "机器学习", "深度学习"],
  "excludeKeywords": ["广告", "推广"]
}
```

## 实现步骤

### Phase 1: 基础设置 (今天)
1. ✅ 获取创作者列表
2. ⏳ 创建 GitHub 仓库
3. ⏳ 设置基本结构
4. ⏳ 配置 Git 认证

### Phase 2: 脚本开发 (明天)
1. 开发数据收集脚本
2. 开发报告生成脚本
3. 测试完整流程

### Phase 3: 自动化 (后天)
1. 设置 OpenClaw cron
2. 配置 Telegram 通知
3. 测试定时任务

### Phase 4: 优化 (持续)
1. 添加内容分类
2. 提升报告质量
3. 添加数据分析
4. 创作者列表维护

## 数据收集策略

### 方案 A: X API (推荐，如果有 access)
```bash
# 使用 xurl skill
xurl GET /2/users/by/username/:username/tweets \
  --query "start_time=2026-03-08T00:00:00Z&end_time=2026-03-08T23:59:59Z"
```

### 方案 B: Web 抓取
```bash
# 使用 baoyu-danger-x-to-markdown
bun scripts/main.ts https://x.com/:username --filter-date today
```

### 方案 C: RSS (部分创作者)
某些创作者可能提供 RSS feed

## 通知格式 (Telegram)

```
📰 AI Daily Digest - 2026-03-08

🔥 今日收集了 XX 条优质推文

📊 主要话题：
• 话题1 (XX条)
• 话题2 (XX条)

🔗 查看完整报告：
https://github.com/[username]/ai-daily-digest/blob/main/reports/2026/03/2026-03-08.md

⏰ 下次更新：明天 08:00
```

## 注意事项

1. **API 限制**
   - X API 有速率限制
   - 需要合理控制请求频率
   - 考虑缓存机制

2. **数据质量**
   - 过滤无关内容
   - 去重
   - 语言检测（保留中英文）

3. **隐私和合规**
   - 不存储敏感信息
   - 遵守 X 的 ToS
   - 标注数据来源

4. **可维护性**
   - 模块化设计
   - 日志记录
   - 错误处理

## 下一步行动

1. **立即：** 登录 GitHub CLI (`gh auth login`)
2. **今晚：** 创建仓库和基本结构
3. **明天：** 开发数据收集脚本
4. **后天：** 设置自动化任务

---

_设计者：Kai_
_日期：2026-03-08_
