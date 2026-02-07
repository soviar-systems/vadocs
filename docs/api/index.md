# API Reference

This is the reference documentation for the `vadocs` public API. For tutorials and conceptual guides, see the [Developer Guide](/docs/developer_guide/).

## Package Structure

| Module | Page | Key Symbols |
|---|---|---|
| `vadocs.core.models` | [Core Models](models.md) | `Document`, `ValidationError`, `SyncField`, `SyncResult` |
| `vadocs.core.parsing` | [Parsing Utilities](parsing.md) | `parse_frontmatter`, `extract_status`, `extract_section_content` |
| `vadocs.validators` | [Validators](validators.md) | `Validator`, `AdrValidator`, `FrontmatterValidator`, `MystGlossaryValidator`, `AdrTermValidator` |
| `vadocs.fixers` | [Fixers](fixers.md) | `Fixer`, `SyncFixer`, `AdrFixer`, `SyncDirection` |
| `vadocs.config` | [Configuration](configuration.md) | `load_config` |

## Quick Import Reference

All public symbols are re-exported from the top-level package:

```python
from vadocs import (
    # Models
    Document,
    ValidationError,
    SyncField,
    SyncResult,
    # Parsing
    parse_frontmatter,
    extract_status,
    extract_section_content,
    # Validators
    Validator,
    AdrValidator,
    FrontmatterValidator,
    MystGlossaryValidator,
    AdrTermValidator,
    # Fixers
    Fixer,
    AdrFixer,
    SyncFixer,
    # Config
    load_config,
)
```

## Version

vadocs v0.1.0 — library-only, no CLI. Import directly or use `vadocs.config.load_config` to load YAML configuration files.
