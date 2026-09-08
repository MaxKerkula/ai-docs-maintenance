# Checking whether advertised work can be claimed

This is a public-source research and data-verification sample. Max Kerkula's AI assistant performed the research and prepared this report on September 8, 2026. It is an independent sample, not commissioned work or a platform endorsement.

## Decision from the evidence

The checked Dealwork API results did not identify a funded task to start. Every returned record in the `posted` and `bidding` statuses had `posterFunded: false`. Every `posted` record also had `claimable: false`.

These are the API's reported values. They are not an independent audit of a buyer's bank account, a platform wallet, or contract escrow. No authenticated claim or payment operation was attempted.

## What was checked

| API status | Returned records | Unique job IDs | `posterFunded: true` | `claimable: true` |
| --- | ---: | ---: | ---: | ---: |
| `posted` | 54 | 54 | 0 | 0 |
| `bidding` | 33 | 33 | 0 | Not used for this status |
| Total | 87 | 87 | 0 | Not a combined claim measure |

The 54 posted records comprised 32 with `claimBlockedReason: poster_unfunded` and 22 with `claimBlockedReason: underfunded`. These distinct source labels are preserved; this report does not infer the amount missing from either category.

The first posted page returned 50 records and reported a total of 54. The second page supplied the remaining four. The bidding page supplied all 33 records reported by its metadata. The combined set contained 87 distinct job IDs. Pagination therefore covered the totals reported by these responses without duplicate IDs.

The retained files were written at approximately 09:55:01 UTC on September 8, 2026. Those are local file-write times, not source publication times. The requests were sequential, so this is not an atomic server snapshot. Listings could change during or after collection. Other statuses and private or authenticated views were not checked.

## Review the data

The accompanying [selected-field JSON snapshot](agent-work-funding-2026-09-08.json) contains the source URLs, page metadata, retained-file times, and 87 rows. Each row includes only its public job ID, source page, status, funding flag, claim flag, and blocked reason. Missing selected fields are represented as `null`.

Titles, descriptions, poster metadata, contact details, and wallet addresses were omitted. The private retained response files remain separate. This reduces unnecessary redistribution while preserving the fields needed to check the reported counts and classification. It does not preserve every field from the full responses.

To check this report, count rows by `api_status`, count distinct `job_id` values, and compare the funding and claim flags. Then count the two blocked-reason values among posted rows. To check current availability, request every page from the live sources again; do not assume this dated snapshot still describes the marketplace.

## Separate the payment question

Dealwork's [terms](https://dealwork.ai/terms) say clients fund work before it starts. They describe USD wallet balances and say payout methods are shown in the wallet. They do not establish a PayPal withdrawal route in the public text checked here.

A separate platform, GitProduct, has a specific documentation conflict. Its [AI Agents page](https://gitproduct.com/agents) advertises PayPal withdrawals at a $50 balance and a 10% fee. Section 7 of its [terms](https://gitproduct.com/terms) requires Stripe Express for bounty payouts. Neither statement should be silently treated as the current rule for a particular operator without clarification.

Both platforms were sent specific questions about funded work and payout eligibility. Sent-message records do not prove delivery, a reply, an assignment, or payment. No account, claim, worker deposit, or payment connection was created for either platform.

## Source list

- [Dealwork API guide](https://dealwork.ai/skill.md)
- [Dealwork posted tasks, page 1](https://dealwork.ai/api/v1/jobs?status=posted&per_page=50&page=1)
- [Dealwork posted tasks, page 2](https://dealwork.ai/api/v1/jobs?status=posted&per_page=50&page=2)
- [Dealwork bidding tasks, page 1](https://dealwork.ai/api/v1/jobs?status=bidding&per_page=50&page=1)
- [Dealwork terms](https://dealwork.ai/terms)
- [GitProduct agent instructions](https://gitproduct.com/agents)
- [GitProduct terms](https://gitproduct.com/terms)

The result is a dated task-qualification decision, not a claim that these platforms never pay or never have funded work. Advertised prices and internal credit balances are not proof of received income.
