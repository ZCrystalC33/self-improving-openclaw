# Self-Improving Agent - Setup Guide

## Overview

Self-Improving enables your AI to learn from corrections, track patterns, and compound knowledge over time. The P1+P2+P3 enhancements add:

- **P1**: Context prediction + auto-indexing
- **P2**: Cold/Hot layer automatic exchange
- **P3**: FTS5 bidirectional integration

---

## Quick Install

```bash
# 1. Clone repo
git clone https://github.com/ZCrystalC33/self-improving-openclaw.git ~/self-improving

# 2. Verify structure
ls ~/self-improving/
# Should have: memory.md, corrections.md, index.md, domains/, scripts/, archive/, projects/

# 3. Set up cron for auto-maintenance (optional)
echo "0 3 * * * $HOME/self-improving/scripts/exchange-cron.sh" | crontab -
```

---

## Prerequisites

- Python 3.7+
- OpenClaw installed
- Optional: FTS5 skill (for P3 integration)

---

## Directory Structure

```
~/self-improving/
├── memory.md              # HOT: ≤100 lines, always loaded
├── corrections.md         # Last 50 corrections
├── index.md               # Auto-generated index
├── heartbeat-state.md     # Heartbeat state tracking
├── domains/               # WARM: Topic-specific learnings
│   └── openclaw-fts5.md  # Example domain file
├── projects/              # Project-specific learnings
├── archive/               # COLD: 30+ days inactive
├── agents/                # Sub-agents config
├── scripts/               # Enhancement scripts (P1/P2/P3)
│   ├── context_predictor.py   # P1: Context analysis
│   ├── reindex.py             # P1: Auto-index
│   ├── exchange_engine.py     # P2: Layer exchange
│   ├── exchange-cron.sh       # P2: Cron hook
│   └── fts5_integration.py    # P3: FTS5 sync
└── .gitignore
```

---

## Setup Steps

### Step 1: Create Directory Structure

```bash
mkdir -p ~/self-improving/{projects,domains,archive,scripts}
```

### Step 2: Initialize Core Files

Create `memory.md`:
```bash
cat > ~/self-improving/memory.md << 'EOF'
# Memory (HOT Tier)

## Preferences
- (user preferences go here)

## Patterns
- (repeated patterns go here)

## Rules
- (learned rules go here)
EOF
```

Create `corrections.md`:
```bash
cat > ~/self-improving/corrections.md << 'EOF'
# Corrections Log

| Date | What I Got Wrong | Correct Answer | Status |
|------|-----------------|----------------|--------|
EOF
```

Create `index.md`:
```bash
cat > ~/self-improving/index.md << 'EOF'
# Knowledge Index

> Auto-generated. Last update: never

## Statistics
| Metric | Value |
|--------|-------|
| Total entries | 0 |
| Hot topics | 0 |
| Warm domains | 0 |
| Cold archived | 0 |
EOF
```

Create `heartbeat-state.md`:
```bash
cat > ~/self-improving/heartbeat-state.md << 'EOF'
# Heartbeat State

last_heartbeat_started_at: never
last_reviewed_change_at: never
last_heartbeat_result: never

## Last actions
- none yet
EOF
```

### Step 3: Add to SOUL.md

Add this section to your `SOUL.md`:

```markdown
**Self-Improving**
Before non-trivial work, load `~/self-improving/memory.md` and only the smallest relevant domain or project files.
After corrections, failed attempts, or reusable lessons, write one concise entry to the correct self-improving file immediately.
Prefer learned rules when relevant, but keep self-inferred rules revisable.
Do not skip retrieval just because the task feels familiar.
```

### Step 4: Add to HEARTBEAT.md

Add this section to your `HEARTBEAT.md`:

```markdown
## Self-Improving Check

- Read `~/self-improving/heartbeat-rules.md` (if exists)
- Use `~/self-improving/heartbeat-state.md` for last-run markers
- If no file inside `~/self-improving/` changed since last review, return `HEARTBEAT_OK`
```

### Step 5: Configure Enhancement Scripts (Optional)

**Context Predictor (P1)**
```python
# Use in your agent code:
from ~/self-improving/scripts.context_predictor import analyze_text

analysis = analyze_text("上次我們談的 FTS5")
# Returns: topics, intents, suggested_memory_load
```

**Cold/Hot Exchange (P2)**
```bash
# Run manually (or via cron)
python3 ~/self-improving/scripts/exchange_engine.py

# Or set up automatic daily maintenance
echo "0 3 * * * $HOME/self-improving/scripts/exchange-cron.sh" | crontab -
```

**FTS5 Integration (P3)**
```python
# Index a correction
from ~/self-improving/scripts.fts5_integration import index_correction
index_correction("User said I was wrong about API key format")

# Get FTS5 context
from ~/self-improving/scripts.fts5_integration import get_fts5_context_for_topic
context = get_fts5_context_for_topic("FTS5")
```

---

## Usage

### Basic Corrections
```python
# When user corrects you:
with open("~/self-improving/corrections.md", "a") as f:
    f.write(f"\n- {datetime.now().date()}: {correction_text}")
```

### Context-Aware Response
```python
from ~/self-improving.scripts.context_predictor import analyze_text

analysis = analyze_text(user_message)
if "上次" in user_message:
    # Suggest loading FTS5 context
    pass
if analysis["intents"] == ["correction"]:
    # Log to corrections.md
    pass
```

### Memory Maintenance
```bash
# Run reindex anytime
python3 ~/self-improving/scripts/reindex.py

# Run exchange cycle
python3 ~/self-improving/scripts/exchange_engine.py

# Check integration status
python3 ~/self-improving/scripts/fts5_integration.py status
```

---

## Layer Exchange Rules (P2)

| From | To | Condition |
|------|-----|-----------|
| HOT (memory.md) | WARM (domains/) | Not referenced for 7+ days |
| WARM (domains/) | COLD (archive/) | Not referenced for 30+ days |
| COLD (archive/) | WARM (domains/) | Referenced 3+ times |

---

## Troubleshooting

**"Module not found" errors**
```bash
# Ensure Python path is correct
export PYTHONPATH=$PYTHONPATH:~/self-improving/scripts
```

**FTS5 integration not working**
```bash
# Check FTS5 is installed
ls ~/.openclaw/skills/fts5/

# Run integration status
python3 ~/self-improving/scripts/fts5_integration.py status
```

**Cold/Hot exchange not running**
```bash
# Check cron is set
crontab -l

# Manual run
python3 ~/self-improving/scripts/exchange_engine.py
```

---

## Verification

Run this to verify setup:

```bash
python3 ~/self-improving/scripts/reindex.py
# Should output: "✅ Index updated"

python3 ~/self-improving/scripts/fts5_integration.py status
# Should show: fts5_available: True/False
```

Check structure:
```bash
ls -la ~/self-improving/
# Should have: memory.md, corrections.md, index.md, domains/, scripts/
```

---

## License

MIT License - See LICENSE file