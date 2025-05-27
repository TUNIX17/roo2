# File Operations Guidelines

This document outlines the correct usage and best practices for file operation tools: `<read_file>`, `<write_to_file>`, `<list_files>`, `<insert_content>`, and `<search_and_replace>`, aligning with their **official Roocode documentation**.

## 1. General Principles for All File Operations

*   **Path Specificity**:
    *   All `<path>` arguments **MUST** be relative to the workspace root.
    *   Do **NOT** use absolute paths.
    *   Ensure paths are accurate.
*   **File Existence (Except for `write_to_file` creating new files)**:
    *   Before reading or modifying (except creating anew with `write_to_file`), verify file/directory existence using `<list_files>` or context.
*   **Encoding**: Assume UTF-8 unless specified otherwise.

## 2. `<read_file>` (as per `read-file.md`)

Reads content of a specified file. Output includes line numbers.

### 2.1. XML Structure:
```xml
<read_file>
  <path>path/to/your/file.ext</path>
  <!-- Optional: Read specific line range (1-based, inclusive) -->
  <!-- <start_line>10</start_line> -->
  <!-- <end_line>20</end_line> -->
</read_file>

2.2. Parameters:
<path> (Required): Relative path to the file.
<start_line> (Optional): 1-based start line.
<end_line> (Optional): 1-based end line (inclusive).
2.3. Key Features from Docs:
Auto-truncates large text files if no range specified, showing the start and optionally method summaries.
Can extract text from PDF and DOCX.
2.4. Best Practices:
CRITICAL for Editing: ALWAYS use <read_file> to get current content before using <apply_diff> or modifying with <search_and_replace>.
Use line ranges for large files to improve efficiency.
3. <write_to_file> (as per write-to-file.md)
Creates new files or completely replaces existing file content. Requires interactive approval.
3.1. XML Structure:
<write_to_file>
  <path>path/to/your/file.ext</path>
  <content>
    Full content to write.
    This will create a new file or completely overwrite an existing one.
  </content>
  <line_count>N</line_count> <!-- MANDATORY: Accurate total number of lines in <content> -->
</write_to_file>

3.2. Parameters:
<path> (Required): Relative path to the file.
<content> (Required): Full content to write.
<line_count> (Required): Integer, total lines in <content>. Crucial for validation and preventing truncation.
3.3. Key Features & Limitations from Docs:
Shows changes in diff view for approval; user can edit in diff.
NOT for modifying existing files (use <apply_diff>). Slower for large files.
3.4. Best Practices:
Primarily for creating new files or complete, reviewed overwrites.
Ensure line_count is absolutely accurate.
If file might exist and modification is needed, strongly prefer <apply_diff> or <insert_content>.
4. <list_files> (as per list-files.md)
Lists files and directories.
4.1. XML Structure:
<list_files>
  <path>path/to/your/directory_or_file</path>
  <!-- Optional: <recursive>true</recursive> (default: false) -->
</list_files>

4.2. Parameters:
<path> (Required): Relative path.
<recursive> (Optional): true for recursive, false (default) for top-level.
4.3. Key Features from Docs:
Ignores common large dirs (node_modules, .git) in recursive mode. Respects .gitignore.
Marks .rooignore files (if showRooIgnoredFiles enabled).
Capped at ~200 files by default.
4.4. Best Practices:
Use to verify file/directory existence before other operations.
Use recursive="true" cautiously; be specific with <path>.
5. <insert_content> (as per insert-content.md)
Adds new lines into an existing file at a specific location without modifying original content. Requires interactive approval.
5.1. XML Structure:
<insert_content>
  <path>path/to/existing/file.ext</path>
  <line>10</line> <!-- 1-based line number BEFORE which to insert. Use 0 to append. -->
  <content>
    New content to insert.
    Can span multiple lines.
    Existing content from line 10 onwards will be shifted down.
  </content>
</insert_content>

5.2. Parameters:
<path> (Required): Relative path to an existing file.
<line> (Required): 1-based line number before which <content> is inserted. Use 0 to append to the end of the file.
<content> (Required): Text content to insert.
5.3. Key Features & Limitations from Docs:
Targeted insertion, preserves existing content. Shows diff view for approval.
Cannot replace/delete (use <apply_diff> or <search_and_replace>). Target file must exist.
5.4. Best Practices:
Verify file existence first with <list_files>.
Use for adding new import statements, functions, config blocks, log entries.
For precise end-of-file appends, line="0" is the direct method.
If needing to insert at multiple locations, consider if sequential calls are clearer than complex line number calculations.
6. <search_and_replace> (as per search-and-replace.md)
Finds and replaces text within a file, supporting literal strings and regex. Requires interactive approval.
6.1. XML Structure:
<search_and_replace>
  <path>path/to/existing/file.ext</path>
  <search>text_or_regex_pattern_to_find</search>
  <replace>text_to_replace_matches_with</replace>
  <!-- Optional parameters: -->
  <!-- <start_line>1</start_line> -->              <!-- 1-based start of search scope -->
  <!-- <end_line>50</end_line> -->                <!-- 1-based end of search scope (inclusive) -->
  <!-- <use_regex>true</use_regex> -->          <!-- Default: false -->
  <!-- <ignore_case>true</ignore_case> -->      <!-- Default: false -->
</search_and_replace>

6.2. Parameters:
<path> (Required): Relative path to an existing file.
<search> (Required): Text string or regex pattern.
<replace> (Required): Replacement text.
<start_line> (Optional): 1-based line number to begin search scope.
<end_line> (Optional): 1-based line number to end search scope (inclusive).
<use_regex> (Optional, String "true"/"false"): Default false. If "true", <search> is regex.
<ignore_case> (Optional, String "true"/"false"): Default false. Case-insensitive search.
6.3. Key Features & Limitations from Docs:
Flexible searching, case control, scoped replacements. Shows diff view for approval.
Single file only. Review overhead. Regex complexity.
6.4. Best Practices:
Verify file existence.
Prefer <apply_diff> for complex or highly contextual changes. <search_and_replace> is for simpler, broader replacements.
Make search terms specific. Test regex patterns carefully.
Use <start_line> and <end_line> to narrow scope and improve precision/safety.
---
**Archivo: `/rules/04-insert_content.md` (Actualizado para reflejar parámetros directos de `insert-content.md`)**
---
```markdown
# `insert_content`: Guidelines for Precise Content Insertion

