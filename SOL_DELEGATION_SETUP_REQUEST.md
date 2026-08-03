# 建立「高高手」與「高手高手高高手」雙層 Sol 委派功能

請直接在我目前使用的本機 Codex 環境中完成以下全域設定。

這是一項 Codex 個人層級的 Agent 與路由規則設定，不是目前專案的程式功能。

請實際檢查目前安裝的 Codex 版本、設定結構、模型目錄及自訂 Agent 支援狀態，再進行最小幅度修改。

不得只提供操作教學或範例。請實際完成設定、驗證、顯示 diff，並回報結果。

---

# 一、最終功能目標

我的 Codex App 主代理模型、推理強度及一般速度模式，會由我自行在 App 中選擇。

目前預期的日常主代理為：

* 模型：GPT-5.6 Luna
* 推理強度：Max
* 速度：由 Codex App 當前設定決定

本次不要替我設定或更改 Luna。

本次只建立兩個由固定中文暗號啟動的自訂子代理：

## 1. Sol High 子代理

* 自訂 Agent 名稱：`sol_high`
* 模型：`gpt-5.6-sol`
* 推理強度：`high`
* 執行速度：標準速度
* 啟動暗號：`交給高高手處理`

## 2. Sol xHigh 子代理

* 自訂 Agent 名稱：`sol_xhigh`
* 模型：`gpt-5.6-sol`
* 推理強度：`xhigh`
* 執行速度：標準速度
* 啟動暗號：`交給高手高手高高手處理`

兩個子代理都不得使用 Fast 模式或 Fast service tier。

兩個暗號都是「單次完整任務委派」，不是永久切換目前對話的主模型。

子代理完成、失敗、被終止或回報阻礙後，控制權立即回到原本主代理。

後續新任務恢復使用 Codex App 當前選擇的：

* 主模型
* 推理強度
* 速度模式

除非使用者在新任務中再次輸入完整暗號。

---

# 二、速度模式的最終要求

## 2.1 兩個 Sol Agent 都必須使用標準速度

以下兩個 Agent 都必須使用標準速度：

* `sol_high`
* `sol_xhigh`

不得使用：

```toml
service_tier = "fast"
```

不得在啟動子代理前或啟動子代理時開啟 Fast 模式。

不得因為主代理目前是 Fast 模式，就直接讓 Sol 子代理繼承 Fast 模式。

不得因為使用者要求更高推理強度，就將速度自動升級為 Fast。

推理強度與速度模式是兩個獨立設定：

* `high` 或 `xhigh` 代表推理深度。
* 標準或 Fast 代表服務速度層級。
* 本功能只提高 Sol 的推理強度，不提高服務速度層級。

## 2.2 不得未經驗證寫入 `service_tier = "standard"`

目前不要直接假設以下設定合法：

```toml
service_tier = "standard"
```

必須先根據以下來源確認目前版本及目前模型實際公布的 service tier ID：

1. 目前安裝版本的模型目錄。
2. `codex --help`、`/fast status` 或其他目前版本支援的命令。
3. 本機 Codex 設定參考或模型 metadata。
4. 目前版本實際載入 Agent 後的運行資訊。
5. OpenAI 當前官方 Codex 文件。

如果目前模型目錄明確公布一個代表標準速度的 service tier ID，例如該版本實際使用的 `default`、`standard`、`auto` 或其他值，才可以在 Agent TOML 中使用該值。

不得自行猜測 tier ID。

## 2.3 標準速度的實作優先順序

請按照以下順序實作。

### 方法 A：使用目前版本明確公布的標準 tier ID

如果目前 Codex 版本及 `gpt-5.6-sol` 的模型目錄明確公布了標準速度 tier ID：

* 在兩個 Agent TOML 中設定該正式值。
* 必須記錄實際值及判斷依據。
* 必須實際驗證子代理運行時沒有使用 Fast tier。

### 方法 B：不設定 `service_tier`

如果目前版本只明確公布：

```toml
service_tier = "fast"
```

但沒有公布標準模式的固定字串：

* 兩個 Agent TOML 都不要加入 `service_tier`。
* 確保兩份 Agent TOML 中不存在 `service_tier = "fast"`。
* 確認啟動子代理時沒有明確 Fast override。
* 確認子代理執行階段的 Fast 狀態為關閉。
* 不得只因為沒有寫 `service_tier`，就直接宣稱已保證標準速度。
* 必須檢查是否從父代理或其他設定層繼承 Fast。

### 方法 C：使用目前版本支援的明確 Fast-off 機制

如果子代理會繼承父代理目前的 Fast 狀態，必須使用目前 Codex 版本真正支援的方式，對這兩個子代理明確關閉 Fast。

例如目前版本若支援：

* 子代理專屬 service tier override。
* Spawn 時的 service tier override。
* Agent configuration layer 中的 Fast-off 值。
* 建立子代理 Session 時的標準 tier 選項。

