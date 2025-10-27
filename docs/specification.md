# Mandate Specification

**Version:** 0.1.0  
**Status:** Draft  
**Last Updated:** 2025-10-23  

---

## 1. Introduction

This document defines the open standard for **Mandates** — deterministic, verifiable agreements between autonomous agents or humans operating under the [ERC-8004 (Trustless Agents Standard)](https://eips.ethereum.org/EIPS/eip-8004).  

Mandates specify *what* must be done and *who* is responsible, while remaining agnostic to payment models, consensus mechanisms, and validation logic.

Each Mandate is a **self-contained, immutable document** that can be stored on decentralized storage (e.g., IPFS) and referenced on-chain by its hash.

---



## 2. Structure Overview

A Mandate consists of four main layers:

| Layer | Description |
|--------|-------------|
| **Header** | Metadata, versioning, and participant identifiers |
| **Intent** | A text message from the client describing the purpose of the mandate |
| **Core** | The free-form payload that defines the actual task |
| **Signatures** | Cryptographic proofs of agreement from both the client and the server |

---

## 3. Field Definitions

### Top-Level Fields

| Field | Type | Required | Description |
|--------|------|-----------|-------------|
| **mandateId** | `string` | ✓ | Unique identifier for each Mandate. Use **ULID** or **UUIDv7** for globally unique and sortable IDs. Example: `01J9X9A3T3DMD3M3CYAJW1Y0SZ`. |
| **version** | `string` | ✓ | Version of the Mandate schema being used (`MAJOR.MINOR.PATCH`). |
| **client** | `string` | ✓ | Issuer of the Mandate, represented as a **CAIP-10 identifier** (`namespace:chainId:address`). Example: `eip155:1:0xabc...`. |
| **server** | `string` | ✓ | Executor or counterparty responsible for fulfilling the task (CAIP-10 identifier). |
| **createdAt** | `string (ISO-8601)` | ✓ | Timestamp indicating when the Mandate was created. Example: `2025-10-23T10:00:00Z`. |
| **deadline** | `string (ISO-8601)` | ✓ | Time limit by which the Mandate should be fulfilled. Example: `2025-10-23T10:20:00Z`. |
| **intent** | `string` | ✓ | A descriptive or non-descriptive text message authored by the **client**, describing the purpose or context of the Mandate. This can be a single sentence or a short paragraph, e.g., “Swap 100 USDC for WBTC on Ethereum mainnet.” |
| **core** | `object` | ✓ | Free-form task body containing data relevant to the commitment. The structure of this object is defined by the specific primitive (see Section 4). |
| **signatures** | `object` | ✓ | Detached signatures proving agreement between the `client` and the `server` (see Section 5). |

> **Note:**  
> The Mandate itself is immutable once signed. Any state transitions (e.g., fulfillment, expiration, revocation) are tracked externally via receipts or registries and are not part of this schema.

---

## 4. Core (Task Body)

The `core` section represents the body of the Mandate.  
It contains the domain-specific data that defines the task to be performed.

This section is intentionally **free-form**, allowing primitives to define their own internal schemas.  
Each primitive defines the **shape and validation rules** for its own payload.

### Example (Primitive Schema)
A primitive called `swap@1` might define the following payload:

```json
{
  "chainId": 1,
  "tokenIn": "0xA0b8...6eB48",
  "tokenOut": "0x2260...C599",
  "amountIn": "100000000",
  "minOut": "165000",
  "recipient": "0xUSER000000000000000000000000000000000001",
  "deadline": "2025-10-23T10:20:00Z"
}
```

## 5. Signatures

The signatures field contains cryptographic attestations from both the client and server.
Each signature is computed over the canonicalized Mandate JSON (excluding the signatures field).

| Field         | Type        | Required | Description                             |
| ------------- | ----------- | -------- | --------------------------------------- |
| **clientSig** | `Signature` | ✓        | Signature from the `client` (issuer).   |
| **serverSig** | `Signature` | ✓        | Signature from the `server` (executor). |

```json
{
  "alg": "eip712",
  "mandateHash": "0x4e8d5f1...",
  "signature": "0xSIG..."
}
```

### Notes

- mandateHash — Keccak256 hash of the canonicalized Mandate (excluding signatures).

- alg — Either eip712 or eip191.

- signer — CAIP-10 identifier of the entity that signed.

- domain — EIP-712 domain parameters (optional for EIP-191).

- signature — Hex-encoded digital signature.

- createdAt — Timestamp of when the signature was generated.



## 6. Canonicalization & Hashing

Before signing, the Mandate JSON must be canonicalized using JSON Canonicalization Scheme (JCS)
.
This ensures identical hashes for both parties, regardless of key ordering.

```mandateHash = keccak256(JCS(mandateWithoutSignatures))```


## 7. Storage & On-Chain Reference

Mandates are designed to be immutable and content-addressed.
The recommended pattern is:

Store the full Mandate JSON on IPFS (CIDv1).

Reference the CID and/or mandateHash in an on-chain registry or contract.