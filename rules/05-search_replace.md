# `search_and_replace`: Guidelines for Text Replacement

The `<search_and_replace>` tool finds and replaces text within a file, supporting both literal strings and regular expression patterns. It allows for targeted replacements across multiple locations, optionally within specific line ranges. All changes are presented in a diff view for user review and approval. This guide aligns with the **official `search-and-replace.md` documentation**.

## 1. XML Structure (as per `search-and-replace.md`)

```xml
<search_and_replace>
  <path>path/to/your/existing_file.ext</path>
  <search>text_or_regex_pattern_to_find</search>
  <replace>text_to_replace_matches_with</replace>
  <!-- Optional parameters: -->
  <start_line>1</start_line>              <!-- 1-based start of search scope -->
  <end_line>50</end_line>                <!-- 1-based end of search scope (inclusive) -->
  <use_regex>true</use_regex>          <!-- String "true" or "false". Default: "false" -->
  <ignore_case>true</ignore_case>      <!-- String "true" or "false". Default: "false" -->
</search_and_replace>

2. Parameters (as per search-and-replace.md)
Required Parameters
<path>: The relative path (from the workspace root) of the file to modify. The file MUST exist.
<search>: The text string or regex pattern to find.
<replace>: The text to replace matches with. Can be an empty string to delete matches.
Optional Parameters
<start_line>: The 1-based line number where the search scope begins.
<end_line>: The 1-based line number where the search scope ends (inclusive).
<use_regex> (String): Set to "true" to treat the <search> parameter as a regular expression pattern (default is "false").
<ignore_case> (String): Set to "true" to perform a case-insensitive search (default is "false").
3. Key Features & Limitations (from search-and-replace.md)
Flexible Searching: Supports literal text and regex.
Case Sensitivity Control.
Scoped Replacements: Can limit replacements to specific line ranges.
Interactive Approval: Shows diff view for approval; user can edit in diff.
Single File Operation: Operates on only one file at a time.
Review Overhead: Mandatory diff view adds an interactive step.
Regex Complexity: Complex regex patterns might require careful construction.
4. Common Errors to Avoid
Non-Existent Target File: Attempting to use on a file that does not exist.
Prevention: Use <list_files> to verify file existence.
Missing Required Parameters: <path>, <search>, or <replace> are mandatory.
Malformed Regular Expression: If <use_regex>true</use_regex>, providing a syntactically incorrect regex pattern in <search>.
Prevention: Test regex patterns externally. Be mindful of JSON/XML string escaping vs. regex escaping.
Unintended Global Replacements: A broad search term can replace more than intended.
Prevention: Make search terms specific. Use <read_file> to inspect content. Consider <apply_diff> for more contextual control.
Regex Escaping Issues:
When <use_regex>true</use_regex>, special regex characters (e.g., ., *, (, )) in the <search> string must be escaped with a backslash (\) if meant to be matched literally (e.g., \. to match a literal dot).
Boolean Parameter Formatting: Ensure <use_regex> and <ignore_case> are string "true" or "false".
5. Best Practices
Verify File Existence: ALWAYS ensure the target file exists.
Prefer <apply_diff> for Complex or Contextual Changes:
<search_and_replace> is best for straightforward, global, or pattern-based text replacements.
For changes requiring specific context, depending on surrounding lines, or involving complex structural modifications, <apply_diff> is generally safer and more precise.
Specificity is Key:
Make your <search> term (literal or regex) as specific as possible.
Consider adding unique surrounding text to your search string if the target text itself is common.
Test Regex Patterns Thoroughly: If using <use_regex>true</use_regex>, test patterns in a dedicated regex tester against sample text from the target file.
Understand Regex Capture Groups: If using capture groups ($1, $2, etc.) in the <replace> string, ensure your regex correctly defines these groups.
Escaping Special Characters:
For literal searches (<use_regex>false</use_regex>), special characters are generally treated literally.
For regex searches (<use_regex>true</use_regex>), correctly escape regex metacharacters in your <search> pattern.
Use Line Scoping: Employ <start_line> and <end_line> whenever possible to limit the scope of changes, reducing the risk of unintended modifications in large files.
Read File Before and After: Use <read_file> to examine relevant parts of the file before the operation to confirm your <search> term and after to verify changes, especially for critical operations.
By following these guidelines, agents can utilize <search_and_replace> effectively and safely for targeted text manipulations.