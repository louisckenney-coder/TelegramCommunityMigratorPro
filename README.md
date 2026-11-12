# Telegram Community Migrator Pro

A Windows desktop application for **consent-based Telegram community migration workflows**. It stores records locally in SQLite and creates Telegram join-request invitation links for contacts who have opted in.

## Important Telegram limitations

- The Telegram Bot API does not provide a general-purpose way to list or scrape every member of arbitrary groups.
- A bot must be added to a group and have the required administrator permissions to inspect that chat or create invitation links.
- This app does not bypass privacy controls, scrape arbitrary group members, or silently add accounts to groups.
- CSV imports must contain an explicit consent field. Rows without affirmative consent are skipped. Invitation links require an affirmative consent record and destination-group setup.
- A join-request link lets the invited person decide whether to join; it does not guarantee that they will join.

## Build the Windows installer automatically

The source repository includes `.github/workflows/windows-build.yml`. To build the actual Windows installer:

1. Create a GitHub repository and upload the contents of this project.
2. In GitHub, open **Actions** and select **Build Windows installer**.
3. Click **Run workflow** and wait for the job to finish.
4. Open the completed workflow run and download the artifact named `TelegramCommunityMigratorPro-Windows-Installer`.
5. Extract it. The installer is named `TelegramCommunityMigratorPro_Setup.exe`.

The workflow runs on a Windows GitHub runner, installs the dependencies, rebuilds the SQLite native module for Electron, and packages an NSIS installer with desktop and Start Menu shortcuts and an uninstaller.

## Local development / build

Requires Node.js 22 and npm. On Windows:

```powershell
npm install
npm run start
npm run dist:win
```

The installer is written to `release/`.

## First launch

1. Install and launch the application.
2. Create a Telegram bot with [@BotFather](https://t.me/BotFather) and paste its bot token under **Connect Telegram**.
3. Add the bot to the destination group as an administrator with permission to invite users.
4. Save source and destination group references.
5. Import a CSV of contacts who have explicitly consented to receive an invitation. Required headers: `name,username,telegram_id,source,destination,consent`. Use `TRUE` in the consent column.
6. Under **Migration Manager**, select a consented member and destination group, then generate a join-request invitation.

## Data and security

SQLite database and bot token are stored under Electron's per-user `userData` directory. The token is stored locally in the SQLite settings table; protect your Windows account and do not share the database. For production deployments, consider Windows Credential Manager/DPAPI encryption and code signing.

## Included functionality

- Native Electron desktop window
- Local SQLite database created on first launch
- Telegram Bot API token validation (`getMe`)
- Chat verification (`getChat`)
- Source and destination group records
- CSV import with consent filtering and duplicate detection
- CSV export
- Consent records and audit history
- Join-request invite creation (`createChatInviteLink`)
- NSIS installer, desktop shortcut, Start Menu shortcut, and uninstaller
- GitHub Actions Windows build workflow

Excel `.xlsx` import/export, full personal-account (MTProto) authentication, and automatic membership discovery are not implemented in this initial version. Use CSV with Excel for data exchange.