則使用其中最小、最局部、可驗證的方法。

不得為了讓子代理使用標準速度，而永久關閉使用者整個 Codex App 的 Fast 功能。

不得修改所有其他 Agent 的速度設定。

## 2.4 無法保證標準速度時

如果目前版本無法讓自訂子代理獨立覆蓋父代理的速度層級：

* 不得假裝已完成固定標準速度。
* 必須明確說明此限制。
* 必須列出目前已做到的部分。
* 必須說明子代理是否仍可能繼承父代理速度。
* 不得擅自永久修改 App 全域速度。
* 不得擅自將整個 Codex App 設成 Standard。
* 必須把此項標記為「未能完全驗證」。

---

# 三、暗號與 Agent 對應

## 3.1 第一個暗號

完整暗號：

```text
交給高高手處理
```

對應：

```text
Agent：sol_high
Model：gpt-5.6-sol
Reasoning effort：high
Speed：Standard
```

## 3.2 第二個暗號

完整暗號：

```text
交給高手高手高高手處理
```

對應：

```text
Agent：sol_xhigh
Model：gpt-5.6-sol
Reasoning effort：xhigh
Speed：Standard
```

## 3.3 精確匹配原則

只有以下完整字串作為使用者操作指令時才觸發：

```text
交給高高手處理
```

```text
交給高手高手高高手處理
```

不得把以下文字當作同義暗號：

* 高高手
* 高高手處理
* 交給高手處理
* 高手處理
* 交給高高手
* 交給高手高手處理
* 交給最高高手處理
* 請仔細處理
* 請深入分析
* 使用更強模型
* 使用 Sol
* 用高推理處理
* 用最快速度處理

不得自行進行模糊匹配或近似匹配。

---

# 四、暗號的任務範圍

## 4.1 暗號與任務在同一則訊息

例如：

```text
交給高高手處理：修復登入流程，找出根本原因並執行相關測試。
```

應將這則訊息要求的完整任務交給 `sol_high`。

例如：

```text
交給高手高手高高手處理：全面檢查目前資料架構，修正必要問題並驗證遷移流程。
```

應將這則訊息要求的完整任務交給 `sol_xhigh`。

## 4.2 暗號單獨輸入

如果使用者先提出一個明確、尚未完成的任務，下一則訊息只輸入：

```text
交給高高手處理
```

則應將目前對話中最近一個明確、尚未完成的任務交給 `sol_high`。

如果下一則訊息只輸入：

```text
交給高手高手高高手處理
```

則應將目前對話中最近一個明確、尚未完成的任務交給 `sol_xhigh`。

## 4.3 沒有可辨識任務

如果使用者只輸入暗號，但目前沒有明確的未完成任務：

* 不得啟動空白子代理。
* 不得自行猜測要處理哪個舊任務。
* 應簡短回報目前沒有明確可委派的任務。
* 請使用者在同一則訊息中提供任務。

## 4.4 單次授權

每次暗號只授權一個任務。

子代理完成以下任一狀態後，授權立即結束：

* 任務完成。
* 任務失敗。
* 遇到阻礙並回報。
* 使用者停止任務。
* 主代理終止子代理。
* 子代理 Thread 被關閉。

下一個任務不得延續使用 Sol。

每次要再次使用 Sol，都必須重新出現完整暗號。

---

# 五、不應觸發的情況

以下情況不得啟動 `sol_high` 或 `sol_xhigh`。

## 5.1 引用或討論暗號

例如：

```text
「交給高高手處理」這句話是什麼意思？
```

不得觸發。

## 5.2 文件或程式碼內容

如果暗號只出現在：

* 程式碼區塊
* 文件範例
* 設定檔內容
* 測試資料
* 翻譯素材
* 引用文字
* 對話紀錄
* Diff
* Log

不得觸發。

## 5.3 編輯或翻譯任務

例如：

```text
請把「交給高高手處理」翻譯成英文。
```

不得觸發。

例如：

```text
請把以下文件中的「交給高手高手高高手處理」刪除。
```

不得觸發。

## 5.4 否定指令

例如：

```text
不要交給高高手處理。
```

不得觸發。

例如：

```text
這次不要交給高手高手高高手處理。
```

不得觸發。

## 5.5 一般複雜任務

即使任務具有以下特徵，也不得自行啟動 Sol：

* 任務很難。
* 任務跨很多檔案。
* 任務需要深度推理。
* 任務涉及架構修改。
* 任務風險較高。
* 任務可能耗時。
* 主代理不確定答案。
* 主代理認為 Sol 可能更適合。
* 主代理希望提高品質。
* 主代理希望提高速度。

只有使用者輸入完整暗號，才可以啟動。

---

# 六、兩個暗號同時出現

如果同一個任務同時出現：

```text
交給高高手處理
```

