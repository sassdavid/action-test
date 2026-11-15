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

`renovate.yml` runs Renovate weekly as the `sassdavid-renovate` GitHub App. It opens one pull
request per `mise.toml` tool, which `relgap-comment.yml` digests, and one when a new relgap-action
or tfcensus-action release comes out. Merging a `mise.toml` pull request moves the pin; to test
against an older version again, set the old pin by hand.

Unlike the tool repos, `renovate.json` doesn't group `mise.toml` updates, because comment mode is
tested on one tool per pull request. `renovate.yml` and `renovate-config.yml` are the same files as
in the tool and action repos; the setup and the reasons are in relgap's engineering
[ADR-0015](https://github.com/sassdavid/relgap/blob/main/docs/engineering/adr/0015-dependency-updates-renovate.md).

`mise.toml` started with old pins for the M14 evidence. `plans/` and `.tfcensus.yaml` come from
tfcensus's `testdata/drift/ingest` (Apache-2.0), without its failed unit, and `live/` has a stub
`terragrunt.hcl` for each unit so code scanning can link to it. No AWS account is involved.

The fork fallback (no comment, step summary only) is tested with a fake event in each action
repo's `scripts/test-run.sh`.
