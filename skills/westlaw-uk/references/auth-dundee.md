# Authentication: University of Dundee

Dundee is a member of the **UK Access Management Federation** and uses
Shibboleth, **not** OpenAthens. Dundee's library guide explicitly says not to
pick the OpenAthens option. The route Westlaw calls "Access through academic
institution" is the one to use.

## WAYFless URL

The Dundee library's Westlaw link (`https://libguides.dundee.ac.uk/Westlaw`)
302-redirects to this URL, which pre-selects the Dundee IdP and returns to the
Westlaw UK home page:

```
https://signon.thomsonreuters.com/federation/UKF?entityID=https%3A%2F%2Fidp.dundee.ac.uk%2Fshibboleth&returnto=https%3A%2F%2Fwestlawuk.thomsonreuters.co.uk%2FBrowse%2FHome%2FWestlawUK%3FskipAnonymous%3Dtrue
```

Components:

| Part | Value |
|------|-------|
| Federation endpoint | `https://signon.thomsonreuters.com/federation/UKF` |
| `entityID` | `https://idp.dundee.ac.uk/shibboleth` |
| `returnto` | `https://westlawuk.thomsonreuters.co.uk/Browse/Home/WestlawUK?skipAnonymous=true` |

If the libguides redirect ever changes, fetch `https://libguides.dundee.ac.uk/Westlaw`
with redirects disabled and read the `Location` header to get the new one.

## Flow as seen in the browser

1. WAYFless URL → `signon.thomsonreuters.com` issues a SAML AuthnRequest.
2. Redirect to Dundee's IdP, which **is Microsoft Entra**: the browser lands
   directly on `login.microsoftonline.com/<tenant>/saml2` titled "Sign in to
   your account" (confirmed 2026-10-25; no Shibboleth-branded page is shown).
   Authenticator MFA follows. This is where **the user types**. Claude does not.
3. SAML response back to `signon.thomsonreuters.com`.
4. On a user's **first ever** visit Westlaw may ask to create or link a
   **OnePass** profile. Hand this to the user too.
5. Back on `signon.thomsonreuters.com/?comp=wluk&productid=PLCUK&viewproductid=UKWL&opentoken=...`
   for a second or two, then redirect to
   **`https://uk.westlaw.com/Browse/Home/WestlawUK?...&firstPage=true`**, page
   title "Home | Westlaw Edge UK". Session cookies live on `uk.westlaw.com`. Signed in.
   The Dundee entitlement showed Cases, Legislation, Journals, Current Awareness,
   EU, Westlaw Books, Practical Law, Case Analytics, Legislation Compare.

## Detecting state cheaply

After navigation, `get_page_text` the tab and look for:

- Signed in: URL host is `uk.westlaw.com`, title ends "| Westlaw Edge UK", and
  a **Sign out** button is in the header.
- Not signed in: a password field exists, or the URL host is
  `login.microsoftonline.com`, `idp.dundee.ac.uk`, or `signon.thomsonreuters.com`.
- An already-authenticated Entra session makes the whole hop silent: on the
  second visit of 2026-10-25 the WAYFless URL went straight through to Westlaw
  in about 8 seconds with no prompt. Wait, then re-check, before declaring
  "needs login".

## Session lifetime

Westlaw UK sessions time out after a period of inactivity (commonly around 30
minutes; confirm in practice and update here). Westlaw also enforces a
single-concurrent-session rule per user; signing in on a second device may
kick the first. If a mid-task page comes back as the SSO form, treat it as
"needs login" again, do not retry blindly.

## Open University (not yet wired)

OU goes via OpenAthens "Find your institution" → OUCU + password, plus a
OnePass account on first visit. The same `signon.thomsonreuters.com/federation`
endpoint applies but with the OU `entityID`. Tracked as a repo issue.
