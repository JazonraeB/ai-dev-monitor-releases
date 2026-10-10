# AI Dev Monitor — User Guide

AI Dev Monitor shows what AI-assisted development costs you, per project, model, day and feature. It reads the history your AI coding tools already keep on your computer, prices it, and ties it to your commits. **Nothing is uploaded.**

Today it reads **Claude Code**. Other agents and provider billing data come in later versions.

## Install (portable edition)

1. Download `AI-Dev-Monitor-<version>-windows-x64-portable.zip`.
2. Extract it to a folder you keep, for example `C:\Users\<you>\Apps\AI Dev Monitor`. Run it from the extracted folder, not from inside the ZIP.
3. Double-click **AI Dev Monitor.exe**.

**"Windows protected your PC"?** This early build isn't code-signed yet, so Windows SmartScreen may warn you the first time. Click **More info → Run anyway**. To check the download is genuine, compare its SHA-256 with the one published next to it:

```powershell
Get-FileHash .\AI-Dev-Monitor-0.1.0-windows-x64-portable.zip -Algorithm SHA256
```

## First start

A short setup appears the first time:

1. **Welcome.** What the app does.
2. **Choose the projects to track.** The app looks in your Claude Code history (`%USERPROFILE%\.claude\projects`) and lists every folder you've used Claude Code in, with its number of sessions and when it was last active.
   - Folders used in the last 90 days are ticked. Untick any you don't want.
   - Sessions started in a subfolder count toward the folder above it.
   - **Add a folder…** adds a project the list missed.
   - **Choose logs folder…** if your Claude Code history is somewhere else.
3. **Import.** The app reads the history of the projects you picked. A large history (hundreds of MB) can take a minute. Then it explains how to read the costs.

You can **Skip setup** at any step and add projects later on the **Projects** screen. **Settings → Run setup again** brings it back: projects you already track are marked **Already tracked**, and only newly ticked folders are added. Setup never removes a project; use **Stop tracking** on the Projects screen for that.

## The screens

