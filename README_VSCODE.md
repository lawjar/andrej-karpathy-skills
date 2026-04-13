# 在 VS Code 中使用 Karpathy Guidelines

本文說明如何將 [Karpathy-Inspired Claude Code Guidelines](./README.md) 應用於 VS Code 開發環境，透過 GitHub Copilot 自訂指令讓 AI 助理遵循四大行為原則。

---

## 四大原則速覽

這份指引源自 [Andrej Karpathy 對 LLM 編程缺陷的觀察](https://x.com/karpathy/status/2015883857489522876)，針對四個常見問題提供對應原則：

| 原則 | 解決的問題 |
|------|-----------|
| **先思考，再編碼** | AI 默默做出假設、隱藏困惑、不說明取捨 |
| **簡單優先** | 過度設計、膨脹的抽象層、1000 行能用 100 行解決的事 |
| **精準修改** | 改動無關程式碼、順手重構沒問題的東西 |
| **目標驅動執行** | 任務描述模糊、缺乏可驗證的成功標準 |

---

## 在 VS Code 中安裝

### 方式一：專案層級（推薦）

在專案根目錄建立 `.github/copilot-instructions.md`，將指引內容貼入。GitHub Copilot 會自動讀取此檔案作為整個專案的自訂指令。

```bash
# 下載並建立 Copilot 自訂指令檔
curl -o .github/copilot-instructions.md \
  https://raw.githubusercontent.com/lawjar/andrej-karpathy-skills/main/CLAUDE.md
```

> **注意**：若 `.github/` 目錄不存在，請先執行 `mkdir -p .github`。

### 方式二：使用者層級（全域套用）

在 VS Code 設定中加入自訂指令，對所有專案生效：

1. 開啟命令列（`Ctrl+Shift+P` / `Cmd+Shift+P`）
2. 搜尋並執行 **"GitHub Copilot: Open User Instructions"**
3. 將 [CLAUDE.md](./CLAUDE.md) 的內容貼入

### 方式三：現有專案追加

若專案已有 `.github/copilot-instructions.md`，可將原則附加至末尾：

```bash
echo "" >> .github/copilot-instructions.md
curl https://raw.githubusercontent.com/lawjar/andrej-karpathy-skills/main/CLAUDE.md \
  >> .github/copilot-instructions.md
```

---

## 各原則說明與 VS Code 使用情境

### 1. 先思考，再編碼

**不做假設。不隱藏困惑。主動揭露取捨。**

在讓 Copilot 實作功能前，先描述清楚：
- 需要哪種格式？哪個欄位？哪個範圍？
- 如有多種實作方向，要求 Copilot 列出選項再動手

**VS Code 使用技巧：** 在 Copilot Chat 中，先用 `#file` 或 `#selection` 指定範圍，並在提示語中加上「先列出你的假設」。

---

### 2. 簡單優先

**用最少的程式碼解決問題。不加猜測性功能。**

- 不要為了「未來可能需要」而加抽象層
- 200 行能用 50 行寫完，就重寫
- 自問：「資深工程師會說這太複雜嗎？」

**VS Code 使用技巧：** 在提示語加上「只實作必要功能，不加額外抽象」，避免 Copilot 生成過度設計的程式碼。

---

### 3. 精準修改

**只動該動的地方。只清理自己製造的垃圾。**

編輯既有程式碼時：
- 不「順便改進」旁邊的程式碼或格式
- 不重構沒有問題的東西
- 沿用現有程式碼風格

若你的修改讓某些 import / 變數 / 函式變成無用，才需要一併移除。

**VS Code 使用技巧：** 使用 `#selection` 精確限定 Copilot 的作業範圍，並在提示語加上「只修改與此任務直接相關的程式碼」。

---

### 4. 目標驅動執行

**定義可驗證的成功標準，執行到達標為止。**

將模糊任務轉換為可驗證目標：

| 模糊指令 | 可驗證目標 |
|---------|-----------|
| 「加上驗證」 | 「為無效輸入寫測試，讓測試通過」 |
| 「修這個 bug」 | 「寫一個能重現 bug 的測試，再讓它通過」 |
| 「重構 X」 | 「重構前後測試都要是綠燈」 |

多步驟任務請先列出計畫：

```
1. [步驟] → 驗證：[檢查方式]
2. [步驟] → 驗證：[檢查方式]
3. [步驟] → 驗證：[檢查方式]
```

**VS Code 使用技巧：** 在 Copilot Chat 描述任務時，加上「請列出步驟，並說明每個步驟如何驗證完成」。

---

## 確認指引生效

當四大原則發揮作用時，你會看到：

- ✅ **diff 更乾淨** — 只出現被要求的修改
- ✅ **第一次就夠簡單** — 不需要事後大改
- ✅ **提問在實作前** — 而不是做錯之後才問
- ✅ **PR 精簡** — 沒有不相關的順手重構

---

## 自訂與擴充

這份指引設計為可與專案規則合併使用。在 `.github/copilot-instructions.md` 末尾加入你的專案規範：

```markdown
## 專案規範

- 使用 TypeScript strict mode
- 所有 API 端點必須有測試
- 錯誤處理請參考 `src/utils/errors.ts` 的現有模式
```

---

## 注意事項

這份指引偏向**謹慎而非速度**。對於簡單的小修改（拼字錯誤、明顯的單行改動），可自行判斷是否套用完整流程。目標是減少複雜任務上的代價高昂的錯誤，而不是拖慢簡單任務的速度。
