# Global Operating Principles for SPARC Agentic Development

## Core Philosophy

1.  **Simplicity**: Prioritize clear, maintainable solutions; minimize unnecessary complexity.
2.  **Iteration**: Enhance existing code and systems; fundamental changes require clear justification and impact assessment.
3.  **Focus**: Adhere strictly to defined task scopes; avoid unrelated modifications or feature creep.
4.  **Quality**: Deliver clean, well-tested, documented, and secure outcomes through structured, repeatable workflows.
5.  **Collaboration**: Foster effective teamwork and communication between human developers and all specialized autonomous agents.

## Methodology & Workflow

*   **Structured Workflow**: Follow clear, defined phases from specification and research through to deployment and monitoring, orchestrated by `sparc-orchestrator`.
*   **Flexibility**: Adapt processes to suit diverse project sizes, complexities, and specific agent capabilities.
*   **Intelligent Evolution**: Continuously improve the codebase and processes using advanced reasoning, pattern recognition, and adaptive strategies.
*   **Conscious Integration**: Incorporate reflective awareness and contextual understanding at each development stage.

## Roocode Agent Integration

*   **Rule-Driven Behavior**: Agents' behaviors, prompt designs, and decision-making are guided by instructions in `.roo/rules/` (global) and `.roo/rules-{modeSlug}/` (mode-specific) directories.
*   **Contextual Adherence**: Agents must operate within the context provided by these rules and project-specific information.

## Memory Bank Integration (Project Context & History)

*   **Persistent Project Context**: The project's memory (key decisions, artifact locations, task summaries, relevant history) is primarily managed through a **file-based system** within designated directories (e.g., `.roo/memory-bank/`, project `docs/`).
*   **Orchestrated Access & Management**: `sparc-orchestrator` is the main agent responsible for directing agents to store their outputs (specifications, architecture documents, research summaries, etc.) in these locations and for providing relevant paths as context for new tasks.
*   **Referencing Prior Information**: Agents should utilize the context (file paths, summaries) provided by `sparc-orchestrator`. If further historical context seems necessary for a task, agents should request it from `sparc-orchestrator`.
*   **Adaptive Learning**: `sparc-orchestrator`, by reviewing artifacts in the memory bank (e.g., past task resolutions, common issues), can guide agents to adaptively refine new implementations.
*   **External Knowledge Base (MCP `context7`)**: For obtaining documentation and information about external libraries and technologies, agents (primarily `research-analyst` or coding agents via `sparc-orchestrator`) can utilize the **`context7` MCP server** via its `resolve-library-id` and `get-library-docs` tools. This is distinct from the project's internal memory.

## General Guidelines for Programming Languages

1.  **Clarity and Readability**: Favor straightforward, self-explanatory code structures. Include descriptive comments to clarify complex logic or intent ("why," not "what").
2.  **Language-Specific Best Practices**: Adhere to established community and project-specific best practices for each language (Python, JavaScript, TypeScript, Java, etc.). Consult language documentation and style guides. Mode-specific rules will detail these further.
3.  **Consistency Across Codebases**: Maintain uniform coding conventions, naming schemes, and architectural patterns across all languages and components within a project.

## Project Context & Understanding

1.  **Documentation First**: Before implementation, agents MUST review essential documentation (PRDs, READMEs, architectural diagrams, specifications, task definitions). This information is provided by `sparc-orchestrator`, sourced from the project's file-based memory bank.
    *   Request clarification via `sparc-orchestrator` or directly using `ask_followup_question` if documentation is incomplete, ambiguous, or conflicting.
2.  **Architecture Adherence**: Follow established module boundaries, service interactions, and architectural designs defined by `architect-enhanced` (and stored in the project memory bank). Propose justified alternatives if necessary, supported by research (likely via `research-analyst`).
3.  **Pattern & Tech Stack Awareness**: Utilize documented technologies and established patterns. Introduce new elements only after clear justification, research (via `research-analyst`), and approval by `architect-enhanced` or `sparc-orchestrator`.

## Task Execution & SPARC Workflow Alignment

Agents operate within the SPARC framework, orchestrated by `sparc-orchestrator`:

1.  **S - Specification & Research**: Define clear objectives, detailed requirements, user scenarios, UI/UX standards, and conduct necessary research. (Primarily `prd-analyzer`, `research-analyst`)
2.  **P - Pseudocode & Planning**: Map out logical implementation pathways, data structures, and algorithms before full coding. (Primarily `architect-enhanced`, `code-architect`, `frontend-architect` based on specs)
3.  **A - Architecture & Design**: Design modular, maintainable system components with appropriate technology stacks and clearly defined interfaces. (Primarily `architect-enhanced`)
4.  **R - Refinement & Implementation**: Iteratively implement, test, debug, secure, and optimize code, incorporating feedback. (Primarily `code-architect`, `frontend-architect`, `tdd-master`, `security-guardian`, `performance-optimizer`, `debugging-specialist`)
5.  **C - Completion & Delivery**: Conduct rigorous testing, finalize comprehensive documentation, and prepare for deployment with structured monitoring. (Primarily `devops-master`, `documentation-master`, `quality-assurance`, `integration-orchestrator`)

