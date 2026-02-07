# vadocs Documentation

vadocs is a validation engine for Documentation-as-Code workflows. It validates structure, enforces consistency, and auto-fixes common issues in documentation.

## Developer Guide

Conceptual guides and tutorials for using and extending vadocs.

- [Architecture Overview](developer_guide/01_architecture.md) — pipeline, project structure
- [Core Data Models](developer_guide/02_data_models.md) — Document, ValidationError, SyncField, SyncResult
- [Extending vadocs: Validators and Fixers](developer_guide/03_plugins.md) — plugin architecture, creating custom plugins
- [Development Workflow](developer_guide/04_development.md) — environment, testing, dogfooding, roadmap

## API Reference

Complete reference for all public symbols.

- [API Index](api/index.md) — package structure, quick import reference
- [Core Models](api/models.md) — `Document`, `ValidationError`, `SyncField`, `SyncResult`
- [Parsing Utilities](api/parsing.md) — `parse_frontmatter`, `extract_status`, `extract_section_content`
- [Validators](api/validators.md) — `Validator`, `AdrValidator`, `FrontmatterValidator`, `MystGlossaryValidator`, `AdrTermValidator`
- [Fixers](api/fixers.md) — `Fixer`, `SyncFixer`, `AdrFixer`, `SyncDirection`
- [Configuration](api/configuration.md) — `load_config`, config key reference

## Architecture Decision Records

- [ADR Index](adrs/adr_index.md)
- [ADR-26001: Dogfooding for Self-Documentation](adrs/adr_26001_dogfooding_self_documentation.md)

