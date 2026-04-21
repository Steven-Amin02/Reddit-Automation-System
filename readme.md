# Reddit Automation System (UiPath)

UiPath automation project that logs in to Reddit, updates the profile picture, and creates text posts.

## Features
- Login to Reddit (Microsoft Edge / Modern UI Automation)
- Update profile picture (upload from a local file path)
- Create a **text** post to a specified subreddit
- Designed for reliability (recommended: Retry Scope + element-based verification)

> Note: Your repository may currently have workflows at the root. The structure above is the target best-practice layout for maintainability.
## Requirements
- Windows
- UiPath Studio (Modern experience recommended)
- Microsoft Edge
- UiPath packages (as in `project.json`):
  - `UiPath.System.Activities`
  - `UiPath.UIAutomation.Activities`

## Setup
1. Clone the repository.
2. Open `project.uiproj` in UiPath Studio.
3. Restore dependencies when prompted.
4. Configure credentials and inputs:
   - **Credentials:** Use Orchestrator Credential Assets (recommended) or Windows Credential Manager.
   - **Do not store passwords in plain text** in workflows or Excel.

## How to Run
1. Open `Main.xaml`.
2. Run in Debug mode.

### Suggested Inputs (Config)
If you use an external config file (e.g., `Data/Config.xlsx`), store values such as:
- `RedditUrl` (default: `https://www.reddit.com`)
- `Username`
- `ProfileImagePath`
- `Subreddit`
- `PostTitle`
- `PostBody`
- Feature toggles: `DoUpdateProfilePic`, `DoCreatePost`

## Reliability Notes (important)
- Prefer **element-based waits** (`Check App State`, `Element Exists`) over long fixed delays.
- Wrap **submit + verify** in a **Retry Scope** (e.g., 3 retries) for login.
- Reddit may occasionally trigger CAPTCHA or additional verification steps; unattended automation may fail in those cases.

## Security Notes
- Do not commit secrets (passwords, tokens, cookies).
- Use SecureString / Credential activities for passwords.
- Keep `.gitignore` updated to exclude generated/studio artifacts where appropriate.

## Disclaimer
This project is for educational purposes. Ensure your automation complies with Reddit’s Terms of Service and community rules, and avoid spammy behavior.