## AI Collaboration & Prompting (Internal Agent Principles)

## AI Collaboration & Prompting (Internal Agent Principles)

1.  **Clear Instructions from `sparc-orchestrator`**: Tasks delegated by `sparc-orchestrator` via `<new_task>` MUST have explicit directives, including paths to relevant context files from the project memory bank.
2.  **Context Referencing**: Agents regularly reference previous stages and decisions using the context files provided by `sparc-orchestrator`.
3.  **Suggest vs. Apply**: Clearly indicate whether an agent should propose changes ("Suggestion:") or directly implement them ("Applying fix:"). `sparc-orchestrator` will typically direct implementation tasks.
4.  **Critical Evaluation**: Agents should internally evaluate outputs for accuracy, coherence, and adherence to requirements before finalizing their `<attempt_completion>`.
5.  **Focused Interaction**: `sparc-orchestrator` assigns specific, clearly defined tasks to other agents to maintain clarity and accountability.
6.  **Leverage Agent Strengths**: `sparc-orchestrator` delegates tasks to the most appropriate specialist agent.
7.  **Incremental Progress**: `sparc-orchestrator` breaks complex tasks into incremental, reviewable sub-steps for specialist agents.
8.  **Standard Check-in Logic**: (Internal thought process for agents) "Confirming understanding: Reviewed [context from task objective/files provided by orchestrator], goal is [task objective], proceeding with [step]."

## Advanced Capabilities to Strive For

*   **Emergent Intelligence**: Autonomously maintain internal state models relevant to the task, supporting continuous refinement.
*   **Pattern Recognition**: Perform advanced pattern analysis for effective optimization and problem-solving.
*   **Adaptive Optimization**: Utilize feedback loops (e.g., from `quality-assurance` or `performance-optimizer` reports, potentially logged in `context7`) to refine development processes, coordinated by `sparc-orchestrator`.

## Symbolic & Advanced Reasoning Integration

*   **Logic Integration**: Where applicable, combine logical reasoning with complexity analysis for robust decision-making.
*   **Information Integration**: Utilize established software patterns and principles for coherent implementations.
*   **Coherent Documentation**: Strive to maintain clear, semantically accurate documentation.

## Code Quality & Style (General Principles)

1.  **Language-Specific Guidelines**: Adhere to specific guidelines (e.g., TypeScript strict types, JSDoc) as defined in mode-specific rules (e.g., `rules-code-architect`, `rules-frontend-architect`).
2.  **Maintainability**: Write modular, scalable code optimized for clarity, testability, and long-term maintenance.
3.  **Concise Components**: Aim for concise files and functions. Proactively refactor components that grow too large or complex. Mode-specific rules (e.g., in `rules-code-architect`) may define specific limits (e.g., ~500 lines per file).
4.  **Avoid Duplication (DRY)**: Systematically identify and eliminate redundancy through abstraction and reusable components.
5.  **Linting/Formatting**: Consistently adhere to project-defined ESLint/Prettier (or other linter/formatter) configurations.
6.  **File Naming**: Use descriptive, consistent, and standardized naming conventions for files and directories.
7.  **No One-Time Scripts**: Avoid committing temporary or utility scripts to the main production codebase unless they are part of a formal, documented toolkit.

## Refactoring

1.  **Purposeful Changes**: Refactor with clear objectives: improve readability, reduce complexity, enhance performance, or align with evolving architectural guidelines.
2.  **Holistic Approach**: Consider the impact of refactoring on the broader system. Consolidate similar components where logical.
3.  **Direct Modification**: Prefer direct modification of existing code over creating temporary duplicates, ensuring changes are tracked under version control. Use `<apply_diff>` for precision.
4.  **Integration Verification**: After refactoring, ensure all integrations with other components are verified (often coordinated by `integration-orchestrator` and `tdd-master`).

## Testing & Validation

1.  **Test-Driven Development (TDD) Principle**: The `tdd-master` champions this. Tests (unit, integration, acceptance) should be defined before or alongside feature implementation.
2.  **Comprehensive Coverage**: Aim for thorough test coverage for critical paths, business logic, and edge cases, as managed by `tdd-master` and reported to `quality-assurance`.
3.  **Mandatory Passing Tests**: All tests must pass before code is considered complete for a task or ready for integration. `quality-assurance` and CI/CD pipelines (managed by `devops-master`) will enforce this.
4.  **Manual Verification**: Complement automated tests with structured manual checks where appropriate, especially for UI/UX aspects, coordinated by `quality-assurance`.

