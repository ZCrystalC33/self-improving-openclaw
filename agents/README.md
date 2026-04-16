# 專家代理 (Specialist Agents)

> 最後更新：2026-04-15

---

## 代理列表

| Agent | 用途 | 專精領域 |
|-------|------|---------|
| **quant-researcher** | 量化交易研究 | 量化策略、風險管理、演算法交易 |
| **defi-researcher** | DeFi 研究 | 去中心化金融、智能合約、區塊鏈應用 |
| **data-agent** | 數據分析 | 數據工程、ML、數據治理 |
| **marketing-agent** | 行銷增長 | 數位行銷、內容創作、品牌建設 |
| **pm-agent** | 產品管理 | 產品策略、用戶研究、敏捷開發 |

---

## 如何使用

### 使用sessions_spawn 呼び出す

```javascript
// 舉例：召喚量化研究專家
sessions_spawn({
  task: "研究比特幣期現套利策略",
  label: "quant-bitcoin-arbitrage",
  runtime: "subagent",
  agentId: "quant-researcher"  // 或手動傳入 SOUL.md 內容
})
```

### 手動指定 SOUL

如果還沒配置 agentId，可以直接在工作目錄指定 SOUL.md：

```javascript
sessions_spawn({
  task: "研究比特幣期現套利策略",
  label: "quant-bitcoin-arbitrage",
  runtime: "subagent",
  cwd: "~/self-improving/agents/quant-researcher"
})
```

---

## 各代理 SOUL.md 位置

```
~/self-improving/agents/
├── quant-researcher/
│   └── SOUL.md
├── defi-researcher/
│   └── SOUL.md
├── data-agent/
│   └── SOUL.md
├── marketing-agent/
│   └── SOUL.md
└── pm-agent/
    └── SOUL.md
```

---

## 未來擴展

可以繼續細分：

- `security-agent` — 滲透測試、資安審計
- `legal-agent` — 法規合規
- `trading-agent` — 加密貨幣交易
- `writer-agent` — 商業寫作

---

*由 Ophelia 維護*
