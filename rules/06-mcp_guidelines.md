# Guidelines for Using Management Control Protocol (MCP) Tools

This document provides CRITICAL guidelines for all agents interacting with external services via the Management Control Protocol (MCP) tools: `<use_mcp_tool>` and `<access_mcp_resource>`. Proper MCP integration is key for tasks like deep research (via `research-analyst` using `perplexityai` MCP) and **accessing external library documentation (via the `context7` MCP)**, as per the **official Roocode MCP tool documentation**.

**Note on Project Memory:** The primary mechanism for storing and retrieving internal project context, decisions, and artifact metadata is a **file-based system** (e.g., in `.roo/memory-bank/`), orchestrated by `sparc-orchestrator` using standard file operation tools. The `context7` MCP serves the purpose of library documentation retrieval.

## 1. Core Principles for MCP Usage

1.  **Authorization & Security**:
    *   **NEVER** hardcode credentials, API keys, or access tokens within tool arguments or agent instructions. These MUST be managed via secure environment variables or Roocode's secure MCP configuration.
    *   Assume the MCP connection itself handles authentication securely based on the environment setup.
    *   Adhere to the principle of least privilege.
2.  **Parameter Validation (CRITICAL)**:
    *   **ALWAYS** validate that `server_name` and `tool_name` are accurate and correspond to available MCP integrations (refer to Roocode MCP configuration for available servers, e.g., `perplexityai`, `context7`).
    *   For `<use_mcp_tool>`, the `<arguments>` parameter **MUST** contain a valid **JSON string object**. The structure of this JSON is specific to each `<tool_name>`. Agents MUST consult relevant MCP server/tool documentation (or be provided with the schema by `sparc-orchestrator` or these guidelines) to construct this correctly. Even if a tool takes no arguments, an empty JSON object `{}` is typically required.
    *   For `<access_mcp_resource>`, the `<uri>` **MUST** be a valid URI for the specified server.
