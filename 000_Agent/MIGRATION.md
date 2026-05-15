# AI 大腦遷移手冊

> 這份文件是 pro-kit 07 生成，記錄你的 AI 分身架構。
> 未來換新電腦、換新 AI 時，照這份走就能一鍵接管。

## 當前架構

- 母體資料夾：`C:\Users\will7\Raymond-Agent\`
- 同步管道：GitHub 私有 repo（無雲端即時同步）
- GitHub repo：https://github.com/billsmile/bill-hahahouse
- 體檢腳本：`000_Agent/scripts/sync-health.sh`
- 檢查頻率：每週一次（週五複盤日）

## 情境 1：換一台新電腦（Windows）

1. 安裝 Git、Claude Code
2. 開啟開發者模式（設定 → 系統 → 開發人員專用）
3. clone repo：
   ```powershell
   git clone https://github.com/billsmile/bill-hahahouse C:\Users\你的帳號\Raymond-Agent
   ```
4. 建立 symlinks（在 PowerShell 以管理員執行，或開啟開發者模式後用一般權限）：
   ```powershell
   $mother = "$env:USERPROFILE\Raymond-Agent\.claude"
   $claude = "$env:USERPROFILE\.claude"
   New-Item -ItemType SymbolicLink -Path "$claude\settings.json" -Target "$mother\settings.json"
   New-Item -ItemType SymbolicLink -Path "$claude\CLAUDE.md"     -Target "$mother\CLAUDE.md"
   New-Item -ItemType SymbolicLink -Path "$claude\hooks"         -Target "$mother\hooks"
   New-Item -ItemType SymbolicLink -Path "$claude\skills"        -Target "$env:USERPROFILE\000_Agent\skills"
   ```
5. 跑 `bash 000_Agent/scripts/sync-health.sh` 驗證

## 情境 2：換新 AI 大腦（Codex / Gemini / 未來新產品）

1. 確認新 AI 的規則檔命名慣例（例如 Codex 讀 `AGENTS.md`）
2. 建立對應 symlink：
   ```bash
   ln -s "$HOME/Raymond-Agent/CLAUDE.md" "$HOME/Raymond-Agent/AGENTS.md"
   ```
3. skills / memory 邏輯需要新 AI 支援同等機制才能完整復用

## 情境 3：備份還原

如果 07 跑出事，從備份還原：

```powershell
Remove-Item -Recurse -Force "$env:USERPROFILE\.claude"
Rename-Item "$env:USERPROFILE\claude-backup-20260510-133443" ".claude"
```

然後重新規劃再跑一次 pro-kit 07。

## 定期維護

- **每週五**：跑 `bash Raymond-Agent/000_Agent/scripts/sync-health.sh`
- **有改動時**：`cd Raymond-Agent && git add . && git commit -m "update: 說明改了什麼"`
- **推到 GitHub**：`git push origin main`
