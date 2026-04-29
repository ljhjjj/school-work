# Project Instructions for Codex

## Priority
- This repository root is the workspace root.
- Before modifying files, inspect relevant files under agent_rules/.
- Follow `agent_rules/codex_execution_guide.md` for execution behavior.
- Follow `agent_rules/input_guide.md` for scope determination and data merging.
- Follow `agent_rules/note_creation_guide.md` for markdown output rules.
- Follow `agent_rules/validation_guide.md` before finalizing outputs.
- Follow `agent_rules/web_cross_validation_guide.md` only for terminology and formula verification at the final review stage.

## Scope Control (Critical)
- The lecture-note content scope must be determined by `primary_scope_anchor` as defined in `input_guide.md`.
- Do not include content that is not explicitly supported by the `primary_scope_anchor`.
- Do not expand content using slides, PDFs, or external sources unless explicitly allowed.

## Language Policy
- AI guideline documents are written in English.
- PLAN files may mix English and Korean.
- Final markdown lecture notes must be written in Korean by default.
- Do not produce English lecture notes unless explicitly requested by the user.

## Safety
- Do not overwrite user files without checking current contents.
- Do not fabricate missing information.
- If interpretation is uncertain, record it as an issue instead of guessing.

## Execution Flow
- Follow this order:
  1. Read input and determine scope (`input_guide.md`)
  2. Update or create PLAN file (`memory_management_guideline.md`)
  3. Generate markdown content (`note_creation_guide.md`)
  4. Run validation (`validation_guide.md`)
  5. Perform optional web verification if needed
  6. Produce final report

## Output
- Keep logs of changed files and validation commands.
- Final reports must distinguish:
  - Completed work
  - Skipped work
  - Unresolved uncertainties