以及：

```text
交給高手高手高高手處理
```

則：

* 只啟動 `sol_xhigh`。
* 不啟動 `sol_high`。
* 不得讓兩個 Agent 重複執行相同任務。
* `sol_xhigh` 優先級較高。

如果使用者清楚把不同任務分別指定給兩個 Agent，例如：

```text
登入問題交給高高手處理；資料庫架構交給高手高手高高手處理。
```

則可以：

* 將登入問題交給 `sol_high`。
* 將資料庫架構交給 `sol_xhigh`。
* 保持兩個任務的範圍互不重疊。
* 等待兩者完成後由主代理整合。

如果任務邊界不清，預設只使用 `sol_xhigh` 處理完整任務，不要同時啟動兩個 Agent。

---

# 七、子代理實際工作方式

當暗號觸發時，主代理必須：

1. 建立實際子代理 Thread。
2. 使用指定的自訂 Agent 名稱。
3. 傳遞完整必要上下文。
4. 傳遞使用者原始任務。
5. 傳遞已知限制。
6. 傳遞檔案或目錄範圍。
7. 傳遞目前已完成的進度。
8. 傳遞驗收條件。
9. 傳遞不得修改的範圍。
10. 等待子代理完成。
11. 接收子代理結果。
12. 向使用者回報結果。

不得：

* 只模擬 Sol 的語氣。
* 只在主代理提示詞中說自己是 Sol。
* 由 Luna 完成工作後聲稱是 Sol 完成。
* 在主代理中重複執行相同任務。
* 未啟動子代理卻聲稱已委派。
* 將任務改交給其他 Agent。
* 靜默降級模型。
* 靜默降低推理強度。
* 靜默啟用 Fast。
* 在子代理完成後繼續保留 Sol 授權。

---

# 八、環境檢查

修改前請先確認實際環境。

## 8.1 確認 Codex Home

依以下優先順序判斷：

1. 如果設定了 `CODEX_HOME`，使用該路徑。
2. 如果未設定 `CODEX_HOME`，使用目前使用者 home 目錄下的 `.codex`。
3. 不得硬編碼 Windows 使用者名稱。
4. 不得假設 Codex 一定運行於 WSL。
5. 必須列出實際使用的絕對路徑。

可能的形式包括：

```text
C:\Users\<user>\.codex
```

```text
/home/<user>/.codex
```

```text
/Users/<user>/.codex
```

## 8.2 確認執行檔

檢查：

```text
codex --version
```

以及目前作業系統可用的執行檔定位方式，例如：

Windows：

```powershell
Get-Command codex
where.exe codex
```

Linux、macOS 或 WSL：

```bash
command -v codex
which codex
```

記錄：

* Codex 版本。
* 執行檔路徑。
* 運行環境。
* App 與 CLI 是否共用同一個 Codex Home。
* 目前是否為 Windows 原生、WSL、Linux 或 macOS。

## 8.3 檢查功能支援

確認目前版本是否支援：

* 自訂 Agent。
* `$CODEX_HOME/agents/`。
* Agent TOML。
* `name`。
* `description`。
* `developer_instructions`。
* `model` override。
* `model_reasoning_effort` override。
* `gpt-5.6-sol`。
* `high`。
* `xhigh`。
* 多代理功能。
* Agent Thread。
* Agent 活動顯示。
* service tier override。
* Fast 狀態檢查。
* 子代理速度層級檢查。

不得只根據記憶判斷。

## 8.4 檢查模型目錄

檢查目前 Codex 模型目錄或模型 metadata，確認：

* `gpt-5.6-sol` 是否存在。
* 是否支援 `high`。
* 是否支援 `xhigh`。
* 是否提供 Fast tier。
* 標準速度的 tier ID 是什麼。
* 未設定 service tier 時會如何處理。
* 子代理是否繼承父代理的 service tier。
* Agent TOML 是否能覆蓋父代理 service tier。

如果無法從目前客戶端取得其中某些資訊，必須清楚標記為無法確認。

---

# 九、修改前盤點

修改前檢查以下檔案及設定：

* `$CODEX_HOME/config.toml`
* `$CODEX_HOME/AGENTS.md`
* `$CODEX_HOME/AGENTS.override.md`
* `$CODEX_HOME/agents/`
* `$CODEX_HOME/agents/sol_high.toml`
* `$CODEX_HOME/agents/sol_xhigh.toml`

另外檢查：

* 是否已有名為 `sol_high` 的 Agent。
* 是否已有名為 `sol_xhigh` 的 Agent。
* 是否已有相同暗號規則。
* 是否已有舊版「高高手」規則。
* 是否已有 `service_tier = "fast"`。
* 是否已有全域 Fast 設定。
* 是否已有專案層級 `.codex/config.toml`。
* 是否有專案層級 `AGENTS.md`。
* 是否有 `AGENTS.override.md`。
* 是否明確關閉多代理功能。
* 是否有管理員 enforced settings。
* 是否有其他設定層會覆蓋本次設定。

