

---

## 🧠 文章草稿：為什麼要用 Mac 來安裝龍蝦 [2026-02-14]
**來源**：G大 語音口述  
**標題**：為什麼要用 Mac 來安裝龍蝦  
**狀態**：📝 草稿階段

### 核心論點
Mac 是安裝 OpenClaw（龍蝦）的最佳平台，因為它結合了成熟的生態鏈與底層的 Linux 相容性。

### 一、Mac 生態鏈的完整性
Mac 擁有成熟的應用生態，可以同時運行：
- **通訊軟體**：LINE（這是你的主要溝通渠道）
- **設計工具**：Photoshop、Illustrator、SketchUp
- **辦公套裝**：Word、Excel、PowerPoint

**關鍵洞察**：這些軟體在 Linux 上無法原生運行，在雲端也無法完整實現。

### 二、為什麼不用 Linux 直接跑？
如果你選擇純 Linux 環境：
- ❌ LINE 無法安裝
- ❌ Adobe 全家桶無法使用
- ❌ Microsoft Office 無法使用
- ❌ 設計軟體生態缺失

**結論**：Linux 雖然是 OpenClaw 的底層基礎，但作為日常使用平台，生態鏈不完整。

### 三、為什麼不用 Windows？
Windows 雖然可以透過虛擬機（WSL）跑 Linux，但會遇到：
1. **權限問題**：虛擬機與主系統的檔案權限隔離
2. **網路問題**：port forwarding 設定複雜
3. **Telegram 對接問題**：Webhook 與本地網路配置困難
4. **額外維運成本**：虛擬機本身就是一層複雜度

**結論**：Windows + 虛擬機 = 麻煩的組合。

### 四、Mac 的獨特優勢
Mac 完美結合了兩個世界：
- **表面**：成熟的 macOS 生態，所有生產力軟體都能跑
- **底層**：Unix-like 系統，與 OpenClaw 的 Linux 底層無縫對接

**金句**：「Mac 是唯一能同時擁有『設計師的生產力工具』和『工程師的 Linux 底層』的平台。」

### 五、為什麼選擇 Telegram 作為主要溝通渠道？
**核心原則**：OpenClaw 是開源專案，預設對接全部是 Telegram。

**關鍵洞察**：
1. **更新優先**：任何新功能或修復，測試與部署都以 Telegram 為優先
2. **開源調整快**：因為是開源，Telegram 的整合調整速度最快
3. **避免 LINE 風險**：不建議對接 LINE，因為如果 OpenClaw 後來改了某些設定，LINE 的對接可能會失效
4. **Telegram 持續可用**：無論 OpenClaw 怎麼更新，Telegram 的整合一定會持續支援

**結論**：選擇 Telegram 是確保長期穩定性與第一時間獲得功能更新的最佳選擇。

### 六、Mac + Homebrew + Apple 生態鏈的加乘效果
**核心原則**：Mac 不僅是軟體平台，更是完整的智慧生活生態系統。

**1. Homebrew 套件生態**（H-O-M-E-B-R-E-W）
Mac 擁有強大的 Homebrew 套件管理器，幾乎所有好用的開源工具都能一鍵安裝：
- 開發工具：Python、Node.js、Git、Docker
- 系統工具：wget、curl、tree、htop
- AI 工具：whisper-cpp、ollama、openclaw

**關鍵洞察**：「生態圈超級豐富，直接可以拿來使用。」

**2. Apple HomeKit 智慧家庭整合**
Mac 原生支援 Apple 的智慧家庭生態 HomeKit：
- **燈光控制**：不再需要用精準的指令說「開啟客廳燈」，直接說「把客廳弄亮一點」，龍蝦會聰明地理解並執行
- **窗簾控制**：「幫我把餐廳窗簾打開一半」→ AI 自動調整到 50%
- **情境模式**：「我回家了」→ 自動開燈、開冷氣、播放音樂

**3. Apple TV、iMessage、iOS 無縫整合**
- **Apple TV 控制**：直接用語音「播放 Netflix 上的《紙色通緝令》」
- **iMessage 對接**：預設開啟，可以直接透過龍蝦發送 iMessage
- **iOS 連動**：Mac 上設定的自動化，iPhone 上同步執行

**結論**：用 Mac 可以最先體驗到「AI 助理 + Apple 生態」完整對接的好處。

### 下一步行動
- [ ] 補充技術細節（Homebrew、Python、Node 版本要求）
- [ ] 加入安裝步驟大綱
- [ ] 補充「從 Windows 轉移到 Mac」的建議

---

## 📥 外部資源連結

