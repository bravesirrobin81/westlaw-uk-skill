# Setup: macOS host

Same shape as Windows, fewer caveats.

```sh
curl -fsSL https://claude.ai/install.sh | bash
claude                 # /login with your Anthropic account, not an API key
claude plugin marketplace add git@gitea.freshlybrewed.coffee:leigh/westlaw-uk-skill.git
claude plugin install westlaw-uk@freshlybrewed
claude --chrome
```

Chrome stable with *Claude in Chrome* (1.0.36+) signed in to the same account.
Allow the same sites as in `setup-windows.md` §2.5. Then follow "First run" in
that document.

## FreeBSD note

There is no Claude Code or Chrome-extension path on FreeBSD. The jail hosts
are not involved in this skill at all; the browser and Claude Code must live
on a Mac or Windows box. If a headless, host-agnostic path is ever needed,
the Playwright fallback (see repo issue) would run on FreeBSD with
`www/chromium`, but it would also need its own persistent profile and its own
manual SSO step, and it is a less defensible shape under the licence.
