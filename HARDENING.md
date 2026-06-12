<!-- markdownlint-disable -->

# Hardening Report: tj-actions--coverage-badge-go/v3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **tj-actions--coverage-badge-go/v3** was hardened automatically. 16 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The composite action's single `run:` block in action.yml directly interpolates `${{ inputs.* }}` expressions into shell command strings (rule a). GitHub Actions substitutes these values before the shell parses the script, so a caller supplying a value containing shell metacharacters (`;`, `|`, `$(...)`, backticks, etc.) achieves arbitrary command execution on the runner. All eight inputs are affected:
- Line 36: `if [[ -n '${{ inputs.color }}'  ]]; then`
- Line 37: `EXTRA_ARGS="${EXTRA_ARGS} -color=${{ inputs.color }}"`
- Line 40: `if [[ -n '${{ inputs.green }}'  ]]; then`
- Line 41: `EXTRA_ARGS="${EXTRA_ARGS} -green=${{ inputs.green }}"`
- Line 44: `if [[ -n '${{ inputs.target }}'  ]]; then`
- Line 45: `EXTRA_ARGS="${EXTRA_ARGS} -target=${{ inputs.target }}"`
- Line 48: `if [[ -n '${{ inputs.text }}'  ]]; then`
- Line 49: `EXTRA_ARGS="${EXTRA_ARGS} -text=${{ inputs.text }}"`
- Line 52: `if [[ -n '${{ inputs.value }}'  ]]; then`
- Line 53: `EXTRA_ARGS="${EXTRA_ARGS} -value=${{ inputs.value }}"`
- Line 56: `if [[ -n '${{ inputs.yellow }}'  ]]; then`
- Line 57: `EXTRA_ARGS="${EXTRA_ARGS} -yellow=${{ inputs.yellow }}"`
- Line 60: `if [[ -n '${{ inputs.link }}'  ]]; then`
- Line 61: `EXTRA_ARGS="${EXTRA_ARGS} -link=${{ inputs.link }}"`
- Line 65: `$(go env GOPATH)/bin/gobadge -filename=${{ inputs.filename }} $EXTRA_ARGS`

Fix: move each input into an `env:` variable and reference it as a quoted shell variable (e.g. `"$INPUT_COLOR"`) inside the `run:` block.

Locations:

- `action.yml:36`
- `action.yml:37`
- `action.yml:40`
- `action.yml:41`
- `action.yml:44`
- `action.yml:45`
- `action.yml:48`
- `action.yml:49`
- `action.yml:52`
- `action.yml:53`
- `action.yml:56`
- `action.yml:57`
- `action.yml:60`
- `action.yml:61`
- `action.yml:65`

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

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Moved all eight ${{ inputs.* }} expressions (filename, color, green, target, text, value, yellow, link) from the run: block into a step-level env: block as INPUT_FILENAME, INPUT_COLOR, INPUT_GREEN, INPUT_TARGET, INPUT_TEXT, INPUT_VALUE, INPUT_YELLOW, and INPUT_LINK. The run: script now references only these plain environment variables (e.g. "$INPUT_COLOR", "$INPUT_FILENAME"), eliminating all shell-injection vectors.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed the unquoted `$EXTRA_ARGS` shell variable expansion in action.yml (line 68). Changed `EXTRA_ARGS` from a string to a bash array. Each optional argument is now appended as a separate quoted array element (e.g., `EXTRA_ARGS+=("-color=$INPUT_COLOR")`). The final gobadge invocation now uses `"${EXTRA_ARGS[@]}"` (properly quoted array expansion) instead of the unquoted `$EXTRA_ARGS`, preventing word-splitting and shell metacharacter injection from user-controlled inputs. Also added quotes around the `$(go env GOPATH)/bin/gobadge` command substitution for robustness.

