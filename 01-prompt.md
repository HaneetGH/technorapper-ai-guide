# 1. The prompt

Paste these in order. Replace the URL with yours. The first prompt is read-only on purpose. Do not start with “fix everything.”

## 1. Connect and prove the tool can see the site

```text
Use WPVibe to read my site at https://example.com (the site_info tool).
Tell me the site name and address it returns.
If it fails, show me the error and what to do next.
Do not change anything.
```

Stop if the name or URL is wrong. You are on the wrong site.

## 2. Audit before any repair

```text
Do a full WordPress audit of this site: health, plugins, theme, SEO,
ads, feeds, and performance. Read only. Write down what is broken and
why. Do not change the site yet. Do not invent Lighthouse scores,
response times, or traffic numbers. If you did not measure it, say so.
```

Ask for a short fix list before you approve work:

```text
Give me a summary that clearly says what needs to be fixed.
```

## 3. Approve a batch, then stay specific

```text
Do the fixes on that list. Start now.
Do not delete posts. Do not change permalinks.
Do not change the AdSense publisher ID.
Do not upgrade the parent theme in place.
```

When something is still wrong, name the page and the symptom. A vague “UI is broken” sends the model looking at the wrong file.

```text
The footer is still bad. Ads are still in the wrong place.
This article stays on the loading spinner for more than a minute:
https://example.com/2026/11/example-article/
Which RSS feeds are still importing, and which ones did you remove?
```

## 4. Tell it when the files are older than the database

If you download `public_html` and then fix the database through MCP, the files on disk do not contain those database fixes.

```text
I downloaded public_html before the audit. The database fixes you made
after that are not in these files. Do not assume a problem is already
fixed because you changed it through MCP. Check the code.
Do not tell me to upload the whole folder.
```

## 5. Ask which files to ship

```text
Which files do I need to upload? List the paths only.
Do not upload wp-config.php.
```

Hand over FTP only when you are ready for those paths to go live, and rotate the password afterwards if it was pasted into a chat. This repository does not contain a password.

## 6. Measure the page visitors get

Local Lighthouse and a plugin’s lab are not PageSpeed Insights. After the change is live and the edge cache is a miss:

```text
Re-run official PageSpeed Insights on https://example.com/
mobile lab. Quote the report URL. Do not invent scores.
Do not mix this lab with an earlier Lighthouse run as one chart.
If LCP got worse, say so. Chrome UX Report is field data;
if it says No Data, do not claim Core Web Vitals.
```

## 7. Ask for a report that can be published

```text
Write the report from measurements you already have.
What was there, what changed, and which numbers moved.
Do not invent response time, requests per second, or a Lighthouse score.
If a number got worse, say that too.
```

## What a good prompt keeps saying

- The site URL.
- Read-only, until you explicitly say to change it.
- What must not change (permalinks, publisher ID, the post library).
- The exact URL that is still broken.
- “Do not invent a number.”
