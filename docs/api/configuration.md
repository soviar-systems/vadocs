# Configuration

Module: `vadocs.config`

> For a conceptual overview, see [Developer Guide: Development](/docs/developer_guide/04_development.md).

vadocs validators and fixers are configured via YAML files loaded at runtime. Future versions (v0.2.0+) will support `pyproject.toml` `[tool.vadocs]` sections.

---

## Functions

### load_config

```python
load_config(config_path) → dict
```

Load configuration from a YAML file.

**Parameters:**

- **config_path** (`Path`) — Path to the YAML configuration file.

**Returns:** Configuration dictionary (empty dict if the file is valid YAML but contains no data).

**Raises:**

- `FileNotFoundError` — If `config_path` does not exist.
- `yaml.YAMLError` — If the file contains invalid YAML.

**Example:**

```python
from pathlib import Path
from vadocs import load_config, AdrValidator, Document

config = load_config(Path("adr_config.yaml"))
validator = AdrValidator()
errors = validator.validate(doc, config)
```

---

## Config Key Reference

The following keys are used by the built-in validators and fixers. The canonical example is `docs/adrs/adr_config.yaml`.

### term_reference

Used by `AdrTermValidator` (see [Validators](validators.md)).

| Key | Type | Description |
|---|---|---|
| `separator` | `str` | Separator between "ADR" and number in glossary entries. Default: `"-"` |
| `valid_pattern` | `str` | Regex matching valid term references (group 1 = ADR number) |
| `broken_pattern` | `str` | Regex matching broken term references that need fixing (group 1 = ADR number) |

### required_fields

Used by `AdrValidator` and `FrontmatterValidator`.

```yaml
required_fields:
  - id
  - title
  - date
  - status
  - tags
```

### date_format

Used by `AdrValidator`.

```yaml
date_format: "^\\d{4}-\\d{2}-\\d{2}$"
```

Regex pattern for date validation. Default matches `YYYY-MM-DD`.

### tags

Used by `AdrValidator`.

```yaml
tags:
  - architecture
  - ci
  - documentation
  - governance
  # ...
```

List of valid tags. Documents with tags not in this list produce `invalid_tag` errors.

### required_sections

Used by `AdrValidator`.

```yaml
required_sections:
  - Date
  - Status
  - Context
  - Decision
  - Consequences
  - Alternatives
  - References
  - Participants
```

### statuses

Used by `AdrValidator` and `AdrFixer`.

```yaml
statuses:
  - proposed
  - accepted
  - rejected
  - superseded
  - deprecated
```

### default_status

Used by `AdrFixer`. Fallback status when no correction mapping exists.

```yaml
default_status: proposed
```

### status_corrections

Used by `AdrFixer`. Maps common typos and synonyms to the correct status value.

```yaml
status_corrections:
  proposed:
    - draft
    - pending
    - wip
  accepted:
    - approved
    - active
    - implemented
  # ...
```

### sections

Index display grouping. Maps section headings to lists of statuses.

```yaml
sections:
  Active Architecture:
    - accepted
  Evolutionary Proposals:
    - proposed
  Historical Context:
    - rejected
    - superseded
    - deprecated
```

### sync_direction

Used by `SyncFixer`. One of `"auto"`, `"yaml_to_md"`, `"md_to_yaml"`.

### sync_fields

Used by `SyncFixer`. List of field names to synchronize between YAML and markdown.

```yaml
sync_fields:
  - title
  - status
  - date
```
