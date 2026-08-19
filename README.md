# Stock Research & Monitoring — Data Repo

This repo is the persistent state store for a set of scheduled Claude Code cloud
routines that research stocks, monitor a watchlist/portfolio, and email alerts
when something materially significant happens.

**This is informational research, not personalized financial advice.** Reports
flag information and let you decide — they never issue buy/sell directives.

## Structure

- `data/watchlist.json` — stocks the user asked to research/monitor.
  `status: "deep_dive"` = inside the first 24h research window since being added.
  `status: "monitoring"` = ongoing hourly significance checks.
- `data/portfolio.json` — holdings the user has shared, for sell-relevant flagging.
- `data/alerts_log.json` — every alert already sent, for dedup + audit trail.
- `reports/` — dated markdown research reports (deep-dives, daily market scans).

## Routines reading/writing this repo

1. **Hourly Watchlist & Portfolio Monitor** — checks for market-moving news on
   anything in `watchlist.json` or `portfolio.json`, only during actual market
   hours, only alerts on things judged genuinely significant.
2. **Daily Market Opportunity Scan** — once each weekday, produces a report
   with one blue-chip pick, one ETF, one higher-risk pick.

Both routines: pull latest, do their work, append to logs, commit, push, and
redeploy the dashboard Artifact.
