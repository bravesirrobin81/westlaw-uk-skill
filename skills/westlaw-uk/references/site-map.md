# Westlaw Edge UK site map

Confirmed live on 2026-10-25 from a Dundee session. Product branding is
**Westlaw Edge UK**; the host after login is **`https://uk.westlaw.com`**.
(`westlawuk.thomsonreuters.co.uk` in the WAYFless `returnto` is accepted and
redirects to `uk.westlaw.com`.) Keep URLs here, never in SKILL.md.

## Entry points (all relative to `https://uk.westlaw.com`)

| Area | Path | Notes |
|------|------|-------|
| Home | `/Browse/Home/WestlawUK` | Global search box, topic links, feature tiles |
| Cases search form | `/Browse/Home/WestlawUK/Cases` | Tabs: Cases, Pending Actions, Permissions to Appeal, Companies. "Show Terms and Connectors" help panel |
| Courts browse | `/Browse/Home/WestlawUK/Cases/Courts` | |
| Dockets | `/Browse/Home/WestlawUK/Cases/Dockets` | |
| Law Reports browse | `/Browse/Home/WestlawUK/Cases/LawReports` | |
| Quantum Reports | `/Browse/Home/WestlawUK/Cases/QuantumReports` | PI quantum |
| Legislation search form | `/Browse/Home/WestlawUK/WLLegislation` | Tabs: Primary & Secondary, Bills & Drafts, Policy & Guidance |
| Primary & Secondary browse | `/Browse/Home/WestlawUK/WLLegislation/WLPrimarySecondaryLegislation` | |
| Retained EU legislation | `/WestlawUK/Legislation/Primary-and-Secondary-Legislation/Brexit-Retained-EU-Legislation` | |
| Bills & Drafts | `/Browse/Home/WestlawUK/WLLegislation/WLBillsDraftLegislation` | |
| Policy & Guidance | `/Browse/Home/WestlawUK/WLLegislation/WLPolicyGuidance` | |
| Latest Legislation | `/Browse/Home/WestlawUK/WLLegislation/WLLatestLegislation` | |
| Key Legal Concepts | `/Glossary/UKKeyLegalConcepts` | |
| Journals search form | `/Browse/Home/WestlawUK/Journals` | |
| Journal publications | `/Browse/Home/WestlawUK/Journals/JournalPublications` | |
| Current Awareness search | `/Browse/Home/WestlawUK/CurrentAwareness` | |
| CA most recent | `/WestlawUk/CurrentAwareness/Most-recently-added` | |
| CA best of last 5 days | `/WestlawUk/CurrentAwareness/Best-of-the-last-five-days` | |
| Index of Legal Terms | `/Browse/Home/WestlawUK/IndexofLegalTerms` | |
| EU | `/Browse/Home/WestlawUK/EuropeanUnion` | |
| Scots Law hub | `/Browse/Home/WestlawUK/WLScotsLaw` | ASPs, SSIs, Scottish law reports and cases, Scottish journals, Gloag & Henderson 15th, Parliament House Book |
| Topic hubs | `/Browse/Home/WestlawUK/Topic/WL<Topic>` | e.g. `WLContract`, `WLTorts`, `WLLand` |
| Westlaw Books | `/Browse/Home/Books` | In the Dundee entitlement |
| Practical Law | `/Browse/Home/PracticalLaw` | In the Dundee entitlement |
| Cite & Print | `/CiteAndPrint/Home` | |
| Folders | `/ResearchOrganizer/Home.html` | Account state; do not touch |
| Favourites | `/FavoritesOrganizer/Home.html` | Account state; do not touch |
| Westlaw International / Ireland / US | `/Transfer.html?domainKey=WLI|WestlawGlobal|WestlawNext` | Different products, may be out of plan |

## Search results URL

A Cases free-text search from the form produces:

```
/Search/Results.html?comp=wluk&query=advanced%3A%20((<query>))&contentType=UK-CASES
   &startIndex=1&categoryPageUrl=Home%2FWestlawUK%2FCases&transitionType=Search
   &contextData=(sc.Default)&querySubmissionGuid=<guid>&searchId=<guid>&...flags
```

- `contentType` values seen: `UK-CASES`. Expect `UK-LEGISLATION`, `UK-JOURNALS` etc.; confirm when seen.
- `query=advanced: ((...))` is the terms-and-connectors form. Re-running by URL is a **new metered search**; navigate to a results URL only if the user asks to re-open one.
- `startIndex` pages 20 at a time. Pagination links also exist in-page.
- `ancillaryChargesAccepted=False` and `useNonBillableZoneClientId=False` appear in every search URL. Do not change them.

## Reading a results page

Use `get_page_text`. Each hit renders as:

```
N. <Case name>
<Court> <neutral citation> <date>
<Status text>                       e.g. "No Substantial Judicial Treatment"
Subject: ...; Keywords: ...
Case Analysis | <citations: neutral | WLUK | report series> | Judgment
Summary: <editorial summary or "There is no summary available.">
```