3.  **Error Handling**:
    *   Anticipate that MCP tool calls can fail (network issues, API errors, invalid parameters).
    *   Agents **MUST** implement robust error handling for MCP operations. This includes checking the output for error indicators and having contingency plans (e.g., retrying if appropriate for the MCP tool's idempotency, asking for clarification, reporting failure to `sparc-orchestrator`).
4.  **Data Management**:
    *   Validate and, if necessary, sanitize data received from MCP services.
    *   Be mindful of data sensitivity and PII when constructing arguments for or processing responses from MCP tools.
5.  **Idempotency**: Understand if an MCP tool operation is idempotent. If not, be cautious with retries.
6.  **Orchestration of MCP Calls**: `sparc-orchestrator` is responsible for ensuring the purpose and expected outcome of MCP tool calls are clear when delegating tasks that will involve them (e.g., tasking `research-analyst` to use `perplexityai` or `context7`).

## 2. Tool: `<use_mcp_tool>` (as per `use-mcp-tool.md`)

This is the primary tool for executing actions on an MCP server.

### 2.1. XML Structure:
### 2.1. XML Structure:
```xml
<use_mcp_tool>
  <server_name>target_mcp_server_name</server_name>
  <tool_name>specific_tool_on_that_server</tool_name>
  <arguments>{"param1": "value1", "param2": "value2"}</arguments> <!-- MUST be a valid JSON string -->
</use_mcp_tool>

2.2. Parameters:
<server_name> (Required): The name of the MCP server (e.g., perplexityai, context7).
<tool_name> (Required): The specific action/tool on the server.
<arguments> (Required/Optional based on tool): A JSON string object containing the tool's input parameters. If the tool takes no arguments, use an empty JSON object string: {}.
3. Tool: <access_mcp_resource> (as per access-mcp-resource.md)
Used to retrieve data from resources exposed by MCP servers. Less common for context7 which primarily uses tools, but could be used if context7 exposes direct resource URIs.
3.1. XML Structure:
<access_mcp_resource>
  <server_name>target_mcp_server_name</server_name>
  <uri>mcp_context7_server_uri_scheme://some_resource_identifier</uri>
</access_mcp_resource>

3.2. Parameters:
<server_name> (Required): The name of the MCP server.
<uri> (Required): The URI identifying the resource.
4. General MCP Integration Workflow (Agent Perspective)

Identify Need:
sparc-orchestrator or a specialist agent identifies the need for external research (perplexityai) or library documentation (context7).
Delegate/Prepare:
sparc-orchestrator delegates research/documentation tasks to research-analyst (or a relevant coding agent for simple doc lookups) with clear objectives.
The tasked agent formulates queries/arguments for the respective MCP.
Construct Arguments: The responsible agent carefully prepares the JSON arguments string.
Execute Call: Invoke <use_mcp_tool> or <access_mcp_resource>.
Process Output:
Check for errors...
Parse successful output...
Integrate Results/Report:
research-analyst (or other agent) synthesizes MCP output into a report or uses it directly for their task, then reports completion to sparc-orchestrator. The results (e.g., research summaries, key documentation snippets) are typically saved as files in the project memory bank.
5. Best Practices for MCP Integration
Retry Mechanisms: For transient network errors, implement a retry strategy if appropriate for the operation's idempotency. sparc-orchestrator may coordinate retries for critical context7 operations.
Logging of MCP Calls: sparc-orchestrator should (conceptually) log its interactions with context7 (e.g., "Storing artifact X for task Y in context7"). research-analyst logs its perplexityai usage. Avoid logging sensitive data from arguments or responses.
Timeout Handling: Be aware of configurable timeouts (default 60s, range 1-3600s).
Rate Limiting: Respect any rate limits imposed by external services.
Caching: For external MCPs like perplexityai or context7, if the same information is requested repeatedly, sparc-orchestrator or research-analyst might decide to save the results of an initial query to a file in .roo/memory-bank/research/ to avoid redundant external calls.
6. MCP Server-Specific Guidelines
6.1. perplexityai (External Research - Primarily for research-analyst delegated by sparc-orchestrator)
Tool Name Example: perplexityai_search_internet
Argument Focus:
query: Must be well-formulated by research-analyst to be specific and effective.
search_focus: Use "technical", "academic", etc., as appropriate.
Workflow: research-analyst receives objective from sparc-orchestrator, crafts queries, executes calls, synthesizes results, and reports back to sparc-orchestrator with a summary and path to the detailed research document (stored by research-analyst using <write_to_file>).
<!-- Example call by research-analyst -->
<use_mcp_tool>
  <server_name>perplexityai</server_name>
  <tool_name>perplexityai_search_internet</tool_name>
  <arguments>{"query": "Latest advancements in quantum-resistant cryptography algorithms", "search_focus": "academic", "max_results": 5}</arguments>
</use_mcp_tool>

6.2. context7 (Library Documentation Retrieval)
The context7 MCP server provides tools for discovering and retrieving documentation about various software libraries and technologies. It is used by agents (typically research-analyst, or code-architect/frontend-architect for specific API lookups via sparc-orchestrator) to get up-to-date technical information.
Available Tools (Based on your provided Roocode configuration and tool documentation):
tool_name: "resolve-library-id"
Description: Resolves a package/product name to a Context7-compatible library ID and returns a list of matching libraries. You MUST call this function before 'get-library-docs' to obtain a valid Context7-compatible library ID UNLESS the user explicitly provides a library ID in the format '/org/project' or '/org/project/version' in their query.
Selection Process Guidance for Agent:
Analyze the query to understand what library/package the user is looking for.
Return the most relevant match based on: Name similarity, description relevance, documentation coverage (Code Snippet counts), and trust score (7-10 prioritized).
If multiple good matches, proceed with the most relevant. If no good matches, state this and suggest query refinements. For ambiguous queries, request clarification.
Parameters (as per tool documentation):
libraryName (string, required): Library name to search for.
Arguments Example (JSON string):
{
  "libraryName": "name_of_library_to_search_for"
}
Use code with caution.
Json
Usage Example by an agent (e.g., research-analyst):
<use_mcp_tool>
  <server_name>context7</server_name>
  <tool_name>resolve-library-id</tool_name>
  <arguments>{"libraryName": "express.js"}</arguments>
</use_mcp_tool>
Use code with caution.
Xml
tool_name: "get-library-docs"
Description: Fetches up-to-date documentation for a library. You must call 'resolve-library-id' first to obtain the exact Context7-compatible library ID required to use this tool, UNLESS the user explicitly provides a library ID in the format '/org/project' or '/org/project/version' in their query.
Parameters (as per tool documentation):
context7CompatibleLibraryID (string, required): Exact Context7-compatible library ID (e.g., '/mongodb/docs', '/vercel/next.js').
topic (string, optional): Topic to focus documentation on (e.g., 'hooks', 'routing').
tokens (integer, optional): Maximum number of tokens of documentation to retrieve (default: 10000).
Arguments Example (JSON string):
{
  "context7CompatibleLibraryID": "/organization/project_id_from_resolve_library_id",
  "topic": "authentication",
  "tokens": 5000
}
Use code with caution.
Json
Usage Example by an agent (after getting an ID from resolve-library-id):
<use_mcp_tool>
  <server_name>context7</server_name>
  <tool_name>get-library-docs</tool_name>
  <arguments>{"context7CompatibleLibraryID": "/context7/zod-v4", "topic": "schemas", "tokens": 3000}</arguments>
</use_mcp_tool>
Use code with caution.
Xml
Workflow for using context7 MCP:
Agent (e.g., research-analyst, or code-architect needing specific API info) determines the need for documentation on a library (e.g., "Zod").
Agent calls resolve-library-id with {"libraryName": "Zod"} on server context7.
Agent analyzes the response, selects the most appropriate Context7-compatible library ID (e.g., /context7/zod-v4), and notes the selection rationale.
Agent calls get-library-docs with the obtained ID and optional topic and tokens parameters on server context7.
Agent processes the returned documentation snippets for its research or coding task. The relevant information extracted is then used or incorporated into artifacts saved in the project's file-based memory bank.
6.3. Other MCP Servers (General)
Consult Documentation: The primary source of truth for any other MCP server's tools and arguments is its specific documentation or Roocode's MCP configuration.
Verify Capabilities: Before attempting complex operations, verify the server and tool support it.
By following these guidelines, agents, primarily orchestrated by sparc-orchestrator, can effectively and securely leverage MCP tools, using perplexityai for external research and context7 as the central nervous system for project memory and context.