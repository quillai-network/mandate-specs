# Mandate Specifications

This repository defines the open standard for **Mandates** — deterministic, verifiable agreements between autonomous agents or humans, built under [ERC-8004 (Trustless Agents Standard)](https://eips.ethereum.org/EIPS/eip-8004).

Mandates describe **what** must be done and **who** is responsible.  
They do **not** define how validation, payment, or consensus occurs.

---

## Purpose

- Provide a **universal schema** for describing agent-to-agent commitments  
- Establish **consistent identifiers** (IDs, timestamps, signatures)  
- Keep the **core task body free-form**, so domain-specific primitives (e.g. `swap@1`, `bridge@1`) can evolve independently  
- Ensure all Mandates are **verifiable on-chain** via EIP-712 / EIP-191 signatures  

---

## Repository Layout
```
mandate-specs/
├─ README.md
├─ CONTRIBUTING.md
├─ VERSIONING.md
├─ docs/
│  ├─ specification.md      
│  └─ roadmap.md            
└─ spec/
   ├─ core/
   │  ├─ mandate.schema.json   # base/outer contract (client/server, ids, sigs, timestamps, core as free-form)
   │  └─ receipt.schema.json   # (optional/next) envelope for validation receipts
   ├─ primitives/
   │  └─ (future primitive folders, e.g., swap@1/, bridge@1/, tokenDataCheck@1 etc.)
   └─ examples/
      └─ (json instances that combine core + a primitive’s payload)
```
---

## Key Concepts

| Layer | Description |
|--------|-------------|
| **Header** | Metadata, versioning, and participant identifiers (`client`, `server`) |
| **Core** | Free-form task body containing primitive-specific data |
| **Signatures** | Detached EIP-712 / EIP-191 signatures proving mutual agreement |

### Example (minimal)
```json
{
  "mandateId": "01J9X9A3T3DMD3M3CYAJW1Y0SZ",
  "version": "0.1.0",
  "client": "eip155:1:0xCLIENT00000000000000000000000000000000001",
  "server": "eip155:1:0xSERVER00000000000000000000000000000000002",
  "createdAt": "2025-10-23T10:00:00Z",
  "deadline": "2025-10-23T10:20:00Z",
  "intent": "Create a mandate describing the terms of a deal.",
  "core": {},
  "signatures": {
    "clientSig": {
      "alg": "eip712",
      "mandateHash": "0x0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef",
      "signature": "0xaaaaa..."
    },
    "serverSig": {
      "alg": "eip712",
      "mandateHash": "0x0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef",
      "signature": "0xbbbb..."
    }
  }
}

```

---

## Schema

The canonical base schema lives at:

``` https://quillai-network.github.io/mandate-specs/spec/core/mandate.schema.json ```


and references the official JSON Schema dialect:

``` "$schema": "https://json-schema.org/draft/2020-12/schema" ```


This schema validates only the outer structure.
The core object is intentionally open, to be extended by domain-specific primitive schemas in future versions.


## Contributing

All contributions go through pull requests.

External contributors must fork the repo.

Every change requires at least one maintainer approval.
See [CONTRIBUTING.md](CONTRIBUTING.md)
 for minimal workflow details.

## Versioning

Each schema follows semantic versioning (MAJOR.MINOR.PATCH).
Published versions are tagged and served under versioned paths on GitHub Pages.
See VERSIONING.md
 for details.

## License

Released under the MIT License
.
