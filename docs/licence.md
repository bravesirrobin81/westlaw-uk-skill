# Licence and metering: read once

Westlaw UK at Dundee is an **academic site licence**. Nothing in this repo
changes what the licence permits; the skill just makes you faster at doing
what you could already do in a browser. Keep it that way.

## What we know

- Access is for members of the University for educational and non-commercial
  research use. Teesside's and other UK library guides spell out that
  breaching database licence terms can cost the **whole institution** access.
- Thomson Reuters **meters searches**. The Inner Temple Library notes that
  each search is counted and the statistics feed into subscription pricing.
  Dundee's subscription cost is affected by how we use it.
- Westlaw's general terms, like most legal databases, restrict automated
  access, bulk download and systematic copying. The exact Dundee licence text
  has **not** been read by us yet; see the repo issue to ask the law librarian.
- The UK text-and-data-mining exception (CDPA s 29A) covers non-commercial
  research and cannot be overridden by contract, but it is about *copying for
  computational analysis*, not about hammering a metered search endpoint.
  Do not lean on it as a justification for volume.

## How the skill behaves because of this

- Interactive pace only: one human question → a handful of searches → a few documents.
- Default budget 6 searches per task; Claude asks before going over.
- No paging beyond 3 results pages, no iteration over result lists, no
  background jobs, no scheduled runs, no caching of Westlaw content outside
  the session.
- Downloads only when you ask for a specific document.
- Free sources first for primary-law text: legislation.gov.uk and Find Case Law
  (caselaw.nationalarchives.gov.uk). Westlaw for what only Westlaw has:
  treatment/status, headnotes, journals, annotations, older reports.

## Things that would be a bad idea

Running this under a cron, sharing your Dundee session with others, pointing
it at Westlaw for anything commercial, or using it to build a local corpus.
