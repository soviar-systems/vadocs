# Parsing Utilities

Module: `vadocs.core.parsing`

> For a conceptual overview, see [Developer Guide: Architecture](/docs/developer_guide/01_architecture.md).

Low-level parsing functions used by validators and the sync engine. All functions are stateless and work on raw string content.

---

## Module Constants

### FRONTMATTER_PATTERN

```python
FRONTMATTER_PATTERN = re.compile(r"^---\s*\n(.*?)\n---\s*\n", re.DOTALL)
```

Matches YAML frontmatter at the start of a file, delimited by `---` markers. Group 1 captures the YAML content between delimiters.

### STATUS_SECTION_PATTERN

```python
STATUS_SECTION_PATTERN = re.compile(r"^##\s+Status\s*\n+\s*(\w+)", re.MULTILINE)
```

Matches the first word after a `## Status` header. Group 1 captures the status value.

### SECTION_HEADER_PATTERN

```python
SECTION_HEADER_PATTERN = re.compile(r"^##\s+(.+?)\s*$", re.MULTILINE)
```

Matches any `##`-level section header. Group 1 captures the section name.

---

## Functions

### parse_frontmatter

```python
parse_frontmatter(content) → dict | None
```

Parse YAML frontmatter from file content.

**Parameters:**

- **content** (`str`) — File content that may contain YAML frontmatter.

**Returns:** Parsed frontmatter as a dictionary, or `None` if:

- No frontmatter present
- Frontmatter not at the start of the file
- Invalid YAML syntax

**Example:**

```python
>>> content = "---\ntitle: Test\n---\n\n# Content"
>>> parse_frontmatter(content)
{'title': 'Test'}
```

---

### extract_status

```python
extract_status(content) → str | None
```

Extract status from document content, checking two sources in priority order:

1. YAML frontmatter `status` field
2. Content under `## Status` markdown section

**Parameters:**

- **content** (`str`) — Document content (markdown with optional frontmatter).

**Returns:** Normalized lowercase status string, or `None` if not found in either source.

**Example:**

```python
>>> content = "---\nstatus: Accepted\n---\n"
>>> extract_status(content)
'accepted'
```

---

### extract_section_content

```python
extract_section_content(content, section_name) → str | None
```

Extract the content of a markdown `##`-level section — everything between the section header and the next `##` header (or end of file).

**Parameters:**

- **content** (`str`) — Document content (markdown).
- **section_name** (`str`) — Name of the section to extract (case-sensitive, must match exactly).

**Returns:** Section content as a string (may be empty), or `None` if the section is not found.

**Example:**

```python
>>> content = "## Context\n\nSome context.\n\n## Decision\n"
>>> extract_section_content(content, "Context")
'\nSome context.\n\n'
```
