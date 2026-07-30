<!-- markdownlint-disable -->

# Hardening Report: tj-actions--coverage-badge-go/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **tj-actions--coverage-badge-go/v3.0.0** was hardened automatically. 18 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The composite action's single `run:` block directly interpolates multiple `${{ inputs.* }}` expressions inside shell command strings without routing them through env vars. An attacker-controlled caller can supply values containing shell metacharacters (`;`, `|`, `$(...)`, etc.) that will be executed by the shell. Offending lines include:
- `if [[ -n '${{ inputs.color }}'  ]]; then` (and similar for inputs.green, inputs.target, inputs.text, inputs.value, inputs.yellow, inputs.link)
- `EXTRA_ARGS="${EXTRA_ARGS} -color=${{ inputs.color }}"`  (and similar for all other inputs)
- `$(go env GOPATH)/bin/gobadge -filename=${{ inputs.filename }} $EXTRA_ARGS`
All 15 expression interpolations in this step are direct script-injection risks.

Locations:

- `action.yml:31`

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions and third-party actions by mutable tags or version strings instead of full 40-character commit SHAs. Unpinned references are vulnerable to supply-chain attacks if the tag is moved or the repository is compromised.

codacy-analysis.yml:
- `codacy/codacy-analysis-cli-action@v4.4.0`
- `github/codeql-action/upload-sarif@v3`

codeql.yml:
- `github/codeql-action/init@v3`
- `github/codeql-action/autobuild@v3`
- `github/codeql-action/analyze@v3`

rebase.yml:
- `cirrus-actions/rebase@1.8`

sync-release-version.yml:
- `tj-actions/release-tagger@v4`
- `tj-actions/sync-release-version@v13`
- `tj-actions/git-cliff@v1`
- `peter-evans/create-pull-request@v6.0.5`

test.yml:
- `reviewdog/action-golangci-lint@v2`
- `actions/setup-go@v5`
- `tj-actions/verify-changed-files@v19`
- `peter-evans/create-pull-request@v6`

update-readme.yml:
- `tj-actions/auto-doc@v3`
- `tj-actions/remark@v3`
- `tj-actions/verify-changed-files@v19`
- `peter-evans/create-pull-request@v6`

Locations:

- `.github/workflows/codacy-analysis.yml:34`
- `.github/workflows/codacy-analysis.yml:44`
- `.github/workflows/codeql.yml:40`
- `.github/workflows/codeql.yml:53`
- `.github/workflows/codeql.yml:67`
- `.github/workflows/rebase.yml:14`
- `.github/workflows/sync-release-version.yml:13`
- `.github/workflows/sync-release-version.yml:15`
- `.github/workflows/sync-release-version.yml:18`
- `.github/workflows/sync-release-version.yml:21`
- `.github/workflows/test.yml:26`
- `.github/workflows/test.yml:31`
- `.github/workflows/test.yml:44`
- `.github/workflows/test.yml:52`
- `.github/workflows/update-readme.yml:15`
- `.github/workflows/update-readme.yml:18`
- `.github/workflows/update-readme.yml:22`
- `.github/workflows/update-readme.yml:32`

### missing-permissions (severity: medium)

Five workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, workflows run with the default (often write) token permissions, violating the principle of least privilege.

- codacy-analysis.yml: no permissions at top level or on job `codacy-security-scan`
- rebase.yml: no permissions at top level or on job `rebase`
- sync-release-version.yml: no permissions at top level or on job `update-version`
- test.yml: no permissions at top level or on job `test`
- update-readme.yml: no permissions at top level or on job `sync-assets`

Locations:

- `.github/workflows/codacy-analysis.yml:1`
- `.github/workflows/rebase.yml:1`
- `.github/workflows/sync-release-version.yml:1`
- `.github/workflows/test.yml:1`
- `.github/workflows/update-readme.yml:1`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.color }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:37`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.color }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:38`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.green }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:41`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.green }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:42`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.target }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:45`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.target }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:46`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.text }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:49`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.text }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:50`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.value }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:53`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.value }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:54`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.yellow }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:57`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.yellow }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:58`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.link }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:61`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.link }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:62`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.filename }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:66`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all findings:
1. script-injection/static-inline-injection in action.yml: Moved all 8 `${{ inputs.* }}` expressions (color, green, target, text, value, yellow, link, filename) into an `env:` block on the step. The shell script now references them as plain env vars ($INPUT_COLOR, etc.), eliminating all 15 injection points.
2. unpinned-uses: Pinned all 18 unpinned action references across codacy-analysis.yml, codeql.yml, rebase.yml, sync-release-version.yml, test.yml, and update-readme.yml to full 40-character commit SHAs with original tags preserved in comments.
3. missing-permissions: Added top-level `permissions:` blocks to codacy-analysis.yml (contents:read, security-events:write), rebase.yml (contents:write, pull-requests:read), sync-release-version.yml (contents:write, pull-requests:write), test.yml (contents:write, pull-requests:write), and update-readme.yml (contents:write, pull-requests:write). Note: codeql.yml already had job-level permissions so it was not listed in the missing-permissions finding.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed the unquoted $EXTRA_ARGS expansion in the gobadge invocation by converting from a string variable to a bash array. Each optional flag is now appended as a separate array element (e.g., EXTRA_ARGS+=("-color=$INPUT_COLOR")), and the final command uses "${EXTRA_ARGS[@]}" to expand each element as a properly-quoted, separate argument. This prevents shell metacharacter injection through user-controlled inputs (color, green, target, text, value, yellow, link) while preserving correct argument passing semantics.

