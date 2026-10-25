# westlaw-uk

A Claude Code plugin that lets Claude do legal research on **Westlaw UK**
(`westlawuk.thomsonreuters.co.uk`, formerly `uk.westlaw.com`) using your own
browser session, signed in through the **University of Dundee** institutional
login (UK Access Management Federation / Shibboleth).

There is no API behind an academic Westlaw UK subscription. Thomson Reuters'
official MCP connector (`cocounsel-legal`) is US-law only and needs a firm
CoCounsel Legal subscription. So this skill drives the real website through the
**Claude in Chrome** extension. Claude never sees or types your password; you
complete the Dundee SSO and MFA yourself, Claude works inside the resulting
session.

## What is in here

| Path | Purpose |
|------|---------|
| `skills/westlaw-uk/SKILL.md` | The skill Claude loads: workflow, guardrails, output format |
| `skills/westlaw-uk/references/auth-dundee.md` | WAYFless login URL and how to detect auth state |
| `skills/westlaw-uk/references/search-syntax.md` | Westlaw UK terms-and-connectors cheat sheet |
| `skills/westlaw-uk/references/site-map.md` | Known entry points and page shapes (fill in as discovered) |
| `skills/westlaw-uk/references/oscola.md` | OSCOLA citation output rules |
| `docs/setup-desktop-app.md` | Testing from the Claude desktop app (built-in browser, no Chrome needed) |
| `docs/setup-windows.md` | Chrome + Claude Code CLI on Windows |
| `docs/setup-android.md` | Driving it from an Android phone via Remote Control |
| `docs/setup-macos.md` | macOS host notes |
| `docs/licence.md` | Licence and metering constraints, read this once |

## Install

Add this repo as a marketplace (SSH, the repo is private), then install the plugin:

```
claude plugin marketplace add git@gitea.freshlybrewed.coffee:leigh/westlaw-uk-skill.git
claude plugin install westlaw-uk@freshlybrewed
```

Or, for hacking on it, clone and run Claude Code with the directory loaded:

```
git clone https://gitea.freshlybrewed.coffee/leigh/westlaw-uk-skill.git
claude --chrome --plugin-dir ./westlaw-uk-skill
```

Then follow the setup doc for your platform, open Chrome, and ask Claude
something like:

> Using Westlaw UK, find the leading Scottish authorities on unjustified
> enrichment since 2015 and give me OSCOLA citations with case status.

## Requirements

- Claude Code signed in with `/login` on a Pro, Max, Team or Enterprise plan.
  API-key auth disables the Chrome integration.
- Google Chrome with the *Claude in Chrome* extension (1.0.36 or newer) signed
  in to the same account.
- A University of Dundee account with Westlaw UK entitlement.
- Windows or macOS. Either Chrome with the extension, or the Claude desktop app's built-in browser. WSL is not supported for the CLI Chrome path.
  Android works only as a remote controller of a desktop session.

## Status

Scaffold. The auth flow and guardrails are written. Page selectors and exact
search URL patterns in `site-map.md` are to be filled in after the first live
sessions. See the repo issues for the plan.
