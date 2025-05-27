# File Operations Guidelines

This document outlines the correct usage and best practices for file operation tools: `<read_file>`, `<write_to_file>`, and `<list_files>`. Adherence to these guidelines is essential for reliable file manipulation.

## 1. General Principles for All File Operations

*   **Path Specificity**:
    *   All `<path>` arguments **MUST** be relative to the workspace root.
    *   Do **NOT** use absolute paths.
    *   Ensure paths are accurate and correctly specify the target file or directory.
*   **File Existence**:
    *   Before attempting to read or write, if there's any doubt, verify file/directory existence using `<list_files>` or by checking context provided by the orchestrator.
*   **Permissions**:
    *   Assume standard read/write permissions within the workspace. Issues related to permissions are typically outside agent control and should be flagged if suspected.
*   **Encoding**:
    *   Assume UTF-8 encoding for all text files unless explicitly specified otherwise by project requirements.

## 2. `<read_file>`

Reads the content of a specified file.

### 2.1. XML Structure:

```xml
<read_file>
  <path>path/to/your/file.ext</path>
  <!-- Optional parameters for reading specific sections of a file: -->
  <!-- <start_line>1</start_line> -->
  <!-- <end_line>100</end_line> -->
</read_file>

2.2. Parameters:
<path> (Required): The path to the file to be read, relative to the workspace root.
<start_line> (Optional): The 1-based line number from which to start reading. If omitted, reading starts from the beginning of the file.
<end_line> (Optional): The 1-based line number at which to stop reading (inclusive). If omitted, reading continues to the end of the file.
If start_line is provided, end_line can be used to read a specific range.
If end_line is greater than the total number of lines in the file, it will read up to the end of the file.
2.3. Common Errors to Avoid:
Non-Existent File: Attempting to read a file that does not exist at the specified path.
Incorrect Path: Providing a malformed path or a path to a directory instead of a file.
Missing <path> Parameter: The <path> parameter is mandatory.
Invalid start_line/end_line: Using non-numeric values, or start_line greater than end_line.
2.4. Best Practices:
Verification for Editing: CRITICAL: ALWAYS use <read_file> to fetch the current content of a file section before attempting to modify it with <apply_diff> or <search_and_replace>. This ensures your SEARCH blocks are accurate.
Large Files: For very large files where only a portion is needed, use start_line and end_line to limit the content returned, improving efficiency and reducing token usage.
Contextual Understanding: Use <read_file> to understand the structure and content of existing files relevant to the current task.
3. <write_to_file>
Writes content to a specified file. If the file exists, it will be overwritten. If it does not exist, it will be created.
3.1. XML Structure:
<write_to_file>
  <path>path/to/your/file.ext</path>
  <content>
    Line 1 of the content.
    Line 2 of the content.
    ...
    Last line of the content.
  </content>
  <line_count>N</line_count> <!-- Where N is the total number of lines in the <content> block -->
</write_to_file>

3.2. Parameters:
<path> (Required): The path to the file to be written, relative to the workspace root.
<content> (Required): The full content to be written to the file. This will replace any existing content.
<line_count> (Required): An integer representing the total number of lines in the provided <content>. This is a crucial parameter for validation by the tool environment.
3.3. Common Errors to Avoid:
Missing <path>, <content>, or <line_count>: All three parameters are mandatory.
Incorrect line_count: The line_count MUST accurately reflect the number of lines in the <content> block. Mismatches can lead to errors or tool rejection.
Path to a Directory: Attempting to write to a path that is an existing directory.
Unintended Overwrite: Accidentally overwriting a file that should have been modified (e.g., using <apply_diff> instead).
3.4. Best Practices:
New Files: Primarily use <write_to_file> for creating new files.
Complete Overwrite: Use when the intention is to completely replace the content of an existing file.
Caution with Existing Files: If a file might exist and you don't intend to overwrite it completely, consider using <apply_diff> or <insert_content> first. Use <list_files> to check for existence if unsure.
Accurate line_count: Programmatically determine or carefully count the lines in your <content> block. Empty lines count. A final newline character might also count depending on the system's line counting convention; generally, count the number of explicit newline characters + 1.
Small Configuration Files: Suitable for generating small configuration files or simple scripts from scratch.
4. <list_files>
Lists files and directories within a specified path.
4.1. XML Structure:
<list_files>
  <path>path/to/your/directory_or_file</path>
  <!-- Optional parameter: -->
  <!-- <recursive>false</recursive> --> <!-- Defaults to false -->
</list_files>

4.2. Parameters:
<path> (Required): The path to the directory to list, or a path to a specific file to check its existence and details. Relative to the workspace root.
<recursive> (Optional): A boolean (true or false).
If true, lists files and directories recursively through subdirectories.
If false (the default), lists only the contents of the specified directory (no recursion).
4.3. Output:
The tool will output a list of files and directories, typically including their names, types (file/directory), and potentially other metadata like size and modification times. The exact format may vary but will be parseable.
4.4. Common Errors to Avoid:
Non-Existent Path: Attempting to list a path that does not exist.
Missing <path> Parameter: The <path> parameter is mandatory.
4.5. Best Practices:
Existence Checks: Use <list_files> with a specific file path to confirm if a file exists before attempting to read or write it.
Directory Exploration: Use to understand the project structure or to find specific files within a directory.
Recursive Listing: Use <recursive>true</recursive> cautiously with large directory trees, as it can produce a lot of output. Be specific with your <path> if possible.
Planning File Operations: Use the output of <list_files> to plan subsequent file operations, ensuring paths are correct and target files/directories are as expected.