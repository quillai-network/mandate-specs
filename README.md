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
  "mandateId": "01K8N3AB6J9FD3RVAQQ4YSQMK3",
  "version": "0.1.0",
  "client": "eip155:1:0x5f451c9210A670681aA299Fdd0E64dBFA068D33b",
  "server": "eip155:1:0x0867CBE40D362F347842Fbb40acd47F04F0fe49a",
  "createdAt": "2025-10-28T09:45:19.314Z",
  "deadline": "2025-10-28T10:05:19.315Z",
  "intent": "Create a mandate describing the terms of a deal.",
  "core": {},
  "signatures": {
    "clientSig": {
      "alg": "eip191",
      "mandateHash": "0xa963cdc8d7269be6c8ff352b2348451fb73d4224cf88fa28288529e45b52687b",
      "signature": "0x47be2a932cfa8eb45cbf2b458603063345ca27c0c9c8f30c21a99d1610d6cb8b5f231deb28d6b1273c7d1d1fb1f5a24fca55f5c0244a9ce78315ed79d6047e3f1b"
    },
    "serverSig": {
      "alg": "eip191",
      "mandateHash": "0xa963cdc8d7269be6c8ff352b2348451fb73d4224cf88fa28288529e45b52687b",
      "signature": "0x9f30b87e54472f789d709923328c7f51af264e8cc4c8fc393f75a8c70b7b50db302ef4b97638af5c5d5d7ff9a802e216a34f9dfec6cb8139635dbbfb745a51bf1b"
    }
  }
}

```

---

## Schema

The canonical base schema lives at:

``` /mandate-specs/spec/core/mandate.schema.json ```


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
See [VERSIONING.md](VERSIONING.md) for details.

## License

Released under the MIT License.
