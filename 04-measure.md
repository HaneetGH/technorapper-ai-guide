# 4. How we measured

Publish the number you measured. Say when a second number got worse.

## What we ran

- Mobile Lighthouse on `https://technorapper.com/`, all four categories, one run on 1 November and again on 2 November.
- A normal browser download of the homepage, one article, and a category archive, with the cache header recorded.
- A count of the live HTML: “Read More”, `javascript:`, the stylesheet version, and the new section headings.

There is no Chrome UX Report field data for this property. Lab only.

## What moved

| Measure | Before | After |
| --- | ---: | ---: |
| Performance | 41 | 69 |
| Best practices | 96 | 100 on two runs |
| Cumulative layout shift | 0.387 | 0.005 |
| First Contentful Paint | 2.5 s | 1.7 s |
| Speed Index | 9.6 s | 5.1 s |
| Total blocking time | 210 ms | 140 ms |
| Unused JavaScript | 429 KiB | about 240–300 KiB |

0.387 / 0.005 is about 77 times smaller. 210 ms to 140 ms is less main-thread blocking. Performance 41 to 69 is 28 points, about a 68 percent rise on that score. Say both the points and the ratio, and say it is one lab run each day.

## What did not move, or got worse

| Measure | Before | After |
| --- | ---: | ---: |
| SEO | 85 | 85 |
| Accessibility | 90 | 89 |
| Largest Contentful Paint | 13.8 s | 16.8 s |
| Homepage time to first byte | about 2.3 s | about 2.5 s |
| Lab transfer | 3,833 KiB | about 9,400 KiB |

The last best-practices run scored 81 because of one deprecated-API warning. The two runs before it scored 100. Treat 81 as that run, not as a new baseline you can ignore, and do not average it into a prettier number.

SEO failed the same two checks both days: ten links with weak text, and links that are not crawlable. The homepage was then changed so those strings are gone. That later HTML was not put through Lighthouse again in this write-up.

The transfer grew on the second lab run. Report it. A bigger page is not a win.

## What we refused to publish

- A requests-per-second or “TPS” figure. None was measured.
- “The server is faster.” The document still takes about two to three seconds.
- An LCP win. It got slower.
- Any score from a write-up that said “no production changes” and “no after metrics.” That note contradicts the measured before and after above. Do not put the two side by side.

## A check you can repeat

After an upload:

```bash
curl -sS -D - -o /tmp/home.html -A 'Mozilla/5.0' 'https://example.com/?v=check'
```

Read `x-proxy-cache`. Then search the saved HTML for the string you changed (a CSS version, a heading, a link phrase). If the string is missing, the file is not the one visitors are getting, and a Lighthouse score from that URL will describe the old page.
