# Decisions

Every decision in the build is recorded with the options considered, the reason, and what evidence would change it. This is a condensed public version.

## D1. Build for one user first

**Options:** a multi-user product from the start; a single-user agent on my own inbox (chosen).

**Why:** a single user removes sign-up, account linking and other people's privacy, so the build can focus on the real question: is the ranking useful?

## D2. Leave out public engagement and reading time

**Options:** rank by likes and comments, or by time spent reading; rank by the reader's own marks and recency (chosen).

**Why:** likes and comments aren't in the emails, and reading them from LinkedIn pages would mean scraping, which LinkedIn prohibits. Reading-time tracking would need a browser extension, which is a large build for a weak signal.

## D3. The notification emails are the only data source

**Options:** a LinkedIn API (there is no consumer API for newsletters); scraping (prohibited and fragile); RSS (not offered); the notification emails in Gmail (chosen).

**Why:** the emails arrive anyway, and they carry the newsletter name, author, title, link and most of the text.

## D4. Prove the idea with a hand-made sample before writing code

**Why:** a sample digest built from real emails answers the important question cheaply: would anyone actually read a ranked digest of these? It also produced real labels for testing the ranking. The sample was extended from one week to seven, because one week left most topics with only a handful of editions and allowed no fair test.

## D5. Three marks instead of thumbs up and down

**Why:** many articles look interesting without being worth opening now. A two-way choice forces those into the wrong bucket. Read later is scored halfway (Worth it 1, Read later 0.5, Not interested 0).

## D6. Track records per newsletter, with automatic muting

**Options:** rank by topic and keywords; track records per publisher; track records per newsletter, with muting (chosen).

**Why:**
- In the sample marks, the newsletter predicted the verdict better than the topic.
- A few near-daily newsletters drew about half of all Not interested marks.
- Two newsletters from the same publisher could score completely differently.

A newsletter with at least five marks and a score of 0.15 or lower is muted, and the reader can mute or unmute any newsletter with one tap.

## D7. Version 1 ranking: newsletter and topic track records

**Decision:** edition score = 0.6 × newsletter score + 0.4 × topic score. Each score is the average of the reader's marks, blended with their overall average as if it had three extra average marks, so one early mark can't bury or boost a newsletter. Two discovery slots go to newsletters with fewer than three marks.

**Why:** on the five-week test, simple track records matched models that score the words in titles and summaries, and they are easy to explain and to build in Apps Script. Adding topic to newsletter put more Worth it articles in the top 5, and ranks a brand-new newsletter sensibly.

## D8. One Google Apps Script project and a Google Sheet

**Options:** a Python service on a server; a scheduled AI session that rebuilds a page; Apps Script with a Sheet (chosen).

**Why:** Apps Script reads Gmail with the user's own permission, runs on a timer while the laptop is off, hosts the small web app that records marks, and writes to Sheets. It is all free and in one place. The ranking is simple enough not to need Python in production; Python stays for the offline test.

## D9. A Sunday email with one-tap marks

**Options:** a hosted web page; an email with signed mark links (chosen).

**Why:** newsletters are already read in Gmail, so the digest should arrive there. Opening an article is a weak signal (about half of the opened articles in the sample ended up Not interested), so the ranking learns from marks only.

## D10. Document as I go

**Why:** every decision goes into a decision log and every prompt to the AI coding assistant into a prompts file, word for word and numbered, as it happens. Writing each decision when it is made keeps the reasoning honest.

## D11. Open source, each user running their own copy

**Decision:** after four live Sundays, the code is released on GitHub. Each user copies a template Sheet that carries the code and runs it in their own Google account, with their own AI key. There is no central service.

**Why:** nobody's email leaves their own account, except the text sent to the AI model under their own key. A shared app would also face Google's limits and security review for unverified apps; personal copies don't.

**Scope:** LinkedIn newsletters only for version 1. Reading every newsletter in Gmail comes later, after one use case works end to end and there is evidence people use it.

## D12. Read-only Gmail access

**Why:** read-only is all the product needs, and it is what each user will be asked to approve. The permission screen should promise no more than the code does. A week is fetched with an exact time window in the user's own time zone, matching the sender address exactly.

## D13. Record every failed email, never skip it

**Why:** a skipped failure is invisible, and a parse failure is the first sign that LinkedIn has changed its email layout. Each run writes a row with the reason for every email that failed.

## D14. Choose the enrichment model by the ranking it produces, with the rule fixed in advance

**Options:** choose on summary quality; choose on agreement with the sample labels; choose on the ranking the labels produce, with a margin that favours the cheaper model (chosen).

**Why:**
- Summary quality didn't separate the models: in a side-by-side review of 20 editions, each model's summary was preferred about equally often.
- Agreement with the sample labels only measures closeness to an earlier judgement.
- The ranking test measures what the labels are for. Writing the rule down before the test ran kept the choice from being fitted to the result.

The result is in [evaluation.md](evaluation.md).
