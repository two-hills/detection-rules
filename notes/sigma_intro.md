# Sigma Introduction Notes

## Anatomy of a Sigma rule

The 4 critical sections:

1. **logsource** — where the data comes from (e.g., windows / process_creation)
2. **detection.selection** — what pattern matches malicious behavior or suspicious 
3. **detection.condition** — how selections combine (and / or / not / wildcards)
4. **level** — severity if rule fires

## Field modifiers

- `|contains` — substring anywhere
- `|startswith` — string begins with
- `|endswith` — string ends with
- `|re` — regular expression
- (none) — exact match

## Logic rules

- **List inside selection = OR** (any one matches)
- **Multiple fields in selection = AND** (all must match)
- **`all of selection_*`** = wildcard match all blocks starting with `selection_`
- **`condition: selection and not filter`** = match selection but exclude filter

## Common gotchas

- Quote strings with `:` to avoid YAML parse errors (e.g. `'/v:'`)
- Use consistent indentation (4 spaces, SigmaHQ convention)
- Filter blocks must be referenced in the condition, otherwise they do nothing
- Anchor CLI flag matches with separator (`/v:` not `/v`) to avoid noise

## The `level:` field — what it really means

Not just "how confident is this malicious." It's a blend of:

1. Confidence — likelihood activity is malicious
2. Severity / impact — how bad if real
3. Policy violation — even non-malicious activity that breaks org policy
4. Operational noise — false positive rate at scale

**Default new rules to `medium`. Tune based on production data.**

This matches OpenAI JD's "rule lifecycle management" requirement. Status of rule might also can be indicate of level as it show status of rule progress.

## Key principle from Day 4

**Combining weak signals creates strong detection.**

A rule that matches "powershell ran" alone is noise.
A rule that matches "powershell ran AND parent was winword AND command had -EncodedCommand" is high signal.
The AND-combination is what makes detection precise.

## Rules I've written

- T1021.001_rdp_mstsc_lateral_movement (May 6, 2026)