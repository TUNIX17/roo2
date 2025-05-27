# Tool Usage Guidelines Index

To ensure correct and efficient tool usage, and to prevent common errors, all agents **MUST** refer to these detailed guidelines before invoking any tool. Understanding these rules, which align with the **official Roocode tool documentation**, is critical for successful task execution.

## 1. General Tool Usage Principles (Applicable to ALL tools)

*   **Parameter Completeness & Correctness**: ALWAYS include all required parameters for each tool, exactly as specified in its official documentation. Consult the specific guideline document for a tool if unsure about its official parameters.
*   **File Existence Verification**: BEFORE attempting to read from or modify any file with tools like `<read_file>`, `<apply_diff>`, `<insert_content>`, or `<search_and_replace>`, ALWAYS verify its existence (e.g., using `<list_files>` or by confirming its presence in the provided context). Do NOT assume a file exists. This is not necessary for `<write_to_file>` if creating a new file.
*   **Idempotency Awareness**: Understand if a tool operation is idempotent. For operations that are not (e.g., appending content with `<insert_content line="0">`), be cautious about repeated executions.
*   **Error Handling**: If a tool call is expected to potentially fail (e.g., `<execute_command>`, MCP calls), ensure the agent's plan includes steps to handle or report the failure gracefully to `sparc-orchestrator`.
*   **Conciseness**: Tool invocations should be as concise as possible while providing all necessary information. Avoid overly verbose content within tool tags unless essential (e.g., the `<content>` for `<write_to_file>`).
*   **Official Documentation is King**: In case of any doubt or discrepancy between these summaries and the full official Roocode tool documentation, the official documentation takes precedence.

## 2. File Operations (Editing, Reading, Listing)

*   **Comprehensive Guidelines**: [File Operations Guidelines](./03-file_operations_guidelines.md)
    *   Covers `<read_file>`, `<write_to_file>`, `<list_files>`, `<insert_content>`, and `<search_and_replace>`.
    *   Includes critical details on path formats, content handling, and mandatory parameters like `line_count` for `<write_to_file>`, and the direct parameter usage for `<insert_content>` and `<search_and_replace>` as per official docs.

## 3. Code Editing (Primarily `apply_diff`)

*   **General Code Editing Principles**: [Code Editing Guidelines](./08-code_editing.md)
    *   Provides overarching best practices for modifying code files.
*   **`apply_diff`**:
    *   **Primary Usage Guide**: [apply_diff Error Prevention & Best Practices](./02-apply_diff_guidelines.md)
    *   CRITICAL: This is the preferred tool for most code modifications.
    *   Ensure the `SEARCH` block is an *exact* match of the text in the target file, including whitespace and line breaks. Use `<read_file>` to get the exact content first.
    *   Use the `:start_line:` hint within the `<<<<<<< SEARCH` block.

## 4. Project Management & Execution Tools

*   **`execute_command`**:
    *   Refer to mode-specific rules (e.g., `devops-master` for PowerShell rules in `09-powershell_rules.md`) for appropriate commands and syntax.
    *   Be mindful of the working directory (`<cwd>`) and potential side effects.
    *   Ensure commands are non-interactive unless explicitly handled.
*   **`attempt_completion`**:
    *   Used by specialist agents to signal the end of their delegated subtask to `sparc-orchestrator`.
    *   The `<result>` should be a concise summary of the outcome and paths to key artifacts.
*   **`ask_followup_question`**:
    *   Used by any agent to request clarification from `sparc-orchestrator` or, through it, the user if context is insufficient or ambiguous.
*   **`new_task` (Orchestrator's Tool)**:
    *   Primarily used by `sparc-orchestrator` to delegate tasks to specialist agents, using `<for_agent>` and `<objective>`.
    *   Can also be used by user/agents to create subtasks with a specific `<mode_slug>` and `<message>` as per `new-task.md`.
*   **`switch_mode`**:
    *   Used by an agent to change *its own* operational mode if the current task phase requires different capabilities (e.g., `code-architect` switching to a more "design-review" mindset temporarily if supported, or to "ask" to gather info). Requires `<mode_slug>` and optional `<reason>`.

## 5. MCP (Management Control Protocol) Integration

*   **Comprehensive Guidelines**: [MCP Integration Guidelines](./06-mcp_guidelines.md)
    *   Covers `<use_mcp_tool>` and `<access_mcp_resource>`.
    *   Emphasizes secure handling of arguments (as JSON string), correct `server_name`, and `tool_name`.
    *   Crucial for interactions with external services like PerplexityAI (via `research-analyst`) for deep research.

## Key Reminders & Common Pitfalls to Avoid (Based on Official Docs):

1.  **Missing Required Parameters**: Double-check each tool's official documentation for *all* required parameters (e.g., `line_count` for `<write_to_file>`).
2.  **Incorrect Paths**: Ensure file paths are accurate and relative to the workspace root.
3.  **`apply_diff` SEARCH Mismatches**: The `SEARCH` block (between `<<<<<<< SEARCH` and `=======`) must be an *exact* match. Use `<read_file>` to confirm the content before generating the diff. The `:start_line:` hint is important.
4.  **Malformed Arguments**: For tools like `<use_mcp_tool>`, the `<arguments>` tag must contain a valid JSON string.
5.  **`write_to_file` without `line_count`**: This parameter is mandatory as per official docs.
6.  **Overwriting Files Unintentionally**: Be cautious with `<write_to_file>`. Confirm if the file should be overwritten or if `<apply_diff>` / `<insert_content>` is more appropriate.
7.  **Assuming Tool Success**: Always consider the possibility of tool failure and plan accordingly (e.g., report to orchestrator).
8.  **Incorrect `insert_content` / `search_and_replace` parameters**: Ensure you are using direct parameters (`<line>`, `<content>` for `insert_content`; `<search>`, `<replace>` for `search_and_replace`) as per their official `.md` files, not a JSON operations block unless your specific Roocode version has a documented override for this. (This version of the rules assumes alignment with official docs' direct parameters).

By adhering to these guidelines, agents can operate more reliably and efficiently, minimizing errors and improving the overall quality of the development process.