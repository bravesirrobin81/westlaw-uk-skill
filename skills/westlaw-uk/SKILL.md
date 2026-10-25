---
name: westlaw-uk
description: Legal research on Westlaw UK (westlawuk.thomsonreuters.co.uk) through the user's own Chrome session, signed in via University of Dundee Shibboleth SSO. Use whenever the user mentions Westlaw, Westlaw UK, uk.westlaw.com, wants UK/Scots case law with status/treatment, journal articles, or Westlaw-style legislation annotations. Not for US law (that is the cocounsel-legal plugin) and not the first choice for plain statute text (use legislation.gov.uk).
---

# Westlaw UK research skill

You are driving a metered, licensed legal database through the user's real browser.
Three rules sit above everything else:

1. **Never type, store or ask for the user's credentials.** The SSO page is theirs. You stop, tell them to log in, and wait.
2. **Every search is counted against the University of Dundee's subscription.** Plan queries before running them. Default budget is 6 searches per task; tell the user when you are about to exceed it and let them raise it.
3. **No bulk anything.** No paging past 3 result pages, no looping over document lists, no saving files unless the user asks for one specific document. See `docs/licence.md` at the plugin root.

## 0. Preflight: pick a browser backend

Two backends work. Use whichever tools are present; never fall back to curl or WebFetch against Westlaw, that bypasses the user's session and the licence.

