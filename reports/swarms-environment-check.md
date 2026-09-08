# Work sample: optional provider keys in a setup check

Checked on September 7, 2026, using Windows and Python 3.13.12.

## Problem

The Swarms environment guide permits users to configure only their selected providers. Its quick verification example nevertheless slices the value returned for `OPENAI_API_KEY` before checking whether it exists.

In an isolated Anthropic-only configuration, that example raises `TypeError`. When an OpenAI key exists, the example prints its first ten characters.

The inspected baseline is [commit bfa65868ddd1152ec46406eb896e6f4baad29061](https://github.com/The-Swarm-Corporation/swarms-framework-docs/blob/bfa65868ddd1152ec46406eb896e6f4baad29061/environment-setup.mdx).

## Correction

The proposed change reports whether OpenAI, Anthropic, and Groq keys contain nonempty values. It prints no key characters. The accompanying text explains that a configured key is not necessarily valid, and that unused providers can report `False`.

The exact change is [commit f69a6cdb2f0188eca88bacdde5b05c19d828a4c6](https://github.com/MaxKerkula/swarms-framework-docs/commit/f69a6cdb2f0188eca88bacdde5b05c19d828a4c6), submitted as [PR #68](https://github.com/The-Swarm-Corporation/swarms-framework-docs/pull/68).

## Verification

The checks execute the Python block extracted from the changed page. Each subprocess receives a clean environment and a temporary `.env` file containing synthetic values. The installed `python-dotenv` package loads that file. No real provider credentials or model API calls are used.

| Check | Result |
| --- | --- |
| Baseline with only an Anthropic key | Raises `TypeError` |
| Absent, empty, and nonempty values for three providers | All 27 combinations checked |
| Each combination with and without a workspace value | 54 cases pass |
| Process exit, exact output, stderr, and synthetic-key disclosure | Checked in each case |
| `git diff --check` | Pass |

The recorded successful result is:

```text
PASS: 54 isolated configuration cases.
```

## Limits and status

These checks establish the behavior of one documentation example. They do not validate provider credentials or demonstrate a successful model request.

Correction added September 7: Mintlify CLI 4.2.876 reported 232 links in 70 files on native Windows for both trees. This did not establish 232 broken links. A fresh comparison on the same baseline checkout found no broken links using Linux Node in WSL. An isolated fixture reproduces a Windows false positive for an existing nested page; Linux accepts that page and still rejects an intentionally missing page. See the [cross-platform verification report](mint-windows-link-check.md). This patch changes no links. A visual site preview was not verified for this contribution.

The pull request is a submitted contribution. A similar earlier contribution, #65, was closed unmerged by its author. Review, merge, bounty eligibility, and payment are separate outcomes; this report claims none of them.

This work used AI assistance. Swarms has not endorsed this service.