| Screen | What it shows |
| --- | --- |
| **Overview** | Total cost for the period (7 days, 30 days or a quarter), the change vs the period before, tokens, sessions, cache hit rate; daily cost bars (orange = a spending spike); cost by model and by project; budgets; recent alerts; cost per feature. **Export report** writes a CSV of every response in the period (metadata and cost only, never content). |
| **Projects** | Your registered projects with cost, sessions and last activity. **Add project folder** registers another. **Initialize .ai-usage** is the only action that writes into a project (see below); it asks first. **Stop tracking** removes a project from every report and from imports; nothing is deleted, and adding its folder again brings it back with its history. |
| **Features** | Cost per feature, from each project's `docs/FEATURES.md`, and how each link was made. See [Linking cost to features](#linking-cost-to-features). |
| **Budgets & alerts** | Monthly budgets for everything, a project or a provider, with a month-end forecast from day 3 and alerts at 50 / 80 / 100%. Below them, alerts from the spending rules, each saying which rule fired and why. **Acknowledge** clears one. |
| **Data sources** | What the app reads, recent import runs, and anything it rejected (quarantine shows only a location and a reason, never content). **Import now** runs an import immediately. |
| **Settings** | See [Settings](#settings). |

## How costs are calculated

Costs are **API-equivalent list prices**: what your usage would cost at the provider's published per-token prices. **They are not your invoice.** If you're on a subscription plan, you pay the plan price; the figure here shows what the same work would cost through the API.

Every cost carries a status, shown as the coloured bar on the Overview:

| Status | Meaning |
| --- | --- |
| **Authoritative** | From the provider's own billing data. Not available yet (provider APIs come in a later version). |
| **Calculated** | Exact token counts × the published price list. Most of your cost. |
| **Estimated** | The token counts were not recorded by a tool (for example, entered by hand), so the cost is an estimate. |
| **Unknown** | No price for that model yet. Shown as $0 and flagged, never guessed. |

Days are counted in your time zone (Settings → Dates). Exports keep timestamps in UTC.

## Linking cost to features

The Features screen needs two things in a project:

1. **A feature list** in `docs/FEATURES.md`, as a table whose first columns are ID, title and status:

   ```markdown
   | ID | Title | Status |
   | --- | --- | --- |
   | FEAT-1 | Login | Done |
   | FEAT-2 | Export to CSV | In progress |
   ```

   A list works too: `- FEAT-1: Login`.

2. **Links from AI sessions to commits.** From strongest to weakest:
   - **Trailer:** the commit carries `AI-Session:` (and optionally `Feature:`) trailers. Exact. Set up with the free `aidus` tool, below.
   - **Branch:** the branch name contains the feature id, such as `feat/FEAT-2-export`.
   - **Commit message:** the message names the feature id.
   - **Inferred:** no trailer, so a session that ended shortly before the commit is linked to it (30 minutes by default; Settings → Commit links). Marked "Inferred".

A session that produced several commits shares its cost between them.

### Exact links with aidus (optional)

[`aidus`](https://github.com/JazonraeB/aidus) is a small open-source tool (Apache-2.0, Python 3.10+) that records AI usage into the repository and adds commit trailers. In a project:

```bash
pip install "git+https://github.com/JazonraeB/aidus#subdirectory=cli"
aidus init --hooks
```

`aidus init` creates `.ai-usage/` in the repository, and `--hooks` installs a `prepare-commit-msg` hook that adds the trailers. Then add the Claude Code hooks from the [aidus README](https://github.com/JazonraeB/aidus/tree/main/cli#recording-claude-code-automatically) to the project's `.claude/settings.json`. The hook never blocks a commit: if anything fails, the commit goes ahead without trailers.

**Initialize .ai-usage** on the Projects screen does the `aidus init` part for you (without the hook).

## Settings

| Section | Options |
| --- | --- |
| **Importing** | Import automatically (on by default) and how often (1 min to 1 hour); the Claude Code logs folder. |
| **Dates** | The time zone days are counted in: your Windows time zone (default), UTC, or any named zone. Type to search. |
| **Commit links** | The link window for commits without trailers (5–240 min, default 30). Saving a change re-links your history. |
| **Alerts** | Windows notifications; each spending rule on or off with its threshold: spending spike (a day well above your usual), week-over-week jump, new model in a project. Budget alerts are set per budget. |
| **About** | Version, pricing catalog, data folder (**Open folder**), **Run setup again**, this guide. |

Changes apply when you click **Save changes**. **Reset to defaults** fills in the defaults without saving.

## Your data and privacy

- Everything stays on this computer. The app makes no network calls.
- It stores token counts, models, timestamps, cost and project paths. **It never stores prompts, code or responses.**
- Commit authors are stored only as a salted hash.
- Your database and settings live in `%LOCALAPPDATA%\AIDevMonitor`, never inside a project or the app folder.
- The app writes into a project only when you click **Initialize .ai-usage**, and then only `.ai-usage/` plus one `.gitignore` line.

## Update and uninstall

- **Update:** close the app, then replace the app folder with the new version's. Your data is kept.
- **Uninstall:** delete the app folder. To also remove your data, delete `%LOCALAPPDATA%\AIDevMonitor`.

## Troubleshooting

| Problem | What to do |
| --- | --- |
| Setup finds no projects | Your Claude Code history may be elsewhere: **Choose logs folder…**, or **Add a folder…** for each project. |
| A project shows no cost | Check it was ticked during setup (or add it on Projects), then **Import now** on Data sources. Only sessions started inside the project folder count. |
| Costs look a day off | Check Settings → Dates. |
| "Unknown" costs | A model the price list doesn't know yet. It's flagged, not guessed; a later version's price list will cover it. |
| Something in quarantine | The app skipped input it couldn't read safely. The location and reason are listed on Data sources. |
| Does this build work? | Run `"AI Dev Monitor.exe" --self-test`. It starts against a temporary database, opens every screen and exits. The result is in `%TEMP%\ai-dev-monitor-self-test.log`. |
