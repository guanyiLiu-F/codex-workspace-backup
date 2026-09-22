# Codex Workspace Backup

Codex Desktop keeps projects, conversation assignments, and custom sidebar sections in local application state. After a crash, forced restart, power loss, or an interrupted update, that relationship data may no longer appear correctly in the sidebar. The conversations may still exist, but projects can disappear and previously organized conversations can be scattered back into the general task list. Rebuilding every project and moving every conversation by hand is slow and error-prone.

**Codex Workspace Backup** is a lightweight Windows utility that creates small snapshots of this relationship metadata and restores it when the Codex sidebar loses its organization.

> This is an unofficial local utility for Windows and is not affiliated with OpenAI.

## What it protects

- Project names, workspace roots, ordering, and ID mappings
- Conversation-to-project assignments
- Custom sidebar section definitions
- Projects and conversations placed inside each section
- Relevant sidebar ordering and display state

## What it does not back up

This is intentionally not a full Codex data backup. It does **not** copy:

- Conversation bodies or session files
- Images and attachments
- Authentication credentials
- Logs, plugins, models, or application binaries
- `config.toml` or unrelated Codex settings

Because only relationship metadata is saved, a typical backup is measured in kilobytes rather than hundreds of megabytes.

## Download

Download the latest `CodexWorkspaceBackup.zip` from the repository's [Releases](https://github.com/guanyiLiu-F/codex-workspace-backup/releases/latest) page, then extract it to a normal writable folder such as Desktop or Documents.

The portable package contains:

```text
CodexWorkspaceBackup/
├─ CodexWorkspaceBackup.exe
├─ README.md
└─ CodexWorkspaceBackups/
```

The application is self-contained. End users do not need to install the .NET SDK.

## How to use

### Create a backup

1. Run `CodexWorkspaceBackup.exe`.
2. Select **立即备份**.
3. Wait for the completion message.
4. Keep the generated JSON and SHA-256 files in `CodexWorkspaceBackups`.

Backup is designed to work while Codex is open. If the current Codex version refuses access to its local state, completely exit Codex and retry the backup.

### Restore projects and sidebar organization

1. Completely exit ChatGPT/Codex, including its system tray process.
2. Run `CodexWorkspaceBackup.exe`.
3. Select **恢复备份**.
4. Choose the required JSON snapshot from `CodexWorkspaceBackups`.
5. Review the confirmation dialog and start the restore.
6. Read the completion message and the generated `restore_report_*.txt` report.

The restore process merges data instead of deleting existing conversations. It creates or reuses projects, restores conversation assignments, creates or reuses native sections, moves the recorded items back into their sections, and writes the corresponding sidebar state.

## Safety and verification

- A lightweight `before_restore_*.json` safety snapshot is created before changes are applied.
- Existing conversations are not deleted by the restore operation.
- Projects, assignments, and section relationships are verified separately.
- Partial failures are written to `restore_report_*.txt` instead of being reported as a complete success.
- The main Codex state file and its backup copy are both updated and checked.

## Important limitations

- A backup must exist from before the metadata is lost. The tool cannot reconstruct relationships that were never recorded.
- Older snapshots without `sidebar-custom-sections-v3` may restore projects and conversation assignments but cannot recreate missing section membership.
- Codex is actively developed, so a future application update may change its local data format or app-server interface.
- Keep more than one recent backup and verify the restore report after every recovery.

## Requirements

- Windows 10 or Windows 11, x64
- Codex Desktop installed for the current Windows user
- Write access to the portable tool directory

All snapshots remain in the local `CodexWorkspaceBackups` folder. The utility does not upload backup files to an external service.
