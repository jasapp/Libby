# Library website lookup instructions

You are finding the official public website for each US public library system in a batch file. Only the WebSearch tool reaches the internet in this environment (WebFetch and curl are blocked, do not try them).

Input: batches/batch_NNN.json (path given in your task) — a JSON array of objects with fields id, name, city, county, state.

Output: append one JSON object per line to results/batch_NNN.jsonl (same NNN, path given in your task) with the fields:
  {"id": "<id>", "website": "<url or empty string>", "confidence": "high|medium|low", "note": "<short reason, optional>"}

Procedure for EACH library (do every single one, in order, skip none):
1. Run WebSearch with a query like: "<name> <city> <state> library". Pass blocked_domains: ["facebook.com","instagram.com","twitter.com","x.com","yelp.com","wikipedia.org","citylibrary.com","publiclibraries.com","libraries.org","librarytechnology.org","imls.gov","overdrive.com","libbyapp.com","mapquest.com","yellowpages.com","linkedin.com","youtube.com","tiktok.com","tripadvisor.com","niche.com","foursquare.com","chamberofcommerce.com","bbb.org","indeed.com","glassdoor.com","zoominfo.com","dnb.com","manta.com","alignable.com","nextdoor.com","alllocallibraries.com","librarian.net","worldcat.org","goodreads.com"].
2. From the result URLs pick the library's own official website. Signs of an official site: the library name or city in the domain (e.g. anchoragelibrary.org, mylibrary.org), a library-specific domain, or a page under the city/county/town government domain (e.g. cityofx.gov/library, xcounty.gov/library). A library section of a city/county government site IS the correct answer for municipal libraries; use the library section URL (e.g. https://www.cityofx.gov/library), not the city home page, when the search shows it.
3. Normalize the URL: use https, strip tracking params, and trim to the library's home page (root domain for standalone library domains; the library section path for government sites). Do not include deep pages like /hours or /catalog unless nothing better exists.
4. If the first search is ambiguous, run at most one more search with a variation (e.g. add "official site", or use the county name for county libraries, or drop punctuation). If still not found, write website "" with confidence "low".
5. Do NOT guess or fabricate URLs. Only write a URL that appeared in a search result. Do not write directory/aggregator sites, social media, catalog vendor pages (e.g. *.bibliocommons.com, *.polarislibrary.com, *.sirsi.net are acceptable only if it is clearly the library's actual main site), or state library pages.
6. confidence: high = clearly the official site; medium = probably right (e.g. a gov-site library page you inferred is the right one); low = unsure.

Write results incrementally with Bash (append with >> using a heredoc or python) after every 5 to 10 libraries so nothing is lost. Keep your own reasoning short. When finished, report only: count done, count with a website, count blank, and a one-line list of any IDs you could not resolve.
