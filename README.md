# daily-stock-pro

缠论（chan.py）云端定时三件套，独立仓库（非 fork），GitHub Actions 定时运行并发邮件。

## 定时任务（北京时间，工作日周一至周五）

| 工作流 | 时间 | 作用 |
|---|---|---|
| `daily_analysis.yml` 缠论日报 | 09:00 / 16:00 | 11股+5指数，算笔/中枢/买卖点/背驰，K线图内嵌发邮件 |
| `news_scan_email.yml` 新闻雷达 | 09:10 / 16:10 | 抓东财全球快讯+财联社电报，分类去重、资金验证，发观察池邮件 |
| `chan_scan.yml` 三买扫描 | 周五 17:00 | 中证500扫描，最新一个买卖点=三类买点(3a/3b)则发邮件 |

## 需要配置的 Secrets（Settings → Secrets and variables → Actions → New repository secret）

只需要 QQ 邮箱三项（其余可选）：

| Secret 名 | 填什么 |
|---|---|
| `EMAIL_SENDER` | 发件邮箱，如 `123456@qq.com` |
| `EMAIL_PASSWORD` | QQ邮箱 SMTP 授权码（设置→账号→POP3/IMAP/SMTP 开启后生成，16位，**不是QQ密码**） |
| `EMAIL_RECEIVERS` | 收件邮箱，多个用英文逗号分隔 |
| `EMAIL_SENDER_NAME` | 发件人显示名（可选，默认"股票分析助手"） |

## 手动触发验证

Actions 页选对应工作流 → `Run workflow` → 跑完去邮箱看。

## 文件结构

```
chan_analysis.py        缠论日报主脚本（11股+5指数）
chan_core.py            取数/缠论引擎封装
chan_scan.py            中证500三买扫描
chanlib/                vendored chan.py 经典缠论框架
news_scanner/           新闻扫描 news_scanner.py + send_email.py
cs500.csv / all_a.csv   股票池清单（内置兜底）
.github/workflows/      三个定时工作流
```

> 免责声明：本项目由公开行情与快讯自动整合生成，仅供研究参考，不构成投资建议。
