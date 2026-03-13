# AI Daily Digest - Cron Configuration

## 任务配置

在 `~/.openclaw/config.yaml` 或 `openclaw.json` 中添加 cron 配置：

```yaml
cron:
  jobs:
    - name: ai-daily-digest
      schedule: "0 8 * * *"  # 每天早上 8:00 (北京时间)
      command: "bash"
      args:
        - "/Users/seanliang/.openclaw/workspace/ai-daily-digest/scripts/daily-digest.sh"
      workdir: "/Users/seanliang/.openclaw/workspace/ai-daily-digest"
      env:
        PATH: "/Users/seanliang/.bun/bin:$PATH"
      enabled: true
      description: "每日 AI 资讯收集和报告生成"
```

或使用 OpenClaw CLI 添加：

```bash
openclaw cron add \
  --name "ai-daily-digest" \
  --schedule "0 8 * * *" \
  --command "bash /Users/seanliang/.openclaw/workspace/ai-daily-digest/scripts/daily-digest.sh" \
  --description "每日 AI 资讯收集"
```

## 时间说明

- `0 8 * * *` = 每天 08:00 (Asia/Shanghai)
- Cron 格式：分 时 日 月 周

## 手动测试

在设置 cron 之前，先手动测试：

```bash
cd /Users/seanliang/.openclaw/workspace/ai-daily-digest
bash scripts/daily-digest.sh
```

## 注意事项

**目前数据收集功能尚未完全实现，需要：**

1. **集成 X API**
   - 方案 A：使用 `xurl` skill (需要 X API 密钥)
   - 方案 B：使用 `baoyu-danger-x-to-markdown` (逆向 API，有风险)

2. **获取创作者列表**
   - 从推文 https://x.com/AI_Jasonyu/status/2030166779096658161 提取
   - 或手动补充到 `creators.json`

3. **完善分类逻辑**
   - 当前是简单关键词匹配
   - 可以接入 LLM 进行智能分类

## 建议的开发流程

### Phase 1: 测试基础功能 (明天)
1. 手动运行数据收集脚本
2. 验证报告生成
3. 测试 Git 提交流程

### Phase 2: 集成 API (后天)
1. 配置 X API 或选择抓取方案
2. 实现真实数据收集
3. 完善错误处理

### Phase 3: 启用自动化 (3-4 天后)
1. 添加 cron 任务
2. 监控运行情况
3. 优化和调整

---

_配置指南由 Kai 创建_
_日期：2026-03-08_