---

# 十、保護現有設定

必須保留所有無關內容。

不得：

* 覆蓋整份 `config.toml`。
* 重建整個 `.codex` 目錄。
* 刪除其他 Agent。
* 刪除其他設定。
* 刪除 MCP。
* 刪除 Skill。
* 修改 API key。
* 顯示 Token。
* 修改主模型。
* 修改主推理強度。
* 修改 App 預設 Luna。
* 修改 App 全域速度。
* 永久關閉整個 App 的 Fast 功能。
* 重新格式化所有設定檔。
* 修改專案原始碼。
* 修改與本功能無關的檔案。

如果目標檔案已存在：

* 只更新本功能相關內容。
* 保留原有無關內容。
* 不得建立重複 Agent。
* 不得建立重複路由區塊。

---

# 十一、備份與安全寫入

對所有即將修改的既有檔案：

1. 先讀取原始內容。
2. 建立暫時快照。
3. 計算或保存修改前內容。
4. 使用安全寫入方式。
5. 完成後解析驗證。
6. 驗證失敗時回滾。
7. 驗證成功後清理暫存檔。

最終報告必須說明：

* 哪些檔案有備份。
* 是否發生回滾。
* 是否留下暫存檔。
* 是否修改了原有內容。

---

# 十二、建立 `sol_high`

目標路徑：

```text
$CODEX_HOME/agents/sol_high.toml
```

如果 `agents` 目錄不存在，只建立該目錄。

## 12.1 基本內容

請建立以下內容：

```toml
name = "sol_high"

description = """
Use this custom agent only when the user invokes the exact Chinese command
「交給高高手處理」 as an active instruction for the current task.

This agent runs GPT-5.6 Sol with high reasoning effort at Standard speed.
Never select it automatically because a task is difficult, complex, important,
cross-file, or reasoning-intensive.
Never run this agent using Fast mode.
"""

model = "gpt-5.6-sol"
model_reasoning_effort = "high"

developer_instructions = """
You are the user's explicitly invoked Sol High execution agent.

You must run at Standard speed. Do not enable, request, or retain Fast service
tier for this delegated task. If the runtime reports that Fast mode is active
and you cannot locally override it, stop and report the speed configuration
conflict to the parent agent instead of falsely claiming Standard speed.

Own the complete task delegated by the parent agent. Use all relevant
conversation context, requirements, constraints, files, current progress,
and acceptance criteria provided by the parent.

Perform the actual requested work rather than merely advising the parent how
to do it. Inspect the relevant context and files, reason carefully, make
required changes when authorized, and validate the result appropriately.

Do not redefine the user's objective.
Do not expand the task into unrelated improvements.
Do not modify unrelated files or configuration.
Do not repeat work already completed unless verification requires it.
Do not change the parent's default model, reasoning effort, or speed setting.
Do not spawn additional subagents unless the user explicitly requested nested
delegation.

If the task is ambiguous in a way that blocks safe completion, return the
ambiguity or blocker to the parent agent rather than inventing requirements.

When finished, return a concise but complete report containing:
- the work completed;
- the main conclusions or decisions;
- files created, modified, or deleted;
- commands, tests, or checks performed;
- validation results;
- the actual model used;
- the actual reasoning effort used;
- the actual service tier or speed status, when observable;
- remaining risks, blockers, or unresolved matters.

Your authorization applies only to the currently delegated task. After
returning the result, control belongs to the parent agent.
"""
```

## 12.2 標準速度欄位

不要預設加入：

```toml
service_tier = "fast"
```

也不要未經驗證加入：

```toml
service_tier = "standard"
```

根據前述速度實作優先順序處理：

* 如果模型目錄明確公布標準 tier ID，加入正式值。
* 如果沒有公布，省略 `service_tier`。
* 如果省略後會繼承 Fast，使用目前版本可驗證的局部 Fast-off override。
* 如果無法避免繼承 Fast，停止宣告完整成功並回報限制。

---

# 十三、建立 `sol_xhigh`

目標路徑：

```text
$CODEX_HOME/agents/sol_xhigh.toml
```

## 13.1 基本內容

請建立以下內容：

