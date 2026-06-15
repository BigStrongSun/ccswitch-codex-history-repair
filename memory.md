# CCSwitch Codex History Repair Memory

- 2026-06-16: This repository packages the currently working Codex Desktop history visibility repair as a standalone Python CLI plus Windows exe release asset.
- Public repository: `https://github.com/BigStrongSun/ccswitch-codex-history-repair`.
- v0.1.0 release: `https://github.com/BigStrongSun/ccswitch-codex-history-repair/releases/tag/v0.1.0`, with `CCSwitchHistoryRepair_0.1.0_python.zip`, `CCSwitchHistoryRepair_0.1.0_windows_x64.exe`, and `SHA256SUMS-v0.1.0.txt`.
- The active Codex Desktop database baseline is `~/.codex/sqlite/state_5.sqlite`. Older scripts that only modify `~/.codex/state_5.sqlite` can appear to succeed while the Desktop sidebar remains unchanged.
- The default repair baseline is the balanced recent-window flow: `sourceFilter=vscode`, `maxPerProject=10`, `maxTotal=300`, with targeted `--session-id` repair available for exact sessions.
- The default source provider set must include `openai`, `custom`, `codex_model_router_v2`, `cc_switch_codex_router`, and `codex_model_router`, because hidden history may live in any of those buckets after provider switches.
- The target provider should prefer the live top-level `model_provider` in `~/.codex/config.toml`; only fall back to `codex_model_router_v2` when live config cannot provide a value.
- Write mode must create backups under Codex home before mutation. Users should close Codex Desktop before `repair --apply`; `--force` is only for deliberate repair while Codex-related processes may still be running.
- Release build command: `powershell -NoProfile -ExecutionPolicy Bypass -File .\scripts\build-windows-exe.ps1`.
- v0.1.0 verification performed locally: `python -m py_compile ccswitch_history_repair.py`, Python `--version`/`--help`, Python `list --limit 3 --json`, exe `--version`/`--help`, and exe `list --limit 1 --json`.
