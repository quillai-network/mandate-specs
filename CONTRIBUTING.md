# Contributing

Contributions to the **Mandate Specifications** repository are welcome.

This repository defines the open standard for Mandates — it is early-stage and still evolving.  
The goal is to maintain clarity, simplicity, and deterministic structure across all contributions.

---

## How to Contribute

1. **Fork** the repository  
2. **Create** a new branch for your change  
3. **Add or modify**:
   - A schema under `/spec/core/`
   - A primitive under `/spec/primitives/`
   - An example under `/examples/`
4. **Validate** your JSON files using the provided schema definitions  
5. **Submit** a Pull Request describing your change  

---

## Guidelines

- Keep all contributions minimal, well-structured, and backward compatible  
- Use **semantic versioning** in the `spec_version` field  
- All JSON must validate successfully before submission  
- Include a brief note in the PR describing:
  - What was added or changed  
  - Why it was needed  

---

## Validation

Use local or CI validation to ensure compliance:

