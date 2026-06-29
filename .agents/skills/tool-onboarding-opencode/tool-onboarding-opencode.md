# Skill: Tool Onboarding & Provisioning - OpenCode

## 1. Objective
- **Purpose:** Create reference files that point to the Single Source of Truth (`/.agents/`) in OpenCode's native runtime environment.
- **Context:** Invoked by the Meta-Agent immediately after fine-tuning or scaling the framework's enterprise workforce.

## 2. Direction of Truth
- **Immutable Rule:** `/.agents/` is the absolute Source of Truth (SSOT). Files inside this folder already contain all project-specific adaptations completed by the Meta-Agent.
- **Source Paths:**
  - Rules: `/.agents/rules/[target_role].md`
  - Workflows: `/.agents/workflows/[target_role].md`
  - MCP Config: `/.agents/mcp/mcp-settings.json`
- **Target Location:** Root-level secure container `/.clinerules/`, `/.clinerules/workflows/`, and the enterprise `mcp-settings.json` sandbox location.
- **Execution Constraint:** This skill creates REFERENCE FILES ONLY. It MUST NOT copy the full content from source files.

## 3. Provisioning Execution Steps
1. **Assert Container:** Ensure the local folder structure for `/.clinerules/workflows/` is initialized.
2. **Create Rule Reference:** Write a reference file at `/.clinerules/[target_role].md` with the following template:
   ```markdown
   # [Role Name]

   This rule is defined in: `/.agents/rules/[target_role].md`

   Please read the source file for the full instructions.
   ```
3. **Create Workflow Reference:** Write a reference file at `/.clinerules/workflows/[target_role].md` with the following template:
   ```markdown
   # [Role Name] Workflow

   This workflow is defined in: `/.agents/workflows/[target_role].md`

   Please read the source file for the full instructions.
   ```
4. **Deploy On-Prem MCP:** Copy the locked corporate MCP tool configuration from `/.agents/mcp/mcp-settings.json` straight into OpenCode's designated active `mcp-settings.json` file.
5. **Atomic Sync Verification:** If a file transport error occurs, halt the pipeline immediately to prevent partial runtime deployment or security gate leakage.
6. **Mirror State:** Ensure that any deletion or renaming of an asset in `/.agents/` triggers an immediate mirror deletion in `/.clinerules/` to prevent ghost rules.