```toml
name = "sol_xhigh"

description = """
Use this custom agent only when the user invokes the exact Chinese command
「交給高手高手高高手處理」 as an active instruction for the current task.

This agent runs GPT-5.6 Sol with extra-high reasoning effort at Standard speed.
Never select it automatically because a task is difficult, complex, important,
cross-file, or reasoning-intensive.
Never run this agent using Fast mode.
"""

model = "gpt-5.6-sol"
model_reasoning_effort = "xhigh"

developer_instructions = """
You are the user's explicitly invoked Sol Extra High execution agent.

You must run at Standard speed. Do not enable, request, or retain Fast service
tier for this delegated task. If the runtime reports that Fast mode is active
and you cannot locally override it, stop and report the speed configuration
conflict to the parent agent instead of falsely claiming Standard speed.

Own the complete task delegated by the parent agent. Use all relevant
conversation context, requirements, constraints, files, current progress,
and acceptance criteria provided by the parent.

Perform the actual requested work rather than merely advising the parent how
to do it. Apply extra-high reasoning depth to difficult dependencies,
assumptions, edge cases, tradeoffs, failure modes, and validation.

Inspect all context and files that are materially relevant, but do not perform
broad unrelated exploration merely because more context is available.

Do not redefine the user's objective.
Do not expand the task into unrelated improvements.
Do not modify unrelated files or configuration.
Do not repeat work already completed unless verification requires it.
Do not change the parent's default model, reasoning effort, or speed setting.
Do not spawn additional subagents unless the user explicitly requested nested
delegation.

When major implementation or architectural choices are required:
- identify the relevant assumptions;
- compare material alternatives;
- choose the option that best satisfies the user's stated objective;
- keep the implementation within the requested scope;
- validate important behavior and edge cases.

If the task is ambiguous in a way that blocks safe completion, return the
ambiguity or blocker to the parent agent rather than inventing requirements.

When finished, return a concise but complete report containing:
- the work completed;
- the main conclusions or decisions;
- files created, modified, or deleted;
- commands, tests, or checks performed;
- validation results;
- important assumptions;
- the actual model used;
- the actual reasoning effort used;
- the actual service tier or speed status, when observable;
- remaining risks, blockers, or unresolved matters.

Your authorization applies only to the currently delegated task. After
returning the result, control belongs to the parent agent.
"""
```

## 13.2 標準速度欄位

同樣不得設定：

```toml
service_tier = "fast"
```

不得在沒有證據時設定：

```toml
service_tier = "standard"
```

必須使用目前版本及模型目錄實際支援的方法。

---

# 十四、全域路由規則

## 14.1 目標檔案

永久規則寫入：

```text
$CODEX_HOME/AGENTS.md
```

如果不存在則建立。

如果存在非空的：

```text
$CODEX_HOME/AGENTS.override.md
```

則：

* 保留原內容。
* 在 `AGENTS.md` 加入本規則。
* 同時在 `AGENTS.override.md` 加入相同規則，確保目前 override 狀態下仍生效。
* 不得覆蓋原內容。

## 14.2 區塊標記

使用：

```md
<!-- BEGIN SOL DELEGATION ROUTING -->
```

以及：

```md
<!-- END SOL DELEGATION ROUTING -->
```

如果區塊已存在：

* 更新現有區塊。
* 不要新增第二份。

如果區塊不存在：

* 在檔案末尾追加。
* 保持適當空行。

## 14.3 路由內容

加入以下內容：

```md
<!-- BEGIN SOL DELEGATION ROUTING -->

## Explicit Sol delegation commands

The current app-selected primary model, reasoning effort, and speed mode handle
every task by default.

The following exact Chinese commands are explicit single-task delegation
commands. They do not permanently switch the primary model, reasoning effort,
or speed mode.

### Sol High command

When the user uses the exact Chinese command `交給高高手處理` as an active
instruction, delegate the complete current task to the custom agent `sol_high`.

The required execution configuration is:

- Agent: `sol_high`
- Model: `gpt-5.6-sol`
- Reasoning effort: `high`
- Speed: Standard
- Fast mode: Off

Requirements:

- Spawn the actual custom agent named `sol_high`.
- Do not merely imitate Sol High in the parent thread.
- Do not replace it with another built-in or custom agent.
- Pass the complete relevant task context, constraints, current progress, file
  scope, and acceptance criteria to the child agent.
- Let the child agent perform the delegated work.
- Do not duplicate the same implementation in the parent thread.
- Wait for the child agent to finish before presenting the result.
- Do not enable or inherit Fast service tier for this delegated task.
- Verify the actual service tier or Fast status when the client exposes it.
- If Standard speed cannot be guaranteed, report the limitation instead of
  falsely claiming that Standard speed was used.
- After the child returns, control immediately returns to the primary agent.
- The authorization does not continue into later user tasks.

### Sol Extra High command

When the user uses the exact Chinese command `交給高手高手高高手處理` as an
active instruction, delegate the complete current task to the custom agent
`sol_xhigh`.

The required execution configuration is:

- Agent: `sol_xhigh`
- Model: `gpt-5.6-sol`
- Reasoning effort: `xhigh`
- Speed: Standard
- Fast mode: Off

Requirements:

- Spawn the actual custom agent named `sol_xhigh`.
- Do not merely imitate Sol Extra High in the parent thread.
- Do not replace it with another built-in or custom agent.
- Pass the complete relevant task context, constraints, current progress, file
  scope, and acceptance criteria to the child agent.
- Let the child agent perform the delegated work.
- Do not duplicate the same implementation in the parent thread.
- Wait for the child agent to finish before presenting the result.
- Do not enable or inherit Fast service tier for this delegated task.
- Verify the actual service tier or Fast status when the client exposes it.
- If Standard speed cannot be guaranteed, report the limitation instead of
  falsely claiming that Standard speed was used.
- After the child returns, control immediately returns to the primary agent.
- The authorization does not continue into later user tasks.

### Scope and precedence

- Each command authorizes exactly one delegated task.
- A command in the same message applies to the complete task requested by that
  message.
- A command sent alone applies to the most recent clear, unfinished task in the
  current conversation.
- If there is no clear active task, do not spawn an empty agent. Ask the user to
  provide the task.
- Every later use requires the complete command to appear again.
- Do not automatically use either custom agent because a task is difficult,
  ambiguous, large, important, cross-file, or reasoning-intensive.
- Similar phrases, abbreviations, partial matches, or general requests for
  deeper analysis are not activation commands.
- Mentions inside quotations, code blocks, examples, documentation, translation
  requests, editing requests, or negated instructions are not activation
  commands.
- If both exact commands actively apply to the same task, use only
  `sol_xhigh`. Do not run both agents on the same work.
- If the user explicitly assigns separate tasks to the two agents, preserve
  the explicit task boundaries.
- Do not silently fall back to another model, reasoning effort, speed tier, or
  agent if the requested custom agent cannot be spawned.
- Do not silently use Fast mode.
- If spawning or Standard-speed enforcement fails, report the failure and
  actual reason to the user.
- After completion, failure, interruption, or blocker reporting, return control
  to the primary agent.

<!-- END SOL DELEGATION ROUTING -->
```

