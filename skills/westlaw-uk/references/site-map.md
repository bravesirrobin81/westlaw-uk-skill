# Westlaw UK site map

Known entry points on `westlawuk.thomsonreuters.co.uk`. Items marked
**TODO** need confirming from a live session and should be updated as soon as
they are seen. Keep URLs here, never in SKILL.md, so the skill stays short.

## Entry points

| Area | URL | Notes |
|------|-----|-------|
| Home | `/Browse/Home/WestlawUK?skipAnonymous=true` | Main search box, content tabs |
| Cases | `/Browse/Home/WestlawUK/Cases` | **TODO** confirm path |
| Legislation | `/Browse/Home/WestlawUK/Legislation` | **TODO** |
| Journals | `/Browse/Home/WestlawUK/Journals` | **TODO** |
| Current Awareness | `/Browse/Home/WestlawUK/CurrentAwareness` | **TODO** |
| Books | `/Browse/Home/WestlawUK/Books` | Entitlement-dependent |
| EU | `/Browse/Home/WestlawUK/EU` | **TODO** |
| Search results | `/Search/Results.html?query=...&jurisdiction=uk&contentType=...` | **TODO** record real parameters; if stable, navigate directly to save a page load (not a search) |
| Document | `/Document/<GUID>/View/FullText.html` | **TODO** confirm |

Legacy `uk.westlaw.com` and `login.westlaw.co.uk` redirect to the above.

## Page shapes to recognise

### Results list
Each hit: title (link), neutral citation, court, date, "Where Reported" list,
status icon (img alt text carries the meaning; read it with `read_page`
rather than guessing colour), snippet. Left rail: facets. Top: result count,
sort control, pagination.

### Case document
Tabs or anchors: **Case Analysis** (status, appellate history, cases citing,
legislation cited, journal articles), **Where Reported**, **Judgment** full
text, **Headnote** / **Case Digest**. Report which tab the information came from.

### Legislation document
Breadcrumb (Act → Part → Section), version selector with dates, **Status**
flag, "Amendments" / "Commencement" / "Modifications" panels, annotations.

## Dialogs to avoid

- **Download / Print / Email** open modals; only on explicit user request.
- **Add to folder** and **Alerts** change the user's account state; never.
- A "You have been signed in elsewhere" page ends the session; stop.

## Changelog

- 2026-10-25: scaffold, nothing confirmed live yet.
