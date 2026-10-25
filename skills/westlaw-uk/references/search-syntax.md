# Westlaw UK search syntax (terms and connectors)

Westlaw Edge UK accepts natural language in the Home search box (slide
toggle), but **terms and connectors** gives repeatable results. The table below
is copied from the site's own "Show Terms and Connectors" panel on the Cases
search form, 2026-10-25. **A bare space means AND.**

## Connectors

| Syntax | Meaning | Example |
|--------|---------|---------|
| `&` or `AND` | both terms | `enrichment & unjustified` |
| `OR` | either term | `pursuer OR claimant` |
| `/s` | same sentence | `duty /s care` |
| `/p` | same paragraph | `negligence /p economic loss` |
| `/n` | within *n* words | `delict /5 damages` |
| `+n` | first term precedes second within *n* words | `unjust! +3 enrich!` |
| `+s`, `+p` | preceding term within same sentence / paragraph | `duty +s care` |
| `#` | turn off plurals and equivalents | `#damage` |
| `TI( )` | words in the title | `TI(Donoghue)` |
| `%` or `BUT NOT` | exclude | `tenancy % agricultural` (use sparingly) |
| `" "` | exact phrase | `"res ipsa loquitur"` |
| `( )` | grouping | `(pursuer OR claimant) /p prescription` |

Rough precedence: `OR` binds tightest, then proximity, then `&`, then `%`. Use brackets anyway.

## Wildcards

| Syntax | Meaning | Example |
|--------|---------|---------|
| `!` | root expander | `contract!` → contract, contracts, contractual |
| `*` | single character | `wom*n` → woman, women |

## Field restrictions (Cases)

Use the **Cases advanced search** form rather than field codes where possible;
the form exposes: party names, citation, court, date range, subject/keyword,
judge, legislation cited, cases cited. Field codes exist (`TI()`, `CI()`,
`JU()`), but the form is more reliable and the labels are self-describing.

## Jurisdiction

Westlaw UK is UK-wide by default. For Scots law narrow by court (Court of
Session, High Court of Justiciary, Sheriff Appeal Court, sheriff courts) or use
the jurisdiction facet on the results page. Session Cases and SLT/SCLR report
series are good secondary filters.

## Cost-saving habits

- Write the query, check it once with the user if the subject is ambiguous, then run it. Each run is metered.
- Facets on a results page (Topic, Jurisdiction, Tribunal, Document Type, Case Type, Status, Authority, Date) reload the page. Whether that is metered is unconfirmed; prefer getting the query right first.
- Test query `unjust! /3 enrich! & Scotland` returned 1,735 cases, many English, because "Scotland" matches anywhere in the text. For Scots cases filter by **Court** (`CO` field: "Court of Session", "Sheriff") or use the Jurisdiction facet rather than a bare word.
- Sort by relevance first; switch to date only when asked for latest authority.
- Open documents by their URL from the results list rather than re-searching the citation.
