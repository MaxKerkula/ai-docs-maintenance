# Mintlify link checks on Windows and Linux

Checked September 7, 2026. This report corrects the earlier description of 232 existing broken links. It is an AI-assisted local investigation, not a customer endorsement or an upstream fix.

## Same files, different results

The baseline is Swarms documentation commit `bfa65868ddd1152ec46406eb896e6f4baad29061`. Both runs read the same checkout on the Windows filesystem. The Linux run uses WSL with a separately installed Linux CLI and its dependencies.

| Input | Native Windows | Linux in WSL |
| --- | --- | --- |
| Fixture with existing shallow and nested pages | Reports the existing nested page; exit 1 | No broken links; exit 0 |
| Same fixture with one missing-page link added | Reports the existing nested page and the missing page; exit 1 | Reports only the missing page; exit 1 |
| Unchanged Swarms documentation checkout | Reports 232 links in 70 files; exit 1 | No broken links; exit 0 |

Both installations use Mint CLI 4.2.876. Windows uses Node.js 22.23.2; WSL uses Node.js 22.22.0. The Node patch versions differ, so this is not an experiment with every runtime variable held constant. The observed Windows false positive is consistent with the independently reported [Mintlify issue #5680](https://github.com/mintlify/docs/issues/5680).

## Minimal fixture

The test project has `docs.json`, `index.mdx`, `guide/shallow.mdx`, and `guide/nested/deep.mdx`. Navigation includes all three pages. The index contains these links:

```markdown
[Shallow page](/guide/shallow)
[Nested page](/guide/nested/deep)
```

The negative control adds `[Missing page](/guide/nested/missing)` without creating that page. Both runs use the installed CLI's `broken-links` command. The Linux negative result shows that its passing result is not obtained by suppressing all failures.

## Interpretation and limits

The evidence establishes a Windows false positive in the fixture and a platform-dependent report for the full checkout. It does not prove the internal code cause or validate every full-site link individually. The earlier claim of 232 existing defects was too strong and is withdrawn. A clean Linux link check does not establish external URL availability, correct page rendering, or correct Python examples.

Use a Linux Node/npm installation and a separate Linux cache when checking from WSL. Reusing Windows native dependencies can fail before the check runs. Check the edited checkout, retain actual failures, and record the runtime and source revision. No credentials, paid APIs, or customer data were used in these checks.
