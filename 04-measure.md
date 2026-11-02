# 4. How we measured

Publish the number you measured. Say when a second number got worse. Name the lab. Do not glue two labs into one before/after table.

## What we ran

- Mobile Lighthouse on `https://technorapper.com/` through WPVibe: 1 November (CoverNews, before repairs) and 2 November (CoverNews, after the first repairs).
- Official [PageSpeed Insights](https://pagespeed.web.dev/analysis/https-technorapper-com/rd9rm04l37?form_factor=mobile), mobile, later on 2 November, after the custom theme and smaller homepage images. Lighthouse 13.6.0, emulated Moto G Power, slow 4G.
- A normal browser download of the homepage, one article, and a category archive, with `x-proxy-cache` recorded.
- A count of the live HTML: “Read More”, `javascript:`, stylesheet version, headings.

There is no Chrome UX Report field data for this property. Lab only.

## Weekend Lighthouse (CoverNews)

One run each day. Same tool, same page.

| Measure | 1 November | 2 November |
| --- | ---: | ---: |
| Performance | 41 | 69 |
| Accessibility | 90 | 89 |
| SEO | 85 | 85 |
| Cumulative layout shift | 0.387 | 0.005 |
| First Contentful Paint | 2.5 s | 1.7 s |
| Speed Index | 9.6 s | 5.1 s |
| Total blocking time | 210 ms | 140 ms |
| Largest Contentful Paint | 13.8 s | 16.8 s |
| Homepage time to first byte | about 2.3 s | about 2.5 s |

Layout shift got about 77 times smaller. Performance rose 28 points. The main image got slower. SEO stayed at 85 on that lab. AIOSEO’s checklist later showed 83; that is not PageSpeed Insights.

## Official PageSpeed Insights (custom theme)

After CoverNews was replaced and homepage heroes were requested at 800px through Photon.

| Measure | PSI, 2 November |
| --- | ---: |
| Performance | 90 |
| Accessibility | 100 |
| SEO | 100 |
| First Contentful Paint | 1.0 s |
| Largest Contentful Paint | 2.7 s |
| Total Blocking Time | 150 ms |
| Speed Index | 5.5 s |
| Cumulative Layout Shift | 0 |

Do not write 69 → 90 as one continuous lab. The honest LCP line is **13.8 s → 16.8 s → 2.7 s**.

## What we refused to publish

- A requests-per-second or “TPS” figure. None was measured.
- “The server is faster.” A cache-miss homepage still takes about two seconds to first byte.
- Mixing AIOSEO 83 with Google’s SEO 100.
- Averaging two labs so a later run looks like a clean +49.

## A check you can repeat

After an upload:

```bash
curl -sS -D - -o /tmp/home.html -A 'Mozilla/5.0' 'https://example.com/?v=check'
```

Read `x-proxy-cache`. Then search the saved HTML for the string you changed (a CSS version, a heading, a link phrase). If the string is missing, the file is not the one visitors are getting, and a Lighthouse score from that URL will describe the old page.

Then open PageSpeed Insights on the same URL. Local Lighthouse on your laptop is a different lab.
