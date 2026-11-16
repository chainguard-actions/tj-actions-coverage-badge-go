<!-- markdownlint-disable -->

# Hardening Report: tj-actions--coverage-badge-go/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **tj-actions--coverage-badge-go/v3.0.0** was hardened automatically. 17 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): the composite step 'coverage-badge-go' in action.yml puts GitHub Actions expressions straight into the run: script. These include inputs.color, inputs.green, inputs.target, inputs.text, inputs.value, inputs.yellow, inputs.link and inputs.filename. Examples: `if [[ -n '${{ inputs.color }}'  ]]; then`, `EXTRA_ARGS="${EXTRA_ARGS} -color=${{ inputs.color }}"` and `$(go env GOPATH)/bin/gobadge -filename=${{ inputs.filename }} $EXTRA_ARGS`. The calling workflow supplies these values, and they are pasted into the script before bash runs it, so a crafted value can run arbitrary commands. Rule (b) also applies: the input-derived $EXTRA_ARGS is expanded unquoted, and this line has no shellcheck SC2086/SC2206 directive. Fix: move each input into the step's env: block, quote every expansion, and build the arguments in a bash array.

Locations:

- `action.yml:37`
- `action.yml:38`
- `action.yml:66`

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

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index 7e4c4b2..d687f6c 100644
--- a/action.yml
+++ b/action.yml
@@ -27,46 +27,70 @@ inputs:
   link:
     description: 'Optional URL when you click the badge'
     required: false
-
 runs:
   using: 'composite'
   steps:
-    - run: |
-        EXTRA_ARGS=""
+    - env:
+        INPUT_FILENAME: ${{ inputs.filename }}
+        INPUT_COLOR: ${{ inputs.color }}
+        INPUT_GREEN: ${{ inputs.green }}
+        INPUT_TARGET: ${{ inputs.target }}
+        INPUT_TEXT: ${{ inputs.text }}
+        INPUT_VALUE: ${{ inputs.value }}
+        INPUT_YELLOW: ${{ inputs.yellow }}
+        INPUT_LINK: ${{ inputs.link }}
+      run: |
+        EXTRA_ARGS=()
 
-        if [[ -n '${{ inputs.color }}'  ]]; then
-          EXTRA_ARGS="${EXTRA_ARGS} -color=${{ inputs.color }}"
+        if [[ -n "$INPUT_COLOR" ]]; then
+          EXTRA_ARGS+=("-color=$INPUT_COLOR")
         fi
 
-        if [[ -n '${{ inputs.green }}'  ]]; then
-          EXTRA_ARGS="${EXTRA_ARGS} -green=${{ inputs.green }}"
+        if [[ -n "$INPUT_GREEN" ]]; then
+          EXTRA_ARGS+=("-green=$INPUT_GREEN")
         fi
 
-        if [[ -n '${{ inputs.target }}'  ]]; then
-          EXTRA_ARGS="${EXTRA_ARGS} -target=${{ inputs.target }}"
+        if [[ -n "$INPUT_TARGET" ]]; then
+          EXTRA_ARGS+=("-target=$INPUT_TARGET")
         fi
 
-        if [[ -n '${{ inputs.text }}'  ]]; then
-          EXTRA_ARGS="${EXTRA_ARGS} -text=${{ inputs.text }}"
+        if [[ -n "$INPUT_TEXT" ]]; then
+          EXTRA_ARGS+=("-text=$INPUT_TEXT")
         fi
 
-        if [[ -n '${{ inputs.value }}'  ]]; then
-          EXTRA_ARGS="${EXTRA_ARGS} -value=${{ inputs.value }}"
+        if [[ -n "$INPUT_VALUE" ]]; then
+          EXTRA_ARGS+=("-value=$INPUT_VALUE")
         fi
 
-        if [[ -n '${{ inputs.yellow }}'  ]]; then
-          EXTRA_ARGS="${EXTRA_ARGS} -yellow=${{ inputs.yellow }}"
+        if [[ -n "$INPUT_YELLOW" ]]; then
+          EXTRA_ARGS+=("-yellow=$INPUT_YELLOW")
         fi
 
-        if [[ -n '${{ inputs.link }}'  ]]; then
-          EXTRA_ARGS="${EXTRA_ARGS} -link=${{ inputs.link }}"
+        if [[ -n "$INPUT_LINK" ]]; then
+          EXTRA_ARGS+=("-link=$INPUT_LINK")
         fi
-        
+
         go install github.com/AlexBeauchemin/gobadge@v0.4.0
-        $(go env GOPATH)/bin/gobadge -filename=${{ inputs.filename }} $EXTRA_ARGS
+        $(go env GOPATH)/bin/gobadge -filename="$INPUT_FILENAME" "${EXTRA_ARGS[@]}"
       id: coverage-badge-go
       shell: bash
-
 branding:
   icon: check-circle
   color: white

```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, prior-fix

**Notes:**

Moved all inputs (filename, color, green, target, text, value, yellow, link) into the step env: block; replaced string EXTRA_ARGS with a bash array of single-value '-flag=value' entries; quoted the gobadge invocation and -filename. Re-applies the previous harden patch.

