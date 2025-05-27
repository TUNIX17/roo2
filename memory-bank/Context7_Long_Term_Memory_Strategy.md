# Context7 MCP Long-Term Memory Strategy

## 1. Introduction to Context7 MCP

Context7 MCP is integrated with the Apoderapp project to serve as a component of the long-term memory system, utilizing SQLite as its database. Its primary role, as currently configured, is to facilitate the retrieval of documented context, such as library documentation.

## 2. Current Capabilities (as per .roo/mcp.json)

The Context7 MCP server is configured and enabled, connecting to `sqlite:chats.db`. The currently exposed tools are:
*   `resolve-library-id`: Used to resolve identifiers for documented libraries.
*   `get-library-docs`: Used to retrieve documentation for specific libraries based on their identifiers.

These tools are suitable for managing and accessing structured documentation related to project libraries and components.

## 3. Strategy for Long-Term Memory (Current Scope)

Within its current capabilities, Context7 MCP will be used for:
*   **Storing and Retrieving Library-Specific Context**: Documenting and retrieving information about third-party libraries, internal shared components, and their usage patterns. This includes API schemas, common usage examples, and design patterns associated with specific code libraries.
*   **Referencing Setup Documentation**: The setup and configuration details for Context7 itself are maintained in `Docs/Setup/Context7_SQLite_Setup.md`.

## 4. Limitations and Future Enhancements for Broader Long-Term Memory

The current toolset (`resolve-library-id`, `get-library-docs`) does not directly support the broader requirements of a living context document, tracking agent actions, validating outputs, or creating a project knowledge graph for arbitrary decisions and resolved issues as outlined in the `context-keeper` role.

To achieve a more comprehensive long-term memory system, future enhancements to Context7 MCP or integration with additional tools would be required. Potential enhancements include:
*   **Adding Write/Update Capabilities**: New MCP tools would be needed to allow agents to store and update arbitrary key-value pairs, structured JSON, or graph data representing project decisions, design patterns, resolved issues, and architectural constraints.
*   **Querying and Graph Traversal**: Tools for querying the stored context based on tags, relationships, or full-text search would enhance retrieval capabilities.
*   **Integration with Agent Workflows**: Develop explicit workflows where agents (e.g., `architect-enhanced`, `code-architect`, `debugging-specialist`) are instructed to log specific types of decisions or findings into Context7 via new dedicated tools.

## 5. Guidelines for Saving and Retrieving Context (Future State with Enhanced Capabilities)

If Context7 MCP is enhanced with broader write/read capabilities, the following guidelines would apply:

### 5.1. Saving Important Context
*   **Project Decisions**: All significant architectural, design, and implementation decisions (e.g., technology choices, major refactorings, API design changes) should be logged with rationale, alternatives considered, and the date of decision.
*   **Configurations**: Key configuration parameters, environment variables, and their purpose should be stored, especially for non-sensitive values or as references to secret management systems.
*   **Resolved Issues/Bugs**: When a bug is resolved, its root cause, the solution implemented, and any relevant context (e.g., affected components, data implications) should be logged.
*   **Patterns and Anti-Patterns**: Document identified design patterns, coding conventions, and common anti-patterns to avoid, along with examples.
*   **Schemas and APIs**: Store up-to-date API specifications, data models, and database schemas.

### 5.2. Retrieving Important Context
*   Agents should query Context7 MCP before starting tasks that might be affected by prior decisions or require specific configurations.
*   Context retrieval should be a proactive step in the planning phase of any significant development or debugging task.
*   The `context-keeper` will provide relevant context summaries to other agents by querying Context7 MCP and other memory bank files.

## 6. Conclusion

Context7 MCP currently provides a foundation for managing documented context. Expanding its toolset will be crucial to fully realize its potential as the central long-term memory hub for the SPARC system, enabling comprehensive tracking and retrieval of project knowledge.