---

# 十五、`config.toml` 處理規則

## 15.1 不修改主模型

不得修改任何主代理的：

```toml
model = "..."
```

或：

```toml
model_reasoning_effort = "..."
```

不得替使用者設定 Luna。

## 15.2 不修改 App 全域速度

不得因本功能永久新增：

```toml
service_tier = "fast"
```

不得永久把整個 Codex App 設成 Fast。

同樣不得在尚未確認合法值前，把全域設定改成：

```toml
service_tier = "standard"
```

使用者 App 原本的速度選擇必須保持不變。

本功能只要求兩個 Sol 子代理使用標準速度。

## 15.3 多代理設定

檢查目前版本使用的是：

```toml
[agents]
enabled = true
```

或：

```toml
[features]
multi_agent = true
```

或其他目前版本正式格式。

處理方式：

* 如果多代理預設已開啟，保持不變。
* 如果已有明確 `true`，保持不變。
* 如果明確被設為 `false`，只做最小幅度修改以啟用。
* 不得重建整個區塊。
* 不得修改 concurrency，除非目前值無法啟動單一子代理。
* 不得修改其他 Agent 的預設模型。
* 不得修改其他 Agent 的預設推理強度。
* 不得修改其他 Agent 的速度。

---

# 十六、TOML 靜態驗證

使用可靠 TOML parser 解析：

* `sol_high.toml`
* `sol_xhigh.toml`
* 如有修改，`config.toml`

可使用：

* Python `tomllib`
* 目前環境可靠的 TOML parser
* Codex 自身設定載入器

確認：

## `sol_high`

```text
name = sol_high
model = gpt-5.6-sol
model_reasoning_effort = high
Fast tier = absent or explicitly disabled using a verified supported value
```

## `sol_xhigh`

```text
name = sol_xhigh
model = gpt-5.6-sol
model_reasoning_effort = xhigh
Fast tier = absent or explicitly disabled using a verified supported value
```

另外確認：

* `description` 存在。
* `developer_instructions` 存在。
* 三引號完整結束。
* 沒有重複欄位。
* 沒有錯誤 tier 值。
* 沒有 `service_tier = "fast"`。
* 沒有未經驗證的 `service_tier = "standard"`。
* 沒有修改父代理設定。

---

# 十七、路由規則靜態驗證

確認：

* `AGENTS.md` 只有一組路由區塊。
* 如果存在有效 `AGENTS.override.md`，其中也只有一組。
* 原有內容沒有被刪除。
* 原有內容沒有被截斷。
* 兩個暗號字串完全正確。
* Agent 名稱完全正確。
* High 與 xHigh 沒有對調。
* 兩者都標示 Standard。
* 兩者都明確禁止 Fast。
* 任務完成後恢復主代理。
* 不存在永久 Sol 模式。
* 不存在近似暗號觸發規則。

---

# 十八、執行階段驗證

修改完成後，使用新的 Codex Session 驗證。

不得在仍使用舊指令快取的 Session 中宣告成功。

可以：

