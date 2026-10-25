# Setup: Windows host with Chrome

This is the primary configuration. Claude Code and Chrome run on the same
Windows machine; the extension and Claude Code talk over Chrome native
messaging, so they must be on the same OS.

## 1. Claude Code, native Windows

Install the **native** Windows build (PowerShell or Git Bash). Do **not** use
WSL for this: the Chrome integration checks for WSL and refuses
("Claude in Chrome is not supported in WSL at this time"). There are community
bridges for WSL2 but they are unsupported; if you must, see the issue tracker
links in the repo issues.

```powershell
irm https://claude.ai/install.ps1 | iex
claude --version      # need 2.0.73 or newer
claude                # then /login
```

Sign in with `/login` using your Anthropic account (Pro, Max, Team or
Enterprise). **API-key or long-lived-token auth disables the Chrome
integration** even with `--chrome`.

## 2. Chrome and the extension

1. Use Google Chrome stable (Edge, Brave, Arc are not supported by the extension).
2. Pick **one Chrome profile** and use it consistently. It should be the
   profile where you are, or will be, signed in to Dundee SSO.
3. Install *Claude in Chrome* from the Chrome Web Store. Version 1.0.36 or
   newer is needed for the Claude Code hookup.
4. Click the extension, sign in with the **same** Anthropic account as Claude Code.
5. In the extension settings, under site permissions, allow:
   - `westlawuk.thomsonreuters.co.uk`
   - `signon.thomsonreuters.com`
   - `idp.dundee.ac.uk`
   - `login.microsoftonline.com` (only if Dundee hands off to Entra for MFA)
   - `libguides.dundee.ac.uk` (optional, only used to re-derive the WAYFless URL)

   You can instead leave it on "ask each time" and approve the prompts the
   first run.

## 3. Install the plugin

```powershell
claude plugin marketplace add https://gitea.freshlybrewed.coffee/leigh/westlaw-uk-skill.git
claude plugin install westlaw-uk@freshlybrewed
```

## 4. First run

```powershell
claude --chrome
```

Inside the session run `/chrome` once; it should report the extension as
connected. Then:

> Open Westlaw UK via the Dundee login and tell me when you're in.

Claude opens a new tab at the WAYFless URL, lands on the Dundee SSO page, and
stops. **You** type your Dundee username and password in that tab, approve
MFA, and if prompted link a OnePass profile. When Westlaw's home page appears,
tell Claude "done". From then on it works inside your session.

## 5. Keeping the session alive

- Leave the Chrome profile open; closing Chrome drops the extension connection.
- Westlaw times out on inactivity. If Claude reports the SSO page again, just sign in again.
- Westlaw allows one concurrent session per user. Do not also sign in on a phone or another PC while a Claude task is running.

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| `/chrome` says extension not detected | Chrome running? Extension signed in to the same account? Not WSL? Restart Chrome then `claude --chrome` again. |
| Tools time out on Westlaw pages | Check the extension's site permission for `westlawuk.thomsonreuters.co.uk`. |
| Stuck on OnePass "registration key" | That is a Westlaw-side account-linking step; complete it manually once. Safari is known to be worse here; stay on Chrome. |
| "Signed in elsewhere" | Another session (phone, another browser) took the seat. Sign out there. |
