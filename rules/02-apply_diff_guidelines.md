# `apply_diff`: Error Prevention & Best Practices

The `<apply_diff>` tool is essential for precise code modifications. However, incorrect usage can lead to errors or unintended changes. Adherence to these guidelines, based on the **official `apply-diff.md` documentation**, is **CRITICAL**.

## 1. Core Principle: Exact Matching & Valid Structure

The fundamental requirement for `apply_diff` to work correctly is that the content within the `<<<<<<< SEARCH` and `=======` markers (the "SEARCH block") **MUST EXACTLY MATCH** a contiguous block of text in the specified `<path>`. The `:start_line:` hint within the SEARCH block is crucial for guiding the tool.

### 1.1. Correct XML Structure (as per `apply-diff.md`):

```xml
<apply_diff>
  <path>path/to/your/file.ext</path>
  <diff>
<<<<<<< SEARCH
:start_line:123  <!-- Mandatory hint for the tool -->
This is the exact, literal block of text
that currently exists in the file and
needs to be found.
It can span multiple lines.
Indentation and whitespace must also match precisely.
=======
This is the new block of text
that will replace the SEARCH block.
It can also span multiple lines.
The final content will be this REPLACE block.
>>>>>>> REPLACE
  </diff>
  <!-- Optional top-level <start_line> and <end_line> parameters exist
       but the official documentation notes the main strategy relies on
       the :start_line: within the diff content and exact text match.
       Use these top-level hints if they assist your specific Roocode version.
  <start_line>120</start_line>
  <end_line>150</end_line>
  -->
</apply_diff>

1.2. The SEARCH Block is Literal:
The content between <<<<<<< SEARCH (after the :start_line: hint) and ======= is NOT a regular expression or a pattern. It's a literal string.
Every character, including spaces, tabs, newlines, and comments, must be identical to the content in the file.
1.3. The REPLACE Block is the Result:
The content between ======= and >>>>>>> REPLACE will be the new content in the file, replacing the SEARCH block.
1.4. Mandatory :start_line: Hint:
The :start_line:N hint within the <<<<<<< SEARCH block is mandatory for the primary matching strategy, as per the official documentation. This line number refers to the expected starting line of the SEARCH block in the original file.
2. CRITICAL: <read_file> First!
ALWAYS use <read_file> on the target file before constructing an <apply_diff> call.
<read_file>
  <path>path/to/your/file.ext</path>
  <!-- Optionally use start_line and end_line to narrow down -->
  <!-- <start_line>120</start_line> -->
  <!-- <end_line>150</end_line> -->
</read_file>

Use the exact output from <read_file> (including line numbers to determine the correct :start_line: hint) to construct your SEARCH block. This is the most reliable way to avoid mismatches.
3. Common Errors to AVOID:
Mismatched SEARCH Block:
Even a single character difference (space, newline, tab, case) will cause the diff to fail.
Solution: Copy-paste from <read_file> output for the SEARCH block content. Ensure the :start_line: hint is accurate.
Missing or Incorrect :start_line: Hint: The :start_line:N line inside the <<<<<<< SEARCH block is crucial.
Including Diff Markers in SEARCH/REPLACE Content: The markers <<<<<<< SEARCH, =======, >>>>>>> REPLACE are control syntax. They cannot appear literally within the content you are trying to search for or replace with, unless they are escaped (e.g., \\<<<<<<< if searching for literal markers, as per apply-diff.md).
Incorrect Diff Marker Syntax: Must be exactly seven <, seven =, seven >.
Attempting Multiple Diffs in One <apply_diff> Call: Each call handles one SEARCH/REPLACE. For multiple distinct changes, use sequential <apply_diff> calls. Be mindful that the file changes after each successful apply_diff.
4. Best Practices for Using apply_diff:
Small, Targeted Diffs: Easier to construct correctly and review.
Mind Whitespace and Line Endings: These are common culprits for mismatches.
Sequential Application for Multiple Changes:
If making multiple, non-contiguous changes, use separate <apply_diff> calls.
After a successful apply_diff, the file is modified. If subsequent SEARCH blocks depend on this modified state, use <read_file> again to get the updated content before the next apply_diff.
Fuzzy Matching Awareness: The official docs mention "fuzzy matching (Levenshtein distance on normalized strings) guided by a :start_line: hint". This means while exactness is key for reliability, the tool has some tolerance, but relying on this tolerance is risky. Aim for exact matches.
By strictly following these guidelines, based on the official documentation, agents can leverage <apply_diff> for precise and reliable file modifications.