* 完全重新啟動 Codex App。
* 建立新的 App Chat。
* 啟動新的 Codex CLI Session。
* 使用安全的無檔案測試。

所有測試都不得修改專案檔案。

## 18.1 普通任務測試

輸入：

```text
請只回答「普通測試收到」，不要修改檔案，也不要啟動子代理。
```

預期：

* 主代理處理。
* 不啟動 `sol_high`。
* 不啟動 `sol_xhigh`。

## 18.2 Sol High 測試

輸入：

```text
交給高高手處理：這是設定驗證。不要修改任何檔案，只回報實際啟動的 Agent、模型、推理強度與速度層級。
```

預期：

* 啟動 `sol_high`。
* 模型為 `gpt-5.6-sol`。
* 推理強度為 `high`。
* 速度為 Standard。
* Fast 為 Off。
* 不啟動 `sol_xhigh`。
* 完成後回到主代理。

## 18.3 Sol xHigh 測試

輸入：

```text
交給高手高手高高手處理：這是設定驗證。不要修改任何檔案，只回報實際啟動的 Agent、模型、推理強度與速度層級。
```

預期：

* 啟動 `sol_xhigh`。
* 模型為 `gpt-5.6-sol`。
* 推理強度為 `xhigh`。
* 速度為 Standard。
* Fast 為 Off。
* 不啟動 `sol_high`。
* 完成後回到主代理。

## 18.4 引用測試

輸入：

```text
請解釋「交給高高手處理」這個暗號的用途，但不要啟動子代理。
```

預期：

* 不啟動子代理。

## 18.5 否定測試

輸入：

```text
不要交給高高手處理。只回答「收到」。
```

預期：

* 不啟動子代理。

## 18.6 近似字串測試

輸入：

```text
交給高高手
```

預期：

* 不啟動子代理。

## 18.7 優先級測試

輸入：

```text
本次測試同時包含交給高高手處理與交給高手高手高高手處理。只將測試交給較高層級 Agent，不要修改檔案。
```

預期：

* 只啟動 `sol_xhigh`。
* 不啟動 `sol_high`。

## 18.8 返回預設測試

完成 Sol 測試後，另輸入：

```text
這是後續普通測試。不要啟動子代理，只由目前 App 預設主代理回答。
```

預期：

* 不延續使用 Sol。
* 回到 App 當前預設主代理。
* 使用 App 當前速度設定。

## 18.9 父代理 Fast 狀態測試

如果 App 或 CLI 允許，另外驗證以下情境：

1. 主代理設定成 Fast。
2. 使用 `交給高高手處理`。
3. 檢查 `sol_high` 是否仍使用 Standard。
4. 使用 `交給高手高手高高手處理`。
5. 檢查 `sol_xhigh` 是否仍使用 Standard。
6. 任務完成後檢查主代理是否恢復原本 Fast 狀態。

預期：

* Sol Agent 不繼承 Fast。
* Sol Agent 使用 Standard。
* 子代理不得永久改變父代理速度。
* 子代理完成後父代理恢復原來的速度狀態。

如果目前版本無法做到此種獨立速度隔離，必須明確回報。

---

# 十九、驗證證據要求

不得只依賴子代理自行宣稱：

```text
我是 sol_high
```

或：

```text
我正在使用標準速度
```

必須盡可能取得實際證據，例如：

* Agent 活動面板。
* 子代理 Thread 名稱。
* Agent metadata。
* 模型 metadata。
* Reasoning effort 顯示。
* Service tier 顯示。
* `/fast status`。
* `/status`。
* `/agent`。
* App 的 Agent 詳細資訊。
* CLI JSON event。
* Codex log。
* 不含敏感資訊的 telemetry 或執行紀錄。

最終報告必須區分：

1. 已實際驗證。
2. 只從設定檔確認。
3. 由模型目錄確認。
4. 目前客戶端無法顯示。
5. 尚未驗證。

不得把靜態設定誤寫成執行階段證明。

---

# 二十、失敗與降級規則

如果任一項失敗，包括：

* Agent 無法載入。
* Agent 名稱無法辨識。
* 模型不可用。
* `high` 不支援。
* `xhigh` 不支援。
* Standard 無法固定。
* Fast 無法關閉。
* 子代理繼承父代理 Fast。
* 多代理功能關閉。
* 路由規則未生效。
* App 不顯示子代理。
* Agent spawn 失敗。

不得：

* 靜默改用 Luna。
* 靜默改用 Terra。
* 靜默改用其他 Sol effort。
* 靜默改用 Fast。
* 靜默改用其他 Agent。
* 由主代理完成後聲稱是子代理完成。
* 宣告完整成功。

必須回報：

* 哪一項失敗。
* 實際錯誤。
* 已完成哪些設定。
* 哪些只能靜態確認。
* 是否已回滾。
* 下一個必要處理步驟。

---

# 二十一、Diff 要求

