# Versioning

All specification files use **semantic versioning**:  
`MAJOR.MINOR.PATCH`

| Type | Increment | Description |
|------|------------|-------------|
| Breaking changes | MAJOR | Structural or required field modifications |
| Additions | MINOR | New optional fields, primitives, or examples |
| Fixes / documentation | PATCH | Clarifications or non-breaking edits |

Each JSON file includes its version via the `spec_version` field.  
Repository-level versions are tagged as `vX.Y.Z`.
