# Setup: Android

## Short answer

Android cannot run the browser side. Chrome for Android has no extension
support, and the *Claude in Chrome* extension needs native messaging to a
Claude Code process on the **same machine**. Edge Canary and Firefox on Android
can load some extensions, but not this one, and there is no Claude Code
binary for Android.

What **is** feasible: run Claude Code and Chrome on a desktop (Windows per
`setup-windows.md`, or macOS) and drive that session from your phone with
Claude Code **Remote Control**. Chrome does the Westlaw work on the desktop;
you type prompts and read results on the phone.

## Setup

### Desktop side (once, then leave it running)

1. Complete `docs/setup-windows.md` or `docs/setup-macos.md`, including a
   successful first Westlaw login in Chrome.
2. Keep Chrome open on the Westlaw-signed-in profile.
3. Stop the machine sleeping. Windows: *Settings → System → Power → Screen and
   sleep → Never* while plugged in. The screen may lock; Chrome keeps running.
4. Start a remote-controllable session in the research directory:

   ```powershell
   cd C:\Users\you\research
   claude --chrome --remote-control
   ```

   Or, inside an existing session, type `/remote-control`. Claude Code prints
   a link and a QR code.

### Phone side

1. Install the **Claude** app from Google Play. Sign in with the same account
   used for `/login` on the desktop. (In Claude Code, `/mobile` shows a QR
   code for the app.)
2. Open the app, tap the **Code** tab. Your desktop appears as a device card,
   and any session started with `--remote-control` appears in the list.
3. Tap the session. You are now typing into the desktop Claude Code. Ask it
   the Westlaw question as normal. The skill runs on the desktop and Chrome
   there does the browsing.

## Second option: phone session linked to the desktop app's browser

If the desktop runs the **Claude desktop app** rather than the CLI, a session
started from the Claude Android app can be linked to that computer and use its
built-in browser pane. The tools then carry the `mcp__remote-devices__Claude_Browser__`
prefix and the skill handles that. Site approvals can be answered from the
phone. The desktop app must be open and online. Same MFA caveat: the Dundee
form is on the desktop pane, not the phone. See `setup-desktop-app.md`.

## The MFA problem

When Westlaw's session expires, the skill stops and asks you to log in. The
login form is in the **desktop** Chrome tab, not on your phone. Options, best
first:

1. **Log in before you leave the desk.** Do a Westlaw task from the desktop
   first so the Westlaw session is warm; then go mobile. Fine for an hour or so of work.
2. **Remote desktop from the phone** (RustDesk, Chrome Remote Desktop, RDP).
   Fill the Dundee form on the desktop screen from the phone, approve the
   Authenticator push, tell Claude "done" in the app. Clunky but works.
3. If Dundee's MFA is a Microsoft Authenticator **push**, the push itself
   arrives on your phone anyway; only the username/password form needs the
   remote desktop step.

Do **not** ask Claude to type your credentials to get around this. The skill
refuses, and it is the right refusal.

## Constraints

- Remote Control needs the desktop online and the Claude Code process alive. If the laptop lid closes, the session goes away.
- Remote Control availability depends on plan; it launched for Max and later widened. Teams/Enterprise admins may need to enable it.
- One Westlaw seat per user: do not open Westlaw in the phone's own browser while a remote session is working.
- Pages, screenshots and GIFs the skill records stay on the desktop filesystem.

## Not feasible, and why

| Idea | Problem |
|------|---------|
| Claude in Chrome on Android Chrome | No extensions on Android Chrome |
| Kiwi / Edge Canary with extensions | Extension needs a local Claude Code native host; none on Android |
| Termux + Claude Code | No Android build; even with a Linux userland there is no Chrome native-messaging path |
| claude.ai web + CoCounsel connector | US-only, firm subscription, not Westlaw UK |
