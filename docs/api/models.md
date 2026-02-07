# Core Models

Module: `vadocs.core.models`

> For a conceptual overview, see [Developer Guide: Data Models](/docs/developer_guide/02_data_models.md).

All models are dataclasses. They are the primary data structures passed between pipeline stages (parsing, validation, fixing).

---

### class Document

Represents a documentation file loaded into memory. This is the primary input to all validators and fixers.

```python
Document(path, content, frontmatter=None, doc_type=None)
```

**Attributes:**

- **path** (`Path`) — Path to the source file.
- **content** (`str`) — Raw file content (full markdown including frontmatter).
- **frontmatter** (`dict | None`) — Parsed YAML frontmatter dictionary, or `None` if not yet parsed.
- **doc_type** (`str | None`) — Document type identifier (e.g., `"adr"`, `"rfc"`). Used by `supports()` methods to filter applicable plugins.

> **Note:** `frontmatter` can be pre-parsed before constructing a `Document`, or left as `None` — validators and fixers will call `parse_frontmatter()` themselves when needed.

**Usage:**

```python
from vadocs import Document, parse_frontmatter
from pathlib import Path

path = Path("architecture/adr/adr_26001_example.md")
content = path.read_text()
doc = Document(
    path=path,
    content=content,
    frontmatter=parse_frontmatter(content),
    doc_type="adr",
)
```

---

### class ValidationError

Represents a single validation failure. Validators return lists of these.

```python
ValidationError(identifier, error_type, message)
```

**Attributes:**

- **identifier** (`int | str`) — Context-specific identifier for the error (e.g., ADR number `26001` or filename `"readme.md"`).
- **error_type** (`str`) — Programmatic category for filtering and grouping. Common values: `"missing_field"`, `"invalid_status"`, `"invalid_date"`, `"invalid_tag"`, `"empty_tags"`, `"missing_section"`, `"title_mismatch"`, `"missing_frontmatter"`, `"invalid_value"`, `"broken_term_reference"`.
- **message** (`str`) — Human-readable error description.

---

### class SyncField

Represents a field that can exist in both YAML frontmatter and markdown body. Used by `SyncFixer` to detect mismatches.

```python
SyncField(name, yaml_value=None, markdown_value=None)
```

**Attributes:**

- **name** (`str`) — Field name (e.g., `"title"`, `"status"`, `"date"`).
- **yaml_value** (`str | None`) — Value from YAML frontmatter.
- **markdown_value** (`str | None`) — Value from markdown content.

**Properties:**

- **is_synced** (`bool`) — `True` if values match or only one source has a value. `False` if both are `None` (no data) or if values differ.

---

### class SyncResult

Return type for all fixer operations. Includes both the list of changes made and any errors encountered.

```python
SyncResult(modified, changes=[], errors=[])
```

**Attributes:**

- **modified** (`bool`) — `True` if the document file was actually written (always `False` when `dry_run=True`).
- **changes** (`list[str]`) — Human-readable descriptions of changes made (or that would be made in dry-run mode).
- **errors** (`list[str]`) — Descriptions of issues that could not be automatically fixed.
