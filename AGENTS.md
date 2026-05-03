# Project Instructions for Codex

## Priority
- This repository root is the workspace root.
- Before modifying files, inspect relevant files under agent_rules/.
- Follow `agent_rules/codex_execution_guide.md` for execution behavior.
- Follow `agent_rules/input_guide.md` for scope determination and data merging.
- Follow `agent_rules/note_creation_guide.md` for markdown output rules.
- Follow `agent_rules/validation_guide.md` before finalizing outputs.
- Follow `agent_rules/web_cross_validation_guide.md` only for terminology and formula verification at the final review stage.

## Encoding and File Reading (Mandatory)
- Always read text-based project files with explicit UTF-8 decoding. This applies to `.md`, `.txt`, `.json`, `.yaml`, `.yml`, `.ps1`, `.py`, `.js`, `.ts`, `.html`, `.css`, and other source/config text files.
- Do not rely on the Windows/PowerShell default encoding, terminal code page, or tool-specific fallback encoding when reading project files.
- Preferred PowerShell read command: `Get-Content -Raw -Encoding UTF8 "path/to/file"`.
- Preferred PowerShell search command: `Select-String -Encoding UTF8 -Path "path/to/file" -Pattern "..."`.
- Preferred Python read pattern: `Path(path).read_text(encoding="utf-8")` or `open(path, encoding="utf-8")`.
- If Korean text or symbols appear garbled, first re-read the file explicitly as UTF-8 before interpreting, editing, or reporting an issue.
- If a file still cannot be decoded as UTF-8, record it as an encoding issue and do not silently rewrite or guess its contents.
- Binary files such as `.pdf`, `.png`, `.jpg`, and `.zip` must not be read as plain text; use the appropriate parser, renderer, or extraction tool. Any extracted sidecar text, markdown, JSON, or logs must still be read as UTF-8.

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