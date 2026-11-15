# action-test

The test repository for [relgap-action](https://github.com/sassdavid/relgap-action) and
[tfcensus-action](https://github.com/sassdavid/tfcensus-action). Nothing here is real
infrastructure.

| Workflow | What it proves |
|---|---|
| `relgap-report.yml` | report mode: `relgap outdated` SARIF in code scanning, on Linux, macOS and Windows; `fail-on` exits 3 (the `gate` job, `workflow_dispatch` only) |
| `relgap-comment.yml` | comment mode: one `relgap digest --pull-request` comment on Renovate's `mise.toml` pull requests |
| `tfcensus-report.yml` | report mode: `tfcensus drift` SARIF from recorded plans, on Linux, macOS and Windows; `fail-on` exits 3 (`gate`) |
| `tfcensus-comment.yml` | comment mode: one drift comment on pull requests that change `live/` or `plans/` |

`mise.toml` pins old versions on purpose. `plans/` and `.tfcensus.yaml` come from tfcensus's
`testdata/drift/ingest` (Apache-2.0), without its failed unit, and `live/` has a stub
`terragrunt.hcl` for each unit so code scanning can link to it. No AWS account is involved.

The fork fallback (no comment, step summary only) is tested with a fake event in each action
repo's `scripts/test-run.sh`.
