# Log table skeletons (empty)

Copy into your repo's `logs/` (or project-local path). Insert new rows
**newest-first** immediately under the sentinel.

## Agent log (AL)

```markdown
# Agent log

| When | Agent | Action | Scope | What | Why | LLM |
|------|-------|--------|-------|------|-----|-----|
<!-- AL+ -->
```

Global-style variant:

```markdown
# Agent log — global

| When | Scope | Agent | Action | Summary | Reference | LLM |
|------|-------|-------|--------|---------|-----------|-----|
<!-- AL+ -->
```

## Auto-approval log (AAL)

```markdown
# Auto-approval log

| When | Action | Scope | Summary | Reference |
|------|--------|-------|---------|-----------|
<!-- AAL+ -->
```

## Insertion rule

Match the sentinel line with StrReplace; replacement = sentinel + newline + new
row. Never append to the bottom of a newest-first table.
