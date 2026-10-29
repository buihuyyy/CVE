
# Stored CSV Formula Injection in Search Insights for WordPress

**Supplier:** Search Insights / Mark Wolters
**Product:** Search Insights — WordPress plugin (`wp-search-insights`)
**Affected version confirmed:** 2.1
**Affected version range:** Not yet established
**Fixed version:** Not known
**Vulnerability type:** Stored CSV/formula injection (CWE-1236)

## Summary

An unauthenticated visitor can submit a normal front-end search term beginning with a spreadsheet formula character. Search Insights stores the term and later includes it in an administrator-generated CSV export without neutralizing that character

In local testing with version 2.1, the exported CSV contained the attacker-controlled search term beginning with `=` unchanged. A spreadsheet application may interpret the cell as a formula when an administrator opens the exported file. This requires the administrator to export and open the CSV, and the result may depend on the spreadsheet application and its security settings

## Affected code paths

- `class-search.php`: `get_regular_search()` captures front-end searches; `write_search_term_to_single_table()` stores the search term after `sanitize_text_field()`
- `class-export.php`: `create_csv_file()` prepares the export; `build_csv_line()` escapes CSV delimiters and quotes but does not neutralize leading spreadsheet formula characters

## Verification and scope

The behavior described above was reproduced in a local WordPress lab using plugin version 2.1. The broader affected version range and a fixed version have not been confirmed

The demonstrated result is that the formula-like value is preserved in the exported CSV
