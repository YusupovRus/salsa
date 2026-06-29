# Skill: Tool Onboarding & Provisioning - Cline

## 1. Objective
- **Purpose:** Create reference files that point to the Single Source of Truth (`/.agents/`) in Cline's native runtime environment.
- **Context:** Invoked by the Meta-Agent immediately after completing any agent adaptation, tooling update, or MCP infrastructure tuning.

## 2. Direction of Truth & Write Boundaries
- **Immutable Rule:** `/.agents/` is the absolute Source of Truth (SSOT). Files inside this folder already contain all project-specific adaptations completed by the Meta-Agent.
- **Source Paths:** 
  - Rules: `/.agents/rules/[target_role].md`
  - Workflows: `/.agents/workflows/[target_role].md`
  - MCP Config: `/.agents/mcp/mcp-settings.json`
- **Target Runtime Environment:** `/.clinerules/` (for roles), `/.clinerules/workflows/` (for sequences), and the tool's expected active `mcp-settings.json` file.
- **Execution Constraint:** This skill creates REFERENCE FILES ONLY. It MUST NOT copy the full content from source files.

## 3. Provisioning Execution Steps
When a synchronization or initial bootstrap is triggered for Cline, execute these atomic operations:

1. **Assert Environment:** Verify the physical presence of the `/.clinerules/workflows/` folder structure at the root. Create if missing.
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
4. **Deploy MCP Settings:** Copy the verified master MCP configuration directly from `/.agents/mcp/mcp-settings.json` into the target environment's active `mcp-settings.json` file to align low-level tool access with the project matrix.
5. **Mirror State:** Ensure that any deletion or renaming of an asset in `/.agents/` triggers an immediate mirror deletion in `/.clinerules/` to prevent ghost rules.