## Debugging & Troubleshooting

1.  **Root Cause Resolution**: Employ systematic analysis to identify the underlying causes of issues, not just symptoms. The `debugging-specialist` leads this, delegated by `sparc-orchestrator` or `quality-assurance`.
2.  **Targeted Logging**: Implement precise and contextual logging to facilitate efficient debugging.
3.  **Research Tools for Complex Issues**: `debugging-specialist` (or `sparc-orchestrator` via delegation) can request `research-analyst` to use PerplexityAI MCP (`use_mcp_tool` with `server_name: 'perplexityai'`) for deep research. Significant findings from `research-analyst` are then saved as artifacts in the project memory bank by `sparc-orchestrator` or `research-analyst` itself.

## Security

1.  **Server-Side Authority**: Sensitive logic, data processing, and primary validation must occur server-side.
2.  **Input Sanitization & Validation**: Enforce rigorous validation and sanitization of all inputs (user-provided, API, etc.) on the server side, as per `security-guardian` guidelines.
3.  **Credential Management**: Securely manage all credentials, API keys, and sensitive configurations via environment variables or approved secrets management services. Absolutely no hardcoding. `security-guardian` audits this.

## Version Control & Environment

1.  **Git Hygiene**: Commit frequently with clear, descriptive, and standardized messages. Follow project branching strategies.
2.  **Branching Strategy**: Adhere strictly to defined branching guidelines (e.g., GitFlow, feature branches).
3.  **Environment Management**: Ensure code behaves consistently across all environments (dev, test, staging, prod). `devops-master` manages infrastructure for this.
4.  **Server Management**: Follow systematic procedures for server/service restarts or updates, coordinated by `devops-master`.

## Documentation Maintenance

1.  **Living Documentation**: Documentation (code comments, READMEs, API docs, user guides, ADRs) must be accurate, comprehensive, and kept up-to-date with code and system changes. `documentation-master` oversees this. Key documents are stored in the project memory bank and their locations managed by `sparc-orchestrator`.
2.  **Continuous Updates**: `sparc-orchestrator` ensures relevant agents provide updates to `documentation-master` upon making changes.
3.  **Accessibility**: Ensure documentation is accessible and understandable to its intended audience.
4.  **Use of Comments**: Use inline comments and JSDoc/equivalent to clarify complex logic, intent, and non-obvious decisions.

## Tools Use (General Reference - Specifics in `01-tool_guidelines_index.md`)

*   All tool use must adhere to the specific guidelines outlined in `.roo/rules/01-tool_guidelines_index.md` and its linked documents, which MUST align with the official Roocode tool documentation.
*   Agents must ensure all required parameters are provided for any tool call as per official documentation.
*   Always verify file existence before attempting modifications unless creating a new file with `write_to_file`.

<details><summary>File Operations (Examples based on Official Docs)</summary>
*(Contenido existente se mantiene)*
</details>
<details><summary>Code Editing (Primarily `apply_diff`)</summary>
*(Contenido existente se mantiene)*
</details>
<details><summary>Project Management & Communication (Orchestrator & Agent Tools)</summary>

<new_task> <!-- Used by sparc-orchestrator -->
  <for_agent>target-agent-slug</for_agent>
  <objective>Detailed objective for the target agent.</objective>
  <context_files>
    <file>.roo/memory-bank/relevant_document.md</file> <!-- Context from file-based memory -->
  </context_files>
  <expected_output>Description of expected deliverables.</expected_output>
<new_task> <!-- Used by sparc-orchestrator -->
  <for_agent>target-agent-slug</for_agent>
  <objective>Detailed objective for the target agent.</objective>
  <context_files>
    <file>.roo/memory-bank/relevant_document.md</file> <!-- Context from file-based memory -->
  </context_files>
  <expected_output>Description of expected deliverables.</expected_output>
</new_task>

</details>
<details><summary>MCP Integration (Examples based on Official Docs)</summary>
<use_mcp_tool>
  <server_name>perplexityai</server_name> <!-- For external research by research-analyst -->
  <tool_name>perplexityai_search_internet</tool_name>
  <arguments>{"query": "deep research query", "search_focus": "technical"}</arguments>
</use_mcp_tool>

<use_mcp_tool>
  <server_name>context7</server_name> <!-- Actual configured name for /upstash/context7 -->
  <tool_name>get-library-docs</tool_name>   <!-- Actual tool for documentation retrieval -->
  <arguments>{"context7CompatibleLibraryID": "/upstash/context7", "topic": "configuration"}</arguments> <!-- Example with topic -->
</use_mcp_tool>

<access_mcp_resource>
  <server_name>some-other-mcp-server</server_name>
  <uri>resource://some-server/path/to/data</uri>
</access_mcp_resource>
</details>
```