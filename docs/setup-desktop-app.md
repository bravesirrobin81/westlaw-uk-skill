# Setup: Claude desktop app (macOS or Windows)

The Claude desktop app has two ways to give this skill a browser:

1. **Built-in browser pane** (Cowork). No Chrome required. The pane is a real
   browser with its own persistent profile inside the app, so you sign in to
   Dundee once and it sticks across sessions. Good for testing on a machine
   without Chrome.
2. **Claude in Chrome** connected to the desktop app. Same extension as the
   CLI path; uses your real Chrome profile.

The skill detects which one is present and adapts (see SKILL.md §0).

## Getting the skill into the desktop app

The desktop app's Claude Code sessions share `~/.claude` with the CLI, so a
user-scope plugin install is visible there:

```sh
claude plugin marketplace add git@gitea.freshlybrewed.coffee:leigh/westlaw-uk-skill.git
claude plugin install westlaw-uk@freshlybrewed
```

(The repo is private, so use the SSH URL. HTTPS fails with "unable to get
password".)

For a **Cowork** session, if the plugin does not show up automatically, add the
cloned repo folder as a plugin in the Cowork plugin settings, or paste the
contents of `skills/westlaw-uk/SKILL.md` into the session once. Confirm which
of these is needed on first test and update this doc.

## Testing on this Mac (no Chrome installed)

1. Open the Claude desktop app, start a **Cowork** session.
2. Make sure the Browser pane is available (globe icon, or Cmd+Shift+B).
3. Ask: *"Using the westlaw-uk skill, open Westlaw UK via the Dundee login and
   tell me when you're in."*
4. Claude opens the WAYFless URL in the pane and stops at the Dundee SSO page.
   If the pane is hidden, Claude will ask you to show it. Sign in there,
   approve MFA, link OnePass if asked, then say "done".
5. Approve the site prompts for `westlawuk.thomsonreuters.co.uk`,
   `signon.thomsonreuters.com` and `idp.dundee.ac.uk` when the app asks.

## Testing on Windows

Same steps. The Windows desktop app also has the built-in browser pane
(Ctrl+Shift+B). If you would rather use your real Chrome profile, install the
*Claude in Chrome* extension and connect it to the desktop app; then the skill
uses the Chrome backend instead. The WSL restriction in `setup-windows.md`
applies only to the CLI `--chrome` path, not to the desktop app.

## One seat

Whichever backend you use, Westlaw allows one session per user. Sign out of
Westlaw in any other browser before testing, or expect "signed in elsewhere".
