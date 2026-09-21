# Codex Workspace Backup

Lightweight backup and restore tool for Codex projects, conversation assignments, and custom sidebar sections.

## Portable layout

```text
CodexWorkspaceBackup/
├─ CodexWorkspaceBackup.exe
├─ README.md
└─ CodexWorkspaceBackups/
```

The release folder is portable. You can move it to another location or keep it on a USB drive. Backup files are stored beside the EXE in `CodexWorkspaceBackups`.

## Use the folder version

- Run `Backup.cmd` at any time. Codex may remain open for a lightweight backup.
- Run `Restore.cmd` only after completely quitting ChatGPT/Codex, including the system tray process.
- Restore creates or reuses projects, restores conversation assignments, recreates native sections, moves projects and conversations into their sections, and writes the sidebar state.

The PowerShell implementation is `codex-project-tool.ps1`.

## Use the EXE version

Double-click `CodexWorkspaceBackup.exe`. The product name and file names are English, while the main operation labels remain Chinese for convenience.

## Build a new EXE

Run `Build.cmd`, or run `Build.ps1 -NoPause` from PowerShell. The script publishes a self-contained Windows x64 single-file EXE into `CodexWorkspaceBackup`.

The first build may download .NET 8 Windows runtime packs from NuGet. The end-user EXE does not require the .NET SDK.

## Backup contents and limits

The lightweight backup contains project names, roots, ordering, ID mappings, conversation-to-project assignments, native section definitions, section membership, and sidebar state. It does not contain conversation bodies, attachments, logs, authentication data, plugins, or model configuration.

Always create a new backup with this version before relying on section restoration. Older backups that do not contain `sidebar-custom-sections-v3` can restore projects and assignments but cannot reconstruct section membership that was never saved.

## Verification

Restore creates a `restore_report_*.txt` file and verifies projects, conversation assignments, and section relationships separately. Existing projects and conversations are merged; the tool does not delete them.
