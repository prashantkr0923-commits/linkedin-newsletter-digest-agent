# Evaluation

## The baseline

The baseline is a hand-made sample digest of seven weeks of real newsletter emails: 274 editions, 250 of them marked Worth it, Read later or Not interested, plus a few adverts and job lists that weren't marked. Every ranking is tested against these marks.

## The ranking test

The test mimics how the live product learns. For each of the last five weeks, a ranking learns only from the weeks before it, ranks that week, and is scored on its top 10.

- **hits@10:** the share of the top 10 marked Worth it or Read later;
- **Worth it in the top 10**, on average;
- **Not interested in the top 10**, on average;
- **Worth it in the top 5**, on average.

| Ranking | hits@10 | Worth it in top 10 | Not interested in top 10 | Worth it in top 5 |
|---|---|---|---|---|
| Newest first | 44% | 0.8 | 5.6 | 0.2 |
| A starter list of interests | 44% | 1.6 | 5.6 | 1.2 |
| Newsletter track record only | 68% | 3.0 | 3.2 | 1.6 |
| **Newsletter + topic track record (version 1)** | **66%** | **3.2** | **3.4** | **1.8** |
| Words in title and summary only | 60% | 3.0 | 4.0 | 1.6 |
| Words + newsletter + topic + format | 66% | 3.6 | 3.4 | 1.6 |

**Version 1 targets:**
- at least 65% hits@10;
- at least 3 Worth it in the top 10;
- at most 3.5 Not interested in the top 10.

## Choosing the enrichment model

Every edition gets a summary and labels from a language model, and the ranking learns topic track records from those labels. So the labeller was chosen by the ranking its labels produce.

**The rule, written before the test ran:** use the smaller, cheaper model if it meets all three targets and comes within 2 percentage points of the larger model on hits@10. Otherwise use the larger model.

All 274 editions were labelled by both models with the same prompt:

| Labeller | hits@10 | Worth it in top 10 | Not interested in top 10 | Worth it in top 5 | Editions ranked |
|---|---|---|---|---|---|
| The sample's own labels | 66% | 3.2 | 3.4 | 1.8 | 250 |
| Smaller model | 64% | 3.8 | 3.6 | 2.2 | 236 |
| **Larger model (chosen)** | **70%** | **3.6** | **3.0** | **2.0** | 247 |

The smaller model missed two targets and was 6 points behind, where the rule allowed 2. The main reason was its advert flag: it set aside 8 editions the reader had marked Worth it or Read later as adverts. Most were articles from companies' own newsletters. The larger model set aside 1.

**The trade-off:** the larger model costs about 20 times as much per edition. That is still small for a weekly digest. A retest of the smaller model with a clearer advert rule is on the roadmap, and if it passes the same rule, it wins.

## Parser accuracy

On a test week of 47 emails:
- the fetch returned the same 47 emails as the hand-made sample, with the same ids and the same receive times to the second;
- the parser matched the sample on all 47 for id, receive time, newsletter and link.

The few differences in author and title were places where the sample had shortened a name or tidied a title.

## Is the sample representative?

A one-year inventory of the newsletter emails counted editions and publishing frequency per newsletter. It read only the newsletter name, author and date, with no article text and no AI calls. It showed that the sample marks are representative in their mix of newsletters, but thin for monthly newsletters. A coverage round adds marks for newsletters with little history.
