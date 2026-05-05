# Project Instructions for Codex

## Priority
- This repository root is the workspace root.
- Before modifying files, inspect the relevant index and sub-guide files under `agent_rules/`.
- Treat `agent_rules/schemas/*.json` as the canonical source for machine-checkable rules.
- If a Markdown guide and a schema JSON conflict, follow the schema JSON.
- P0 safety rules always apply even if a schema file is missing.

## Rule Map
- Execution behavior starts at `agent_rules/codex_execution_guide.md`.
  - File changes, git status, and existing-change protection: `agent_rules/execution/git_and_file_changes.md`
  - Windows, PowerShell, UTF-8, script execution, dangerous commands, network, and install rules: `agent_rules/execution/command_safety.md`
  - Python execution, PDF rendering, and render-cache handling: `agent_rules/execution/python_and_pdf_rendering.md`
  - JSON canonical-source handling, validation execution, failure handling, and reporting: `agent_rules/execution/json_and_reporting.md`
  - Dangerous-command and git-check criteria: `agent_rules/schemas/codex_policy.json`
- Input and scope handling starts at `agent_rules/input_guide.md`.
  - Scope anchor rules: `agent_rules/input/scope_anchor_rules.md`
  - PDF reading strategy: `agent_rules/input/pdf_reading_strategy.md`
  - Uncertainty tags: `agent_rules/input/uncertainty_tags.md`
  - Input and PLAN logging: `agent_rules/input/input_plan_logging.md`
  - Image troubleshooting: `agent_rules/input/image_troubleshooting.md`
- PLAN handling starts at `agent_rules/memory_management_guideline.md`.
  - PLAN language policy: `agent_rules/plan/plan_language_policy.md`
  - Mini vs Full PLAN rules: `agent_rules/plan/mini_full_plan.md`
  - Blocker and resume handling: `agent_rules/plan/blocker_and_resume.md`
- Markdown lecture-note writing starts at `agent_rules/note_creation_guide.md`.
  - Output language: `agent_rules/writing/output_language_policy.md`
  - Markdown note style: `agent_rules/writing/markdown_note_style.md`
  - LaTeX style: `agent_rules/writing/latex_style.md`
  - Note examples: `agent_rules/examples/note_examples.md`
- Validation starts at `agent_rules/validation_guide.md`.
  - Validation overview: `agent_rules/validation/validation_overview.md`
  - Structure and LaTeX validation: `agent_rules/validation/structure_and_latex_validation.md`
  - Input coverage validation: `agent_rules/validation/input_coverage_validation.md`
  - Report validation: `agent_rules/validation/report_validation.md`
  - Mechanical validation rules: `agent_rules/schemas/validation_rules.json`
  - Final report templates: `agent_rules/schemas/report_templates.json`
- Web cross-validation is limited to terminology and formula verification at the final review stage and starts at `agent_rules/web_cross_validation_guide.md`.

## Encoding and File Reading (Mandatory)
- Always read text-based project files with explicit UTF-8 decoding. This applies to `.md`, `.txt`, `.json`, `.yaml`, `.yml`, `.ps1`, `.py`, `.js`, `.ts`, `.html`, `.css`, and other source/config text files.
- Do not rely on the Windows/PowerShell default encoding, terminal code page, or tool-specific fallback encoding.
- Preferred PowerShell read command: `Get-Content -Raw -Encoding UTF8 "path/to/file"`.
- Preferred PowerShell search command: `Select-String -Encoding UTF8 -Path "path/to/file" -Pattern "..."`.
- Preferred Python read pattern: `Path(path).read_text(encoding="utf-8")` or `open(path, encoding="utf-8")`.
- If Korean text or symbols appear garbled, first re-read the file explicitly as UTF-8 before interpreting, editing, or reporting an issue.
- If a file still cannot be decoded as UTF-8, record it as an encoding issue and do not silently rewrite or guess its contents.
- Binary files such as `.pdf`, `.png`, `.jpg`, and `.zip` must not be read as plain text; use the appropriate parser, renderer, or extraction tool. Any extracted sidecar text, markdown, JSON, or logs must still be read as UTF-8.

## Scope Control (Critical)
- For lecture-note tasks, determine the content scope using `primary_scope_anchor` as defined by `agent_rules/input_guide.md` and `agent_rules/input/scope_anchor_rules.md`.
- Do not include content that is not explicitly supported by the `primary_scope_anchor`.
- Do not expand content using slides, PDFs, images, OCR, or external sources unless the relevant scope rules or the user explicitly allow it.
- Record uncertain readings, inferred matches, and unresolved scope issues in the PLAN or final report instead of presenting them as certain body content.

## Language Policy
- AI guideline documents may be written in Korean or English.
- PLAN files follow `agent_rules/plan/plan_language_policy.md` and may mix English and Korean when allowed there.
- Final markdown lecture notes must be written in Korean by default.
- Do not produce English lecture notes unless explicitly requested by the user.

## Safety
- Run the appropriate pre-work git checks from `agent_rules/schemas/codex_policy.json` before file-changing work.
- Protect existing user changes. Do not overwrite, revert, or rewrite changes you did not make unless explicitly requested.
- Treat dangerous commands according to `agent_rules/schemas/codex_policy.json`.
- Do not install packages, change execution policy, delete files, rewrite git history, or change remote state without explicit approval.
- Do not fabricate missing information.
- If interpretation is uncertain, record it as an issue instead of guessing.

## Execution Flow
- For lecture-note creation, update, cleanup, or conversion tasks:
  1. Read input and determine scope using `agent_rules/input_guide.md` and required sub-guides.
  2. Create or update the appropriate PLAN only when the task type requires it, following `agent_rules/memory_management_guideline.md`.
  3. Generate markdown content using `agent_rules/note_creation_guide.md` and required writing sub-guides.
  4. Run validation using `agent_rules/validation_guide.md`, validation sub-guides, and `agent_rules/schemas/validation_rules.json`.
  5. Perform web verification only when needed and allowed, using `agent_rules/web_cross_validation_guide.md`.
  6. Produce the final report using the template selected from `agent_rules/schemas/report_templates.json`.
- For review, dry-run, planning-only, summary, or rule-file tasks:
  - Do not create or update lecture-note PLAN files unless explicitly requested and allowed by the PLAN guide.
  - Treat `inbox/` and manifests as read-only unless the task is confirmed as `new_note` or `update_note`.
  - Use the relevant final report template from `agent_rules/schemas/report_templates.json`.

## Validation and Reporting
- Before finalizing file-changing work, run the applicable after-work git checks from `agent_rules/schemas/codex_policy.json`.
- Separate modified, deleted, untracked, and staged files in the final report.
- Distinguish existing dirty worktree changes from changes made during the current task whenever possible.
- Separate completed work, skipped work, unresolved uncertainties, validation failures, and environment issues.
- If a required JSON schema is missing, continue only where safe, skip the schema-dependent mechanical check, and report the skipped check.