| Backend | Tool prefix | Where |
|---------|-------------|-------|
| Claude in Chrome (preferred, user's real Chrome profile) | `mcp__claude-in-chrome__` | Claude Code CLI with `--chrome`, or the desktop app connected to Chrome |
| Built-in browser pane | `mcp__Claude_Browser__` (or `mcp__remote-devices__Claude_Browser__` from a cloud/phone session) | Claude desktop app (Cowork), no Chrome needed |

Load tools in **one** ToolSearch call:

- Chrome: `select:mcp__claude-in-chrome__tabs_context_mcp,mcp__claude-in-chrome__navigate,mcp__claude-in-chrome__computer,mcp__claude-in-chrome__read_page,mcp__claude-in-chrome__get_page_text,mcp__claude-in-chrome__find,mcp__claude-in-chrome__tabs_create_mcp,mcp__claude-in-chrome__tabs_close_mcp`
- Built-in: query `mcp__Claude_Browser__` with `max_results: 64` (or the `remote-devices` prefix).

If only an `enable__...` tool exists for either, call it first. If neither backend is available, say so and point the user at `docs/setup-desktop-app.md`, `docs/setup-windows.md` or `docs/setup-macos.md`.

Tool name mapping (names after the prefix are otherwise the same):

| Task | Chrome | Built-in |
|------|--------|----------|
| List tabs | `tabs_context_mcp` | `tabs_context` |
| Open a new tab at a URL | `tabs_create_mcp` then `navigate` | `preview_start` with `url` (one call, returns `tabId`) |
| Close tab | `tabs_close_mcp` | `tabs_close` |
| Read text / structure | `get_page_text`, `read_page`, `find` | same |

Call the tab-listing tool first, then open a **new tab** and work only in tabs you created.

**Built-in browser only:** the pane may be hidden behind an artifact or closed. Before asking the user to do anything in it (sign in, approve MFA), call `tabs_context` and check the pane status it reports. If hidden, ask them to press Cmd+Shift+B (Mac) or Ctrl+Shift+B (Windows), or click the globe icon, then tell them what to do. The built-in browser keeps its own persistent profile, so a Dundee/Westlaw sign-in survives across sessions; treat it as the user's and never sign out.

## 1. Authenticate (Dundee WAYFless)

Navigate to the WAYFless URL in `references/auth-dundee.md`. Then read the page and classify where you landed:

| You see | Meaning | Action |
|---------|---------|--------|
| URL starts `westlawuk.thomsonreuters.co.uk/Browse/Home/WestlawUK` and page shows search box, Cases / Legislation / Journals tabs | Signed in | Continue |
| URL on `idp.dundee.ac.uk` or `login.microsoftonline.com`, a username/password form | Needs login | **Stop.** Tell the user: "Please sign in to Dundee in the Chrome tab I just opened, complete MFA, then tell me when Westlaw has loaded." Wait for their reply, then re-read the page. |
| `signon.thomsonreuters.com` page with "Access through academic institution" | WAYF did not pre-select | Click **Access through academic institution**, pick *University of Dundee*, then treat as "needs login". |
| OnePass registration / "create account" | First-time linking | Stop and hand to user. Do not fill it in. |
| "Session expired" or "signed in elsewhere" | Concurrent-session limit | Stop, explain, let the user choose which session to keep. |

Never submit the SSO form yourself, even if the user offers the password in chat. Tell them to type it into the browser.

## 2. Plan before searching

Write a short plan to the user before the first search:

- the legal question in one sentence
- which Westlaw UK content set(s): Cases, Legislation, Journals, Current Awareness, Books, EU
- jurisdiction filter (Scotland / England & Wales / UK-wide)
- the 1 to 3 queries you intend to run, in terms-and-connectors form (see `references/search-syntax.md`)
- whether free sources would do instead (full text of an Act or a 2003+ judgment: suggest legislation.gov.uk or Find Case Law and skip Westlaw)

Then run it. Natural-language search works on Westlaw UK but terms-and-connectors is more predictable and cheaper in re-runs.

## 3. Run searches and read results

- Use the main search box on the home page, or the content-specific advanced search listed in `references/site-map.md`.
- Use `find` and `read_page` (interactive filter) to locate controls; use `get_page_text` to read a results list or a document. Avoid screenshots unless layout matters.
- From a results list capture per hit: case name, neutral citation, "Where Reported", court, date, **case status indicator** (see below), and the document URL.
- Open only the documents you will actually cite. Reading a document is cheaper than another search; do not re-search to re-find something you already have the URL for.
- Do not click **Download**, **Print**, **Email** or **Add to folder** unless the user asked for that specific document.

### Case status (Westlaw UK Case Analysis)

Report the status flag as Westlaw shows it and say what it means:

| Flag | Meaning |
|------|---------|
| Green tick | Positive or neutral judicial treatment |
| Yellow triangle | Mixed or mildly negative treatment, or appeal pending |
| Red stop | Negative treatment: overruled, reversed, not followed, or repealed/superseded (legislation) |
| No flag | No treatment recorded |

Always open **Case Analysis** or the "Appellate History / Cases citing" panel before telling the user a case is good law. Never infer status from the flag colour alone when the question turns on it.

### Legislation

Westlaw UK shows point-in-time versions. Record the **version date** shown and note "as at <date>". For the authoritative current text, legislation.gov.uk is free and unmetered; prefer it for quoting statutory wording and use Westlaw for annotations and amendment history.

## 4. Output

- Citations in **OSCOLA** per `references/oscola.md`, neutral citation first, then best report series from "Where Reported".
- One line of status per case.
- Separate what you read from what you infer. Quote headnote language sparingly; paraphrase and cite.
- End with: number of Westlaw searches run, and the Westlaw URLs of every document relied on so the user can open them directly.

## 5. Teardown

Close the tabs you created (`tabs_close_mcp` / `tabs_close`). **Do not sign out**; the user's session is theirs.

## When to stop and ask

- Any login or OnePass page: always.
- Any dialog, CAPTCHA, or "unusual activity" notice.
- Tool errors on 2 or 3 consecutive attempts.
- You are about to exceed the search budget.
- A result set is over ~200 hits and narrowing needs a legal judgment call (jurisdiction, date, subject).

## Out of scope

- US / Westlaw Next / Westlaw Edge content (the `cocounsel-legal` plugin covers that for subscribers).
- Practical Law UK unless the Dundee entitlement shows it in the menu.
- Drafting legal advice for a specific fact pattern. Research the law; let the user apply it.
