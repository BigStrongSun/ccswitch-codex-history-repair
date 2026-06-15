# CCSwitch Codex History Repair

Standalone repair tool for Codex Desktop history visibility issues seen after
switching Codex providers or using CCSwitchMulti MultiRouter.

The tool is intentionally a single-file Python program. It can be used directly
with Python, or packaged as a Windows exe for users who do not want to install
Python.

## What It Fixes

- Moves hidden Codex sessions into the active provider bucket.
- Repairs `has_user_event` using local rollout JSONL messages.
- Updates `session_index.jsonl` when Codex Desktop sidebar metadata is stale.
- Repairs workspace hints and projectless history state.
- Applies the current successful balanced recent-window baseline:
  `sourceFilter=vscode`, `maxPerProject=10`, `maxTotal=300`.
- Supports targeted repair by one or more `--session-id` values.

The default active database path is `~/.codex/sqlite/state_5.sqlite`, which is
the current Codex Desktop database location. Older scripts that only touched
`~/.codex/state_5.sqlite` may appear to succeed while the Desktop sidebar stays
unchanged.

## Python Version

Preview sessions:

```powershell
python .\ccswitch_history_repair.py list --limit 20
```

Preview the default repair without writing:

```powershell
python .\ccswitch_history_repair.py repair --project-path "C:\Users\sunda\Documents\LLMservice"
```

Apply the default repair:

```powershell
python .\ccswitch_history_repair.py repair --project-path "C:\Users\sunda\Documents\LLMservice" --apply
```

Target a specific session:

```powershell
python .\ccswitch_history_repair.py repair --session-id "<session-id>" --apply
```

Use `--json` for machine-readable output.

## Windows Exe Version

Build the exe and release assets:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\scripts\build-windows-exe.ps1
```

The script creates:

- `release\CCSwitchHistoryRepair_0.1.0_python.zip`
- `release\CCSwitchHistoryRepair_0.1.0_windows_x64.exe`
- `release\SHA256SUMS-v0.1.0.txt`

Run the exe:

```powershell
.\release\CCSwitchHistoryRepair_0.1.0_windows_x64.exe list --limit 20
.\release\CCSwitchHistoryRepair_0.1.0_windows_x64.exe repair --apply
```

## Safety

Write mode creates a timestamped backup under Codex home before mutation. Close
Codex Desktop before applying repairs. Use `--force` only when you explicitly
accept the risk of Codex Desktop rewriting the same SQLite/global-state files
during repair.
