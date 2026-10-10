# AI Dev Monitor

**See what AI-assisted development costs you, per project, model, day and feature.**

AI Dev Monitor is a desktop app for Windows. It reads the history your AI coding tools already keep on your computer, prices it, and ties it to your commits. Everything stays on your machine: the app makes no network calls.

Today it reads **Claude Code**. This is an early build for testers.

## Download

Get the latest ZIP from the [**Releases**](https://github.com/JazonraeB/ai-dev-monitor-releases/releases/latest) page.

1. Download `AI-Dev-Monitor-<version>-windows-x64-portable.zip`.
2. Extract it to a folder you keep, for example `C:\Users\<you>\Apps\AI Dev Monitor`. Run it from the extracted folder, not from inside the ZIP.
3. Double-click **AI Dev Monitor.exe**. A short setup finds your projects the first time.

Requirements: Windows 10 or 11, 64-bit. Nothing else to install.

### "Windows protected your PC"

This early build is not code-signed yet, so Windows SmartScreen warns the first time you run it. Click **More info → Run anyway**.

To check that your download is genuine, compare its SHA-256 with the `.sha256` file published next to the ZIP:

```powershell
Get-FileHash .\AI-Dev-Monitor-0.1.0-windows-x64-portable.zip -Algorithm SHA256
```

## What you get

- **Overview:** total cost, daily cost with spending spikes marked, cost by model and by project.
- **Projects:** every folder you've used Claude Code in, with cost, sessions and last activity.
- **Features:** cost per feature from your `docs/FEATURES.md`, down to the sessions and commits behind each one.
- **Budgets and alerts:** monthly budgets with a forecast, and alerts for spikes, week-over-week jumps and new models.
- **Settings:** auto-import, time zone, alert rules.

Costs are **API-equivalent list prices**: what your usage would cost at the provider's published prices. They are not your invoice. If you're on a subscription plan, you pay the plan price.

The full [**User Guide**](USER_GUIDE.md) covers every screen, how costs are calculated, and how to link costs to features exactly with the open-source [`aidus`](https://github.com/JazonraeB/aidus) tool.

## Your data

- Nothing is uploaded. The app has no account, no telemetry and no network calls.
- It stores token counts, models, timestamps, cost and project paths. It never stores prompts, code or responses.
- Your database lives in `%LOCALAPPDATA%\AIDevMonitor`. Delete that folder to remove everything.

## Feedback

Found a bug or have a suggestion? [Open an issue](https://github.com/JazonraeB/ai-dev-monitor-releases/issues). Please don't paste prompts, code or anything private; the app's **Data sources** screen and `"AI Dev Monitor.exe" --self-test` give safe details to include.

## License

AI Dev Monitor is proprietary software, provided here for evaluation. See [LICENSE.md](LICENSE.md). The ZIP includes the licenses of the third-party components it uses (`licenses` folder and `THIRD-PARTY-NOTICES.txt`).

This repository holds releases and documentation only. The source code is not published here.