- [Threads - @thomas92tw](https://www.threads.com/@thomas92tw/post/DUuowGMkY7n)
    - **來源**: G大 分享
    - **狀態**: 待內容擷取（Threads 需登入/JS 渲染）
    - **存檔時間**: 2026-02-15 09:10

---

## 📋 OpenClaw 排程機制：Cron vs Heartbeat [2026-02-15]
**來源**：官方文件 https://docs.openclaw.ai/automation/cron-vs-heartbeat
**標籤**：#automation #cron #heartbeat #system-architecture

### 核心區別

| 機制 | 執行方式 | 適用場景 |
|------|---------|---------|
| **Heartbeat** | 主 session，每 30 分鐘 | 批次檢查、上下文感知 |
| **Cron (main)** | 主 session，透過 system-event | 提醒需上下文處理 |
| **Cron (isolated)** | 隔離 session，精確時間 | 獨立任務、不同模型 |

### Heartbeat 適用時機
- **批次檢查**：一次檢查郵件、行事曆、通知等多項任務
- **上下文感知**：能根據近期對話判斷優先級
- **降低 API 成本**：一個 heartbeat 取代多個 cron jobs
- **智慧抑制**：無需處理時回覆 `HEARTBEAT_OK`，不發送訊息

### Cron 適用時機
- **精確時間**：「每週一 9:00」而非「大概 9 點」
- **獨立執行**：不污染主 session 歷史記錄
- **模型切換**：可用更強大（如 opus）或更便宜模型
- **一次性提醒**：`--at "20m"` 精確倒數

### 龍蝦系統現況對照

| 任務 | 機制 | 原因 |
|------|------|------|
| 龍蝦精華協議 | Cron (每 10 分鐘) | 精確監控 Context 水位 |
| X Feed Monitor | Cron (每 4H) | 隔離執行、獨立模型 |
| 配額日報 | Cron (每日 23:59) | 精確時間、隔離執行 |

### 決策流程圖

```
需要精確時間？
  YES → 使用 Cron
  NO → 繼續...

需要隔離主 session？
  YES → 使用 Cron (isolated)
  NO → 繼續...

可批次與其他檢查合併？
  YES → 使用 Heartbeat
  NO → 使用 Cron
```

### 最佳實踐
- **保持 HEARTBEAT.md 精簡**：減少 token 開銷
- **批次相似檢查**：用 heartbeat 取代多個 cron jobs
- **隔離 cron 使用便宜模型**：例行任務不需最高品質
- **兩者並用**：heartbeat 處理日常監控，cron 處理精確排程


---

## 🔧 戰勤官技能清單 (Agent Skills) [2026-02-16]
- **last30days-skill**
  - **來源**: https://github.com/mvanhorn/last30days-skill
  - **狀態**: ✅ 已安裝 (2026-02-16)
  - **核心功能**: 研究過去 30 天內的主題，支援 Reddit, X, YouTube, Web。支援 watchlist 與 briefing 產出。
- **Agent Browser CLI**
  - **來源**: https://github.com/TheSethRose/Agent-Browser-CLI
  - **狀態**: ✅ 已安裝 (2026-02-15)
  - **核心功能**: Rust-based 無頭瀏覽器，支援快照、截圖與自動化填表。


---

## 🛡️ 系統教訓：如何避免 Zombie Sessions 塞滿系統 [2026-02-15]
**來源**：G大 語音口述 + 慘痛教訓總結
**標籤**：#system-design #session-management #zombie-sessions #defense-in-depth

### 之前的慘痛教訓

| 問題 | 原因 | 後果 |
|------|------|------|
| Token 100% 爆炸 | Sub-Agent 結束後 session 不會自動清理 | Context window 滿載，無法繼續對話 |
| Zombie Sessions 累積 | Cron 5-10 分鐘掃描一次，時間差導致遺漏 | 系統負載增加，記憶體浪費 |
| 無預警機制 | 只有到 100% 才知道，沒有 80% 預警 | 來不及反應，只能重啟 session |

### 🛡️ 防呆設計：多層防護機制 (Defense in Depth)

```
┌─────────────────────────────────────────────┐
│ Layer 1: 開發者主動調用 (最即時)              │
│ Sub-Agent 結束時主動呼叫 Lifecycle API       │
│ ↓ 即時封存，無延遲                           │
├─────────────────────────────────────────────┤
│ Layer 2: 常駐 Guardian (30秒輪詢)            │
│ 檢測 Token > 80% 或 10分鐘無活動             │
│ ↓ 自動封存 + 通知                            │
├─────────────────────────────────────────────┤
│ Layer 3: Cron 兜底 (每小時)                  │
│ 清理任何漏網之魚                             │
│ ↓ 最後防線                                   │
└─────────────────────────────────────────────┘
```

### 現況觀察

**X Feed Monitor Session 滿載事件** [2026-02-15]：
- 狀態：200,000 / 200,000 tokens (100%)
- 模型：gemini-3-flash-preview
- 問題：未觸發自動清理，導致累積至上限
- 影響：可能影響下次 00:00 排程執行

### 待實作改進

1. **Sub-Agent 結束時主動清理**：
   - 在每個 sub-agent 任務完成後呼叫 session cleanup
   - 確保不留下 Zombie Sessions

2. **80% 預警機制**：
   - 當 Context 達 80% 時提前通知
   - 而非等到 100% 爆炸才處理

3. **Session 生命週期管理**：
   - 定義明確的 session 存活時間（如 30 分鐘無活動即封存）
   - 避免長期閒置的 sessions 佔用資源


---

## ✅ Sub-Agent 管理最佳實踐 [2026-02-15]
**來源**：G大 系統設計原則
**標籤**：#sub-agent #session-management #isolated-session #best-practices

### 核心原則

| 原則 | 說明 |
|------|------|
| ✅ **Isolated Session** | 每個 Sub-Agent 獨立上下文 |
| ✅ **Auto-Terminate** | 任務完成自動結束 |
| ✅ **Auto-Archive** | Transcript 自動保存到 .jsonl |

### 確保事項

1. **不讓 Sub-Agent 存活超過單一任務**
   - 一個 session = 一個任務
   - 任務完成即結束，不拖延

2. **不在多個任務間重用同一個 session**
   - 避免 context 污染
   - 避免累積歷史記錄

3. **每次 sessions_spawn 都是全新的 Agent**
   - 乾淨 slate
   - 無前任任務殘留

### 批次處理模式

```
Batch 1: 啟動 4 個 Sub-Agents
    ↓
等待全部完成 → 自動銷毀並封存
    ↓
Batch 2: 啟動下一批 4 個 Sub-Agents
    ↓
等待完成 → 自動銷毀...
```

### 優勢

- **無 Zombie Sessions**：每個 session 有明確生命週期
- **無 Context 累積**：每次從零開始
- **可追蹤**：.jsonl 自動保存完整對話記錄
- **可擴展**：批次處理不會造成系統負載累積

### 反模式（避免）

❌ **長期運行的 Sub-Agent**：超過單一任務範圍
❌ **Session 重用**：多個任務共用同一 context
❌ **無限制並發**：同時啟動過多 agents 造成資源枯竭


---

## 📰 X 文章：別再為 Token 焦慮了 [2026-02-15]
**來源**：范凯说 AI (@robbinfan) https://x.com/robbinfan/status/2022952496189313404
**標籤**：#openclaw #token-management #subscription #claude #gpt #gemini #cost-optimization
**互動數據**：21 留言 / 51 轉推 / 198 讚 / 482 書籤 / 31.9K 觀看

### 核心觀點

> 「你根本不需要把時間花在『省 Token』上。因為你那三大頂級模型的 $20 訂閱套餐（Claude / GPT / Gemini），在大多數正常使用場景裡，已經足夠覆蓋你的小龍蝦了。」

### 常見焦慮症狀

- 「24 小時燒了 70 美金」
- 「我一周幾百美金」
- 「更誇張的，直接燒到一周 1000 多」

→ 導致「AI 使用習慣性貧窮」：每次要讓智能體多幹點活，手都會先停一下：「要不……算了？」

### 范凯的配置架構

**主控智能體**：Friday（主控 Agent 名稱）
**主控預設模型**：GPT 5.2

**模型池（想用哪個切哪個）**：
- GPT 系列：GPT 5.2、GPT 5.2 Codex、GPT 5.3 Codex
- Claude 系列：Claude Opus 4.6、Opus 4.5、Sonnet 4.5
- Gemini 系列：Gemini 3 Pro、Gemini 3 Flash

**多 Session 並行**：
1. Gemini 3 Pro Session — OAuth 登入，百萬級上下文
2. Claude Opus 4.6 Session — Claude Pro 訂閱 token，40 萬上下文
3. GPT Session — OAuth 登入，40 萬上下文

### 接續教學（三步驟）

**第一步：Claude Pro → OpenClaw**
```bash
# 安裝 Claude Code
curl -fsSL https://claude.ai/install.sh | bash

# 生成 Token
claude setup-token

# 配置
openclaw configure
# provider: Anthropic
# 選擇模型: Opus / Sonnet
```

**第二步：GPT Plus → OpenClaw**
```bash
# 安裝 Codex CLI
npm i -g @openai/codex

# 配置
openclaw configure
# provider: OpenAI Codex（OAuth 方式，非 API Key）
# 選擇模型: GPT 5.2 / 5.2 Codex / 5.3 Codex
```

**第三步：Gemini Pro → OpenClaw**
```bash
# 安裝 Gemini CLI
npm install -g @google/gemini-cli

# 配置
openclaw configure
# provider: Google
# 驗證方式: Google Gemini CLI Auth
# 選擇模型: Gemini 3 Pro / Flash
```

### 驗證指令
```bash
openclaw models list
# Telegram 內可用 /models 查看
```

### 風控提醒

- **Claude**：Anthropic 對部分地區/帳號可能觸發風控 → 建議當「增強項」，非「穩定底座」
- **GPT / Gemini**：穩如磐石

### 關鍵洞察

> 「你真正需要優化的從來不是『省 Token』。而是：怎麼把小龍蝦變成你的生產力外骨骼，持續替你產出價值。」

**真實案例**：朋友每月花 $200 買 ChatGPT Deep Research，以為裝了小龍蝦還得額外買 API Token。配完訂閱套餐後感慨：「這 200 美元，終於真正花到刀刃上了。」

### 行動呼籲

別再為 Token 焦慮了！用足你的訂閱套餐，把每一分錢的價值都榨乾。

