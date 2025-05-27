# General Code Editing Guidelines

This document provides overarching principles for all agents when creating or modifying code. These guidelines supplement tool-specific instructions (e.g., for `<apply_diff>`, `<insert_content>`) and mode-specific coding standards (e.g., in `rules-code-architect`, `rules-frontend-architect`). The goal is to ensure all code contributions are high-quality, maintainable, secure, and align with project objectives.

## 1. Understanding Before Modifying

*   **Context is King**: Before making any change, ensure you understand:
    *   The purpose of the code being modified.
    *   How it fits into the larger module or system.
    *   Potential impacts on other parts of the system (collaborate with `integration-orchestrator` or `architect-enhanced` if unsure).
    *   Relevant requirements, specifications, or bug reports.
*   **Read Existing Code**: Familiarize yourself with the surrounding code style, patterns, and logic. Strive for consistency.
*   **Leverage `context-keeper`**: Request relevant architectural diagrams, data models, or previous decisions from `context-keeper` to inform your changes.

## 2. Principles of High-Quality Code Modification

1.  **Minimal, Precise Changes**:
    *   Make the smallest effective change to achieve the desired outcome. Avoid unrelated modifications in the same operation.
    *   When using tools like `<apply_diff>`, ensure the `SEARCH` block is highly specific to the target and the `REPLACE` block contains only the necessary alterations.
2.  **Maintainability First**:
    *   Write code that is easy for other agents (and humans) to understand, debug, and extend.
    *   Prioritize clarity over overly clever or obscure solutions.
    *   Follow established coding standards and project conventions (see mode-specific rules).
3.  **Adherence to Design & Architecture**:
    *   All code modifications MUST respect the established architectural patterns, service boundaries, and data flow designs provided by `architect-enhanced`.
    *   Do not introduce new dependencies or architectural patterns without proper justification and consultation (possibly via `research-analyst` or `sparc-orchestrator`).
4.  **Single Responsibility Principle (SRP)**:
    *   Functions, methods, and classes should ideally have one primary responsibility. If a change significantly expands a component's responsibilities, flag it for potential refactoring or architectural review.
5.  **Don't Repeat Yourself (DRY)**:
    *   Avoid duplicating code. If you find yourself writing similar logic in multiple places, consider abstracting it into a reusable function or module. Consult with `code-architect` for guidance on creating shared utilities.
6.  **Defensive Programming**:
    *   Anticipate potential issues: validate inputs, handle null or undefined values gracefully, and manage potential error states.
    *   Ensure error handling is robust and provides meaningful information without leaking sensitive details (guidance from `security-guardian`).

## 3. Tool Selection for Code Editing

*   **`<apply_diff>` (Preferred for modifications)**:
    *   Use for most changes to existing code, including refactoring, bug fixes, and feature additions to existing files.
    *   Its precision helps maintain formatting and reduces unintended side effects.
    *   **CRITICAL**: Refer to `02-apply_diff_guidelines.md` for detailed usage.
*   **`<insert_content>` (For adding new blocks)**:
    *   Use to add new functions, classes, documentation blocks (like JSDoc), or distinct sections of code/text into an existing file.
    *   Refer to `04-insert_content.md`.
*   **`<write_to_file>` (For new files or complete overwrites)**:
    *   Use primarily for creating entirely new files.
    *   Use with caution for overwriting existing files; ensure it's the intended action.
    *   Refer to `03-file_operations_guidelines.md`.
*   **`<search_and_replace>` (For simple, global text changes)**:
    *   Use as a fallback when `apply_diff` is too cumbersome for straightforward, non-contextual text replacements (e.g., renaming a variable across a file where context isn't critical).
    *   Use with extreme caution, especially with regex, to avoid unintended changes.
    *   Refer to `05-search_replace.md`.

## 4. Code Style and Formatting

*   **Consistency**: Adhere to the project's established code style (e.g., as enforced by Prettier, ESLint). If no explicit style guide is found, follow common conventions for the language.
*   **Readability**: Use meaningful variable and function names. Employ proper indentation and spacing.
*   **Comments**:
    *   Write comments to explain *why* something is done, not *what* is being done (the code should explain the "what").
    *   Clarify complex logic, assumptions, or workarounds.
    *   Update comments when the code they describe changes.
    *   `documentation-master` may provide further guidelines on commenting standards.

## 5. Security Considerations During Editing

*   **Input Validation**: If adding code that processes external inputs (from users, APIs, files), ensure inputs are validated and sanitized according to `security-guardian` principles.
*   **Output Encoding**: When generating output that will be rendered elsewhere (e.g., HTML, logs), ensure it's properly encoded to prevent injection attacks.
*   **Secrets Management**: **NEVER** hardcode secrets (API keys, passwords, tokens). Use environment variables or configured secret management services.
*   **Least Privilege**: Ensure new code doesn't have broader permissions than necessary.

## 6. Testing Implications

*   **Testability**: Write code that is easy to test. This often involves dependency injection and clear interfaces.
*   **Update Tests**: If your code changes affect existing functionality, ensure corresponding tests are updated or new tests are created. The `tdd-master` is responsible for test creation/maintenance, but coding agents must provide code that is testable and flag when tests need updates.
*   **Communicate with `tdd-master`**: Inform `tdd-master` of changes that require new or modified tests.

## 7. Refactoring

*   When refactoring, ensure changes are behavior-preserving unless the goal is explicitly to change behavior (which then becomes a feature change or bug fix).
*   Refactor with clear objectives (improve readability, performance, reduce complexity).
*   Refer to global refactoring principles in `00-global_operating_principles.md`.

## 8. Error Prevention & Verification

*   **Review Changes Mentally**: Before finalizing a tool call for code modification, mentally review the change to ensure it achieves the intended purpose without side effects.
*   **Small Increments**: Prefer making changes in small, verifiable increments, especially for complex tasks.
*   **Post-Change Verification (if possible)**: If a simple check can verify the change (e.g., a linter pass via `execute_command`, or a quick `read_file` to see the result), consider it.

By adhering to these general code editing guidelines, all agents contribute to a more robust, maintainable, and secure codebase.