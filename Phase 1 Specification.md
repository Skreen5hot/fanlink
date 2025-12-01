# 🚀 Phase 1: Protocol Foundation (Hash-IRI Spec v0.1)

**Project Goal:** Define the minimum viable protocol, cryptographic standards, and data structures necessary to implement a secure, updatable, content-addressed knowledge object resolver.

**Time Estimate:** 4 Sprints (8 Weeks)

---

## 📦 Epic 1: Hash-IRI Syntax and Scheme Definition

**Goal:** Establish the standardized format for all HIRI identifiers, making them globally recognizable and parsable.

| Status | User Story | Acceptance Criteria (Definition of Done) | Priority |
| :--- | :--- | :--- | :--- |
| ☐ | **P1.1: Standardized Scheme** | The HIRI URI scheme is formally defined as `hiri://` and registered (informally) within the project specification. | P1 |
| ☐ | **P1.2: Path Structure** | The path structure is defined as `hiri://[protocol-authority]/[object-type]/[unique-id]`. E.g., `hiri://author/policy/hr-guide`. | P1 |
| ☐ | **P1.3: Content Hash Encoding** | The content hash component uses the **Multiformats/Multihash** standard for future compatibility, ensuring hash algorithm and length are self-describing. | P2 |

---

## 🔐 Epic 2: Cryptographic Standards Selection

**Goal:** Select and document the specific cryptographic primitives (hashing and signature algorithms) that guarantee integrity and authorship.

| Status | User Story | Acceptance Criteria (Definition of Done) | Priority |
| :--- | :--- | :--- | :--- |
| ☐ | **P2.1: Hashing Algorithm** | The protocol mandates **SHA-256** (for initial V0.1) and defines the inclusion of the hash algorithm identifier via **Multihash**. A formal justification document for the choice is drafted. | P1 |
| ☐ | **P2.2: Signature Algorithm** | The signature standard is selected: **Ed25519 (or ECDSA over secp256k1)** for high performance and security. Public/Private key generation and signature verification pseudo-code is documented. | P1 |
| ☐ | **P2.3: Key Management Reference** | A "Best Practices" appendix is drafted, advising implementers on secure storage (e.g., hardware wallets, encrypted vaults) and referencing the need for key rotation (deferred to Phase 4). | P3 |

---

## 📝 Epic 3: Resolution Manifest Data Model (The Core Innovation)

**Goal:** Design the data structure that securely links the stable HIRI to its latest verified content hash, ensuring updatability and verifiability.

| Status | User Story | Acceptance Criteria (Definition of Done) | Priority |
| :--- | :--- | :--- | :--- |
| ☐ | **P3.1: Data Serialization** | The Resolution Manifest **MUST** be serialized using **JSON-LD**. The base `@context` and required vocabulary must be defined. | P1 |
| ☐ | **P3.2: Manifest Required Fields** | The JSON-LD schema includes the following **MUST HAVE** fields: `hiri:id`, `hiri:contentHash`, `hiri:prevManifestHash`, `hiri:timestamp`, and `hiri:signature`. | P1 |
| ☐ | **P3.3: Cryptographic Linking** | The `hiri:prevManifestHash` field **MUST** contain the cryptographic hash of the *entire* previous signed Manifest JSON-LD object, establishing the **Sequential Manifest Link**. | P1 |
| ☐ | **P3.4: Signature Target** | The signature (`hiri:signature`) **MUST** be calculated over a canonicalized version of the Manifest (excluding the signature field itself). The canonicalization method (e.g., [RFC 8785: JCS]) is formally specified. | P1 |

---

## 🔄 Epic 4: Client Resolution Flow Specification

**Goal:** Document the exact steps a client must take to resolve a HIRI and confirm its integrity, forming the basis for the Phase 2 Resolver SDK.

| Status | User Story | Acceptance Criteria (Definition of Done) | Priority |
| :--- | :--- | :--- | :--- |
| ☐ | **P4.1: High-Level Flow Diagram** | A flow diagram is created showing the 4 steps: 1) Resolve Stable ID → Manifest, 2) Verify Signature, 3) Fetch Content, 4) Verify Content Hash. | P1 |
| ☐ | **P4.2: Resolution Interface** | The core resolution function is defined: `resolve(hiri_id, public_key) -> VerifiedResource` (or throws a verifiable error). | P2 |
| ☐ | **P4.3: Error Handling** | Specific error codes are defined for: Signature Mismatch, Content Hash Mismatch, Chain Break (Invalid `prevManifestHash`), and Network Failure. | P3 |

