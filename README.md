# Bible Study (working name)

A standalone, installable PWA prototype for New Testament study.
Tagline: "Do you know what your Bible is really saying?"

Plain static files. No build step, no backend, no accounts. Serve the folder
over http or https and it runs. Book data loads one book at a time, and the
service worker caches the shell and every book pack you open, so after the
first visit the app works offline.

## What it does

- **Reference lookup.** Type "Romans 8:28", "1 Tim 2:12", "John 3", "jn 3:16",
  or "Jude 9" in the box at the top and land on that verse. The parser takes
  full names, common abbreviations, squashed forms like "1tim2:12", chapter
  only lookups, and ranges (it opens the first verse). Old Testament
  references get a plain "not in this prototype yet" message.
- **Read.** All 27 New Testament books, chapter by chapter, in the World
  English Bible, King James Version, or Berean Standard Bible.
- **Study workspace.** Tap a verse: three translations side by side, the
  Greek words as tappable chips. Tap a word: Greek form, transliteration,
  Strong's number, morphology in plain words, the STEPBible lexicon entry,
  and every other place that word appears in the New Testament. A word used
  exactly once (like authentein in 1 Timothy 2:12) is flagged as such.
- **The chain.** Treasury of Scripture Knowledge cross references on every
  verse, sorted by the traditional vote counts. Tapping one follows it, and
  the "Your path" trail at the top keeps the whole rabbit hole so you can
  walk back. Old Testament targets are listed and marked; their text is not
  bundled in this prototype.
- **Notes and My Maps.** Notes save on the device (localStorage) against any
  verse or any word, with the Verse Mapping scaffold (Verse / Design /
  Development / Actions / Outcome) in the verse note editor. All notes and
  your current path collect under My Maps.
- **Search.** Full text search across the New Testament (WEB text), a
  secondary tab in the header.
- **Context.** An Ephesus panel (the city, the Temple of Artemis, Koine
  Greek) built from Wikipedia, linked from Ephesians, 1 and 2 Timothy, and
  Acts. Every book page also carries a short Wikipedia introduction.

## Data sources and licenses

All content is retrieved from stored open sources. No verse text, lexicon
entry, or reading content is generated.

| Layer | Source | License |
| --- | --- | --- |
| Greek text, word tagging, morphology | TAGNT, Translators Amalgamated Greek NT, STEPBible (Tyndale House, Cambridge) | CC BY 4.0 |
| Lexicon | TBESG, Translators Brief lexicon of Extended Strongs for Greek (Abbott-Smith based), STEPBible | CC BY 4.0 |
| Morphology expansions | TEGMC, STEPBible | CC BY 4.0 |
| Cross references | Treasury of Scripture Knowledge (c. 1830), machine readable edition from OpenBible.info | Public domain source, CC BY edition |
| Scripture text | World English Bible (bible-api.com and bolls.life), King James Version | Public domain |
| Scripture text | Berean Standard Bible (via bolls.life) | Freely available for use, (c) Berean Bible |
| Context and book intros | Wikipedia summaries and article introductions | CC BY-SA |

STEPBible data: https://STEPBible.github.io/STEPBible-Data

## Known edition differences

- The WEB places the Romans doxology at 14:24-26 in the source used, so
  Romans shows 436 verse records: the standard 433 plus those three.
  Romans 16:26-27 carry KJV and BSB text (the WEB source omits them there).
- Seven verses have no WEB text in the bundled source (mostly verses the
  WEB brackets or omits); they still show KJV and BSB, and they are the
  reason the search index covers 7,953 of the 7,960 verse records.
- Greek words follow the NA28 text as tagged in TAGNT (137,590 words).
  Thirty of the 1,153 morphology codes used have no plain language
  expansion in TEGMC and show the raw code instead.
- The lexicon covers the full TBESG file (10,376 headwords, including
  Septuagint vocabulary that never occurs in the New Testament).

## Deploy

Any static host works. The only requirement is http(s): browsers will not
load the data files from a file:// page.

**GitHub Pages**

1. Create a repository and copy this whole folder into it (keep the folder
   structure: index.html at the root, data/ and icons/ beside it).
2. Commit and push.
3. In the repository: Settings, Pages, "Deploy from a branch", pick the
   main branch and the root folder, Save.
4. The site appears at https://YOUR-NAME.github.io/REPO-NAME/ within a
   few minutes. Open it on a phone and use "Add to Home Screen" to
   install it as an app.

**Local test**

```
cd bible-pwa
python3 -m http.server 8000
```

Then open http://localhost:8000 in a browser.

**Other hosts**

Netlify, Cloudflare Pages, Vercel, or any web server: publish the folder
as the site root, no build command, no environment variables.

## Files

- `index.html, styles.css, app.js` the app
- `sw.js` service worker (offline shell and data pack caching)
- `manifest.webmanifest, icons/` install metadata and icons
- `data/meta.json` book list, introductions, Ephesus context
- `data/<book>.json` one pack per book: text (WEB/KJV/BSB), Greek words,
  cross references for that book's verses
- `data/lexicon.json` TBESG lexicon, occurrence lists for every Strong's
  number across the NT, morphology expansions
- `data/search.json` WEB text index for full text search
