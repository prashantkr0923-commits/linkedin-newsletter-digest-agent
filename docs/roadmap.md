# Roadmap

## Version 1 (in progress)

| Phase | What it delivers | Check before moving on |
|---|---|---|
| 0. Sample digest | Seven weeks of real editions, marked by hand | The marks become the baseline |
| 1. Foundations | Repository, Google Sheet and Apps Script project; the baseline imported | Row counts match the source files |
| 2. Fetch and parse | Read-only Gmail access, exact weekly window, parser, failures logged | Matches the sample on all 47 emails of a test week |
| 3. Enrich | Summaries and labels, every reply checked, cost tracked per run | Model chosen by the pre-agreed rule |
| 4. Rank | Version 1 ranking in Apps Script, muting and discovery slots | Reproduces the offline test's 70% |
| 5. Report and marks | The Sunday HTML email and signed one-tap mark links | A test email records every mark correctly |
| 6. Automate and go live | The Sunday timer, a failure email, then four live Sundays | Four Sundays without a missed digest |
| 7. Release | Setup guide, licence, a template Sheet, a first-run flow for new users, the public repository | A small beta with 3 to 5 users |

## How other people will use it

- Each user copies a template Google Sheet that carries the code, adds their own AI key, and approves read-only Gmail access in their own account.
- Updates are published as numbered versions, and the Sunday email shows a line when a newer version exists.
- Feedback comes through GitHub Issues and a short form.
- Usage data is opt-in and counts only: no titles, senders or summaries.

## Experiments after version 1

- Retest the smaller enrichment model with a clearer advert rule. If it meets the same rule, it runs at about a twentieth of the cost.
- A different weight for Read later.
- An "angle" label, or a model judgement against a written interest profile, as extra ranking signals.

## Later

Reading every newsletter in Gmail, not only LinkedIn's, once the LinkedIn version shows people use it.