完成後展示本次所有持久修改的 unified diff。

至少包括：

* 新增或修改的 `sol_high.toml`
* 新增或修改的 `sol_xhigh.toml`
* `AGENTS.md`
* 如有修改，`AGENTS.override.md`
* 如有修改，`config.toml`

要求：

* 新檔案顯示為從空檔新增。
* 現有檔案顯示真實修改前後差異。
* 不得只貼最終檔案。
* 不得省略修改。
* 不得把整個大型設定檔誤顯示為全部新增。
* 不得顯示 API key、Token、Cookie、密碼。
* 敏感內容必須遮蔽。
* 遮蔽不得改動原始檔案。
* Diff 必須能看出兩個 Agent 都沒有 Fast 設定。
* 如果加入標準 tier ID，Diff 必須顯示該值及其來源說明。

---

# 二十二、完成標準

只有全部符合以下條件，才能宣告完整完成：

* 已確認 Codex Home。
* 已確認 Codex 執行檔。
* 已確認 Codex 版本。
* 已確認自訂 Agent 格式。
* 已確認多代理功能。
* 已確認 `gpt-5.6-sol` 可用。
* 已確認 `high` 可用。
* 已確認 `xhigh` 可用。
* 已建立 `sol_high`。
* 已建立 `sol_xhigh`。
* 兩份 TOML 語法有效。
* 兩份 Agent 都使用標準速度。
* 兩份 Agent 都沒有使用 Fast。
* 已確認標準速度的實際設定方式。
* 已加入兩個精確暗號。
* 已排除引用、否定及近似字串。
* 已設定 xHigh 優先級。
* 已設定單次授權。
* 已設定任務結束後回到主代理。
* 未修改 App 預設 Luna。
* 未修改 App 主推理強度。
* 未永久修改 App 速度。
* 未刪除無關設定。
* 未建立重複區塊。
* 已完成 TOML parsing。
* 已完成 Agent 載入測試。
* 已完成 High 暗號測試。
* 已完成 xHigh 暗號測試。
* 已完成 Standard 速度測試。
* 已完成 Fast 關閉確認。
* 已完成返回預設測試。
* 已展示完整 diff。

如果標準速度只能從設定檔推定，不能宣告「均已通過實際驗證」。

---

# 二十三、最終回報格式

請按照以下格式回報。

## 1. 環境

* Codex Home：
* Codex 執行檔：
* Codex 版本：
* 作業系統：
* App／CLI 環境：
* 多代理功能狀態：

## 2. 模型相容性

* `gpt-5.6-sol`：
* `high`：
* `xhigh`：
* 判斷依據：

## 3. 速度相容性

* Fast 功能是否存在：
* Fast 當前狀態：
* Standard 的實際 tier 表示方式：
* Agent TOML 是否可覆蓋 service tier：
* 子代理是否會繼承父代理 Fast：
* `sol_high` 實際速度：
* `sol_xhigh` 實際速度：
* 判斷依據：

## 4. 建立或修改的檔案

逐一列出：

* 絕對路徑
* 新增或修改
* 修改目的

## 5. 最終暗號路由

確認：

* 普通任務使用 App 當前主代理。
* `交給高高手處理` 使用 `sol_high`。
* `sol_high` 使用 Sol High Standard。
* `交給高手高手高高手處理` 使用 `sol_xhigh`。
* `sol_xhigh` 使用 Sol xHigh Standard。
* 兩者都不使用 Fast。
* 每次暗號只授權一項任務。
* 任務結束後回到主代理。
* 後續任務不延續使用 Sol。

## 6. 靜態驗證結果

* TOML parsing：
* 必要欄位：
* 模型設定：
* 推理強度：
* Fast 設定檢查：
* 路由區塊：
* 原有設定保留：

## 7. 執行階段驗證結果

* 普通任務測試：
* High 暗號測試：
* xHigh 暗號測試：
* 引用測試：
* 否定測試：
* 近似字串測試：
* 優先級測試：
* 返回預設測試：
* 父代理 Fast／子代理 Standard 隔離測試：
* 未能驗證的項目：

## 8. Diff

展示完整、經敏感資訊遮蔽的 unified diff。

## 9. 問題與風險

列出：

* 無法實際確認的項目。
* App 或 CLI 限制。
* 專案層級設定覆蓋風險。
* `AGENTS.override.md` 影響。
* Speed tier 繼承風險。
* 是否需要重新啟動 App。
* 是否需要建立新 Session。

## 10. 最終結論

只能使用以下其中一個結論：

* `設定完成，兩個暗號、模型、推理強度及標準速度均已通過實際驗證。`
* `設定已建立，但標準速度或以下項目目前只能靜態確認：……`
* `設定部分完成，尚未完成的項目是：……`
* `設定未完成，原因是：……`

不得在速度未經驗證時使用第一個結論。