The `<insert_content>` tool is used to add new lines of content into an existing file at a specific line number without modifying the original content. It shifts subsequent lines down. All changes are presented in a diff view for user approval. This guide aligns with the **official `insert-content.md` documentation**.

## 1. XML Structure (as per `insert-content.md`)

```xml
<insert_content>
  <path>path/to/your/existing_file.ext</path>
  <line>10</line> <!-- 1-based line number BEFORE which the content should be inserted.
                     Use line="0" to append the content to the end of the file. -->
  <content>
    This is the new content to be inserted.
    It can span multiple lines.
    The original content from line 10 (if it existed) will be shifted down.
  </content>
</insert_content>

2. Parameters (as per insert-content.md)
<path> (Required): The relative path (from the workspace root) of the file to insert content into. The file MUST exist.
<line> (Required, Integer): The 1-based line number before which the <content> should be inserted.
Example: If line="10", the new content will begin on what becomes the new line 10, and the original line 10 (and subsequent lines) will be pushed down.
If line="1", the content is inserted at the very beginning of the file.
If line="0", the content is appended to the end of the file.
<content> (Required, String): The text content to insert. Newline characters (\n) within this string will result in multi-line insertions.
3. Key Features & Limitations (from insert-content.md)
Targeted Insertion: Adds content precisely.
Preserves Existing Content: Does not modify or delete original file lines.
Interactive Approval: Shows proposed insertions in a diff view, requiring explicit user approval. User can edit in diff.
Insert Only: Cannot replace or delete existing content. Use <apply_diff> or <search_and_replace> for modifications.
Requires Existing File: The target file specified by <path> must exist. <insert_content> cannot create new files.
4. Common Errors to Avoid
Non-Existent Target File: Attempting to use <insert_content> on a file that does not exist.
Prevention: Use <list_files> to verify file existence if unsure. Use <write_to_file> to create new files.
Missing Required Parameters: <path>, <line>, or <content> are all mandatory.
Invalid <line> Value:
Using non-integer values.
Using a <line> number greater than (current total lines in file + 1) when not intending to append with line="0". For example, if a file has 5 lines, valid insertion points are 1 through 6 (where 6 means inserting after the last line, effectively the same as line="0" or appending). A line="7" would be an error if not using line="0".
Prevention: Use <read_file> to get the file's current content (and thus line count) if precise end-of-file insertion or validation is needed. line="0" is the explicit way to append.
Trying to Modify Content: This tool only inserts. For modifications, use <apply_diff>.
5. Best Practices
Verify File Existence: ALWAYS ensure the target file exists before calling <insert_content>.
<list_files>
  <path>path/to/your/file.ext</path>
</list_files>
<!-- (Agent then processes output to confirm existence before proceeding) -->

Confirm File Content (Optional but Recommended): For critical insertions, use <read_file> first to understand current content and line numbering. This helps in accurately determining the <line> for insertion.
Adding New Content, Not Modifying: <insert_content> is best for adding entirely new blocks of text, code, comments, or documentation.
Inserting at the Beginning of a File: Use <line>1</line>.
Appending to the End of a File: Use <line>0</line>. This is the clearest way to append.
Use for Documentation and Code Comments: Excellent for programmatically adding JSDoc blocks, new sections to Markdown files, or explanatory comments.
Multiple Insertions: If multiple, distinct blocks need to be inserted into the same file, it's generally safer and more predictable to use sequential <insert_content> calls. Be mindful that each insertion shifts subsequent line numbers, so calculate <line> values for later insertions carefully or insert from bottom to top if absolute line numbers are critical.