- **Status is plain text** on the results page and `img` alt text on the document page. Read it; never infer from colour.
- Some hits carry an **"Out of Plan"** label. These are outside the Dundee subscription and may incur charges. **Never open an Out of Plan document.** Report the citation and tell the user.
- Heading `Cases (N)` gives the count. 20 per page; sort control toggles Relevance / Date.
- Left rail facets: Topic (with per-topic counts), Jurisdiction, Tribunal, Document Type, Case Type, Status, Authority, Date, plus "Search within results". Applying a facet reloads the results page; treat it as **possibly metered** until confirmed otherwise.
- "Add to Keep List", "Save To Folder", "Email", "Select all" are account/state actions: never.

## Case document

URL: `/Document/<32-hex GUID>/View/FullText.html?...` Strip the query string when recording a URL for the user; the GUID path alone resolves.

Layout:

- Top toolbar: Email, Print, Download, Add To Folder. Only Download, and only on explicit request.
- Nav links: **Case Analysis**, **Judgment**, **Primary References** (cases/legislation cited), **Case Analytics** (precedent map, treatment).
- Case Analysis page sections: status image (alt text = status), **Where Reported**, **Case Digest** (summary, subject, keywords), appellate history, cases citing, legislation cited, journal articles.
- Example: Well-fed Scotland CIC v Serco Ltd [2026] CSOH 100, `/Document/I97BD4C70C64311F19EA0FC8A495C9D62/View/FullText.html`.

## Advanced search field ids (for `form_input`)

Prefix every id with `co_search_advancedSearch_`. Submit with button `co_search_advancedSearchButton_top`.

**Cases tab:** `TI` Parties · `CI` Citation (quoted, no connectors) · `queryAny` Free Text · `SUJKW` Subject/Keyword · `TD` Document Type (All / Case Analysis / Law Reports and Judgments) · under *More options*: `PUB` Law Reports · `LR` Full Text Law Reports · `defined-term` Term Defined · `LC` Legislation Title · `NO`+`LPN` Legislation Provision · `CO` Court · `JU` Judge · `CC` Cases Cited (party) · `CASES-CITED` Cases Cited (citation) · `LRT-CASES`/`LR-CASES` Legal Representatives.
Pending Actions tab uses `-PA` suffixes; Permissions to Appeal `-PTA`; Companies tab `companyNameNgram`, `companyNumberNgram`, `COMPANY-COURT`, `COMPANY-FT`.

**Legislation tab:** `TI` Title · `NO`+`LPN` Provision Number · `queryAny` Free Text · `SUJKW` Subject/Keyword · `defined-term` Statutory Definition · `JURIS` Jurisdiction · `SINCE` Since · `PIT` Point-in-time date. Bills & Drafts `-BD` suffixes with `DT-BD_*` radios; Policy & Guidance `-PG`.

**Journals tab:** `queryAny` Free Text · `SUJKW` Subject/Keyword · `TI` Article Title · `AU` Author · `JournalTitle` Full Text Journals · `TD` Document Type · `defined-term` · `CC` / `CASES-CITED` Cases Cited · `LC` + `NO`/`LPN` Legislation · `COM` Organisation Cited.

The global search box on Home has id `searchInputId` with a **Natural language** slide toggle; the form pages do not show it in the header, use the form's Free Text instead.

## Tooling notes

- The Claude in Chrome extension **blocks `javascript_tool` output** that contains query strings or GUID-like tokens ("BLOCKED: Cookie/query string data"). Scrape with `get_page_text` / `read_page` / `find`; if using JS, return pathnames only and strip query strings.
- `find` works well for form fields and result links on these pages.

## Dialogs to avoid

- Download / Print / Email open modals; only Download, only on request.
- Add to folder, Keep List, Alerts, Favourites, "Make this my start page" change account state; never.
- "Signed in elsewhere" / session expired page ends the session; stop and tell the user.

## Changelog

- 2026-10-25: confirmed live. Host, nav, results URL, result shape, document layout, field ids, Out of Plan marker, JS-output blocking.

## Case Analysis panels (confirmed 2026-10-25)

Left-hand contents on a case document: Case Analysis > Case Digest, Key Cases Cited,
All Cases Citing, Journal Articles; Case Analytics > Precedent map, Treatment.
Clicking a panel link scrolls within the same document; no new page, no search charge.

- `get_page_text` returns only the digest block whatever panel is open. Read the other
  panels with `find` (e.g. "treatment labels preceding each citing case") and `read_page`
  on the returned refs.
- All Cases Citing: each entry is preceded by a treatment label ("Mentioned by",
  "Considered by", "Applied by", ...) then a heading link with case name, court,
  citation, date, and that citing case's *own* status line. Do not confuse the two.
- No "Appellate History" heading appears when the case was not appealed.
- Citation field `co_search_advancedSearch_CI` accepts report citations ("2016 S.C. 367")
  as well as neutral ones and returned exactly 1 hit. Use it for lookups.
- Data quirk seen: a 2003 case listed as citing a 2013 one. Sanity-check dates.
