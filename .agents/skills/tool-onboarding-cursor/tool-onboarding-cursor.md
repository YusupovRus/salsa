# Skill: Tool Onboarding & Provisioning - Cursor

## 1. Objective
- **Purpose:** Create reference files that point to the Single Source of Truth (`/.agents/`) in Cursor's native runtime environment.
- **Context:** Invoked by the Meta-Agent immediately after fine-tuning or scaling framework core manifests.

## 2. Direction of Truth & File Formatting
- **Immutable Rule:** `/.agents/` remains the absolute Source of Truth (SSOT).
- **Target Subdirectories:** 
  - Rules: `/.cursor/rules/[role_name].mdc` (Requires Markdown with YAML Frontmatter).
  - Workflows: `/.cursor/commands/[role_name].md`
- **Execution Constraint:** This skill creates REFERENCE FILES ONLY with YAML frontmatter. It MUST NOT copy the full content from source files.

## 3. Provisioning & Envelope Wrapping Steps
When synchronization is triggered for Cursor, execute these atomic operations for every active role in the matrix:

1. **Assert Containers:** Initialize and verify the physical presence of `/.cursor/rules/` and `/.cursor/commands/` folders at the workspace root.
2. **Create Rule Reference (.mdc):** Write a reference file at `/.cursor/rules/[target_role].mdc` with the following template:
   ```yaml
   ---
   description: Core operational constraints and behavioral directives for the Fast-ASDLC [Role Name] Agent.
   globs: [Insert Allowed Paths, e.g., "src/**/*, tests/**/*"]
   alwaysApply: true
   ---
   
   # [Role Name]
   
   This rule is defined in: `/.agents/rules/[target_role].md`
   
   Please read the source file for the full instructions.
   ```
3. **Create Workflow Reference:** Write a reference file at `/.cursor/commands/[target_role].md` with the following template:
   ```markdown
   # [Role Name] Workflow
   
   This workflow is defined in: `/.agents/workflows/[target_role].md`
   
   Please read the source file for the full instructions.
   ```
4. **Deploy MCP Settings:** Copy the verified master MCP configuration directly from `/.agents/mcp/mcp-settings.json` into Cursor's project-level active configuration endpoint to enforce unified tool syncing.

## 4. Operational Guardrails
- **Sync Lock:** Any removal or renaming of an agent manifest inside `/.agents/` must trigger an immediate mirror purge inside `/.cursor/rules/` and `/.cursor/commands/` to eliminate zombie rules.
- **Fail-Fast Trigger:** If the file system blocks writing to the `/.cursor/` directory tree, abort the bootstrap sequence immediately and alert the human supervisor.
