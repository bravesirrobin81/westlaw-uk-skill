# Westlaw UK search syntax (terms and connectors)

Westlaw UK accepts natural language in the main box, but **terms and
connectors** gives repeatable results. Verify the odd corner (default handling
of a bare space) against the live help page the first time; sources disagree.

## Connectors

| Syntax | Meaning | Example |
|--------|---------|---------|
| `&` or `AND` | both terms | `enrichment & unjustified` |
| `OR` | either term | `pursuer OR claimant` |
| `/s` | same sentence | `duty /s care` |
| `/p` | same paragraph | `negligence /p economic loss` |
| `/n` | within *n* words | `delict /5 damages` |
| `+n` | first term precedes second within *n* words | `unjust! +3 enrich!` |
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
- Use the **facets** on a results page (court, date, subject, status) instead of a new search. Faceting is a re-filter, not a fresh search, as far as we can tell; confirm and record here.
- Sort by relevance first; switch to date only when asked for latest authority.
- Open documents by their URL from the results list rather than re-searching the citation.
