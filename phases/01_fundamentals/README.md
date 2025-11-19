# Phase 1 — Fundamentals

This phase establishes the baseline forensic environment inside a macOS Sonoma VM.  
The goal is to understand core macOS structures, collect foundational artifacts, and build a reference point for future analysis.

## 🎯 Objectives
- Understand key macOS forensic artifact locations
- Collect system, user, and application metadata
- Create a clean baseline snapshot for comparison
- Build initial Swift + Bash tools used later in the investigation

## 📂 Structure
- `scripts/` → Swift & Bash tools for artifact collection and enumeration
- `analysis/` → Notes, findings, baseline comparisons, screenshots

## 📌 Artifacts Collected in Phase 1
- Hardware & software metadata
- Unified logs (last 24 hours)
- Safari history (if present)
- User shell history
- Installed applications
- User login items and LaunchAgents

## 🚀 Usage
Run the baseline collection script:

```bash
### bash scripts/collect_basics.sh
Run the Swift app enumerator:


### swift scripts/list_apps.swift
Document findings in analysis/baseline.md.
```

---

# 🧰 **phases/01_fundamentals/scripts/collect_basics.sh**

```bash
#!/bin/bash

OUTPUT="./phase1_artifacts"
mkdir -p "$OUTPUT"

echo "[+] Collecting hardware + software info..."
system_profiler SPHardwareDataType > "$OUTPUT/hardware.txt"
system_profiler SPSoftwareDataType > "$OUTPUT/software.txt"

echo "[+] Collecting last 24h unified logs..."
log show --style syslog --last 24h > "$OUTPUT/unified_logs_24h.log"

echo "[+] Exporting shell history..."
cp ~/.zsh_history "$OUTPUT/zsh_history.txt" 2>/dev/null

echo "[+] Copying Safari history database (if it exists)..."
cp ~/Library/Safari/History.db "$OUTPUT/safari_history.db" 2>/dev/null

echo "[+] Listing LaunchAgents + LaunchDaemons..."
ls ~/Library/LaunchAgents > "$OUTPUT/user_launchagents.txt" 2>/dev/null
ls /Library/LaunchAgents > "$OUTPUT/system_launchagents.txt" 2>/dev/null
ls /Library/LaunchDaemons > "$OUTPUT/system_launchdaemons.txt" 2>/dev/null

echo "[+] Collecting installed apps..."
ls /Applications > "$OUTPUT/installed_apps.txt"

echo "[+] Phase 1 baseline artifact collection complete."
echo "    Artifacts saved to: $OUTPUT"
```
---

### Make executable:
```chmod +x scripts/collect_basics.sh```
