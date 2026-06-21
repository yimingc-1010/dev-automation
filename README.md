# dev-automation

GitHub-driven 自治開發流程平台 — 把接單員(claude-code-action)與傳令兵(Telegram notify)集中管理成 reusable workflow,各產品 repo 只放薄貼紙即可一鍵接入。

## 包含什麼

- `docs/superpowers/specs/` — 平台設計 spec(目前只有 1 份)
- `docs/superpowers/specs/assets/` — spec 內 3 段 Mermaid 圖的預渲染 SVG + PNG(用 `mermaid-config.json` + `puppeteer-config.json` 指定 Heiti TC 字型)
- (未來)`docs/disaster-recovery.md` — Phase 0 退場機制
- (未來)`.github/workflows/` — claude.reusable.yml、notify.reusable.yml(Phase 2 實作)
- (未來)`starter-workflows/` — Starter Workflow 範本(Phase 3 實作)

## 重渲染 Mermaid 圖

```bash
cd docs/superpowers/specs/assets
for f in *.mmd; do
  npx --yes -p @mermaid-js/mermaid-cli@10.9.0 mmdc \
    -i "$f" -o "${f%.mmd}.svg" -p puppeteer-config.json -C mermaid-config.json
  npx --yes -p @mermaid-js/mermaid-cli@10.9.0 mmdc \
    -i "$f" -o "${f%.mmd}.png" -p puppeteer-config.json -C mermaid-config.json \
    -w 1600 -H 1200 -b "#0d1117" --scale 2
done
```

需要 `@mermaid-js/mermaid-cli@10.9.0` 與 macOS Heiti TC 字型(預設有)。

## 對應規格

最新 spec: [`docs/superpowers/specs/2026-06-21-dev-automation-platform-design.md`](docs/superpowers/specs/2026-06-21-dev-automation-platform-design.md)

## 帳號類型

此 repo 在 **個人 GitHub 帳號**(`yimingc-1010`)下,所以無法用 Org Secret。每個接入此平台的產品 repo 需自行設定:

- `TELEGRAM_BOT_TOKEN`(repo-level secret)
- `CLAUDE_CODE_OAUTH_TOKEN`(repo-level secret)

詳見 spec 內「技術限制與取捨」表第 3 條。

## 演進階段

```
Phase 0  災難復原文件 (退場機制)
Phase 1  home-splat 單 repo 驗證
Phase 2  抽出 reusable workflow 進此 repo
Phase 3  .github repo + Starter Workflows 推廣
```

完整時程與驗收條件見 spec。
