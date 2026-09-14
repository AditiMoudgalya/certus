# Certus

## Project Context

Certus was **designed and built by Aditi Moudgalya** as a personal project exploring **continuous media provenance for AI-processed speech pipelines**.

The project examines how cryptographic provenance can be used to continuously record and verify speech integrity as media passes through multiple AI-based transformations.

---

## What is Certus?

**Certus is an observational provenance layer that continuously records and verifies speech integrity across AI-processed speech pipelines.**

Certus operates alongside the media transformation path, observing media states and building cryptographic provenance without controlling the underlying speech-processing workflow.

---

## The Problem

AI-processed speech can undergo multiple legitimate transformations such as noise cancellation, source separation, accent translation etc. At the same time, ordinary channel effects and unauthorised manipulation can also alter media.

Existing approaches address individual aspects of media trust, but this creates an emerging product question:

> **How can the integrity and transformation history of AI-processed speech remain continuously verifiable across the speech pipeline?**

Certus explores this gap through **continuous media provenance**.

---

## Product Proposition

Certus is built around three principles:

### 1. Continuous Media Provenance

Cryptographic provenance is established at the source and maintained alongside successive speech transformations.

### 2. Observational, Not Controlling

Certus observes the media pipeline and records provenance evidence without becoming the controller of the underlying speech-processing path.

### 3. Independent Integrity Verification

The final media and accumulated provenance evidence are independently verified before an integrity determination is produced.

---

## How Certus Works

Certus operates through two parallel paths: the **Audio Transformation Path** and the **Provenance Path**.

**1. Source Provenance**  
Source audio is divided into provenance chunks. Each bound chunk is hashed, and the resulting hashes are combined through a Merkle Tree to establish a single **Source Merkle Root**.

**2. Noise Cancellation**  
The source audio undergoes Noise Cancellation. Certus observes the relevant media states and generates cryptographic evidence for the transformation.

**3. Noise Cancellation Provenance**  
The transformation input and output are fingerprinted. Certus constructs a structured attestation describing the transformation and digitally signs it.

**4. Source Separation**  
The Noise-Cancelled audio undergoes Source Separation while Certus continues observing the media path.

**5. Source Separation Provenance**  
Certus verifies continuity with the preceding media state, validates the preceding provenance evidence, and creates the next signed transformation attestation.

**6. Accent Translation**  
The resulting audio undergoes Accent Translation to produce the final authorised transformed media.

**7. Accent Translation Provenance**  
Certus again verifies media continuity and preceding provenance evidence before creating the final signed transformation attestation.

**8. Independent Verification**  
Certus's Independent Verifier evaluates the final observed media against the accumulated provenance evidence, including media fingerprints, digital signatures, provenance context, Source Merkle Root, transformation sequence, and continuity.

**9. Integrity Classification**  
The verification evidence is converted into a final integrity determination, with Root Cause Analysis applied where required.

---

## Cryptography Core

Certus uses a small set of cryptographic and provenance mechanisms to establish and verify media history.

| Component | Role | MVP Implementation |
| --- | --- | --- |
| **Hash — WHAT media?** | One-way fingerprint of observed media | SHA-256 |
| **Attestation — WHAT happened?** | Records the transformation | Canonical JSON |
| **Signature — TRUST Attestation?** | Makes the attestation trustworthy | Ed25519 Signature |
| **Merkle Root — Source Provenance** | Creates a single cryptographic source anchor | Hash + Concatenate + Hash using SHA-256 |

Together, these allow Certus to identify media states, record authorised transformations, establish source provenance, and verify the resulting provenance chain.

---

## Integrity Classification

Certus produces a four-way integrity classification:

| Classification | Meaning |
| --- | --- |
| **Authorised Transformation** | Media and provenance evidence are consistent with the recorded authorised transformation path |
| **Benign Channel Variation** | Variation is attributable to non-malicious channel or transmission effects |
| **Integrity Indeterminate** | Available evidence is insufficient to establish a definitive integrity outcome |
| **Suspected Unauthorised Manipulation** | Verification evidence indicates a potential unauthorised alteration |

**Authorised Transformation** can be determined directly from valid verification evidence. Other outcomes may require **Root Cause Analysis (RCA)** to determine the appropriate classification.

---

## Core Product Features

| Feature | Purpose |
| --- | --- |
| **Source Provenance** | Establishes the cryptographic provenance anchor for the original source audio |
| **Chunk Fingerprinting & Merkle Commitment** | Binds source media into a verifiable cryptographic structure |
| **Signed Transformation Attestations** | Records and authenticates authorised speech transformations |
| **Media Continuity Verification** | Verifies continuity between consecutive observed media states |
| **Independent Verification** | Evaluates final media against accumulated provenance evidence |
| **Integrity Classification & RCA** | Converts verification evidence into an actionable integrity outcome |

---

## MVP Technology Stack

Certus separates speech processing, provenance processing, workflow orchestration, interface, and data storage.

| Layer | Implementation |
| --- | --- |
| **Noise Cancellation** | DeepFilterNet3 |
| **Source Separation** | SpeechBrain SepFormer WHAMR-16k |
| **Accent Translation** | Seed-VC V2 |
| **Processing & Verification** | Python + Flask |
| **Workflow Orchestration** | n8n |
| **Front End** | Figma Make |
| **Data Layer** | Google Sheets |
| **Cryptographic Fingerprinting** | SHA-256 |
| **Digital Signatures** | Ed25519 |

n8n orchestrates the MVP workflow, while the underlying speech transformations, cryptographic processing, and verification are performed by the Python implementation.

---

## MVP Scope

### Included

- Source provenance
- Provenance chunk fingerprinting
- Source Merkle Root construction
- Sequential AI speech transformation simulation
- Media-state hashing
- Transformation attestations
- Ed25519 digital signing
- Provenance continuity verification
- Independent verification
- Integrity classification and RCA
- Prototype workflow orchestration
- Prototype front-end experience

### Out of Scope

- Live PSTN / real-time telephony integration
- Production speech-provider integrations
- Production key management
- Enterprise-scale deployment

The current MVP validates the **provenance and integrity-verification architecture**, rather than functioning as a production real-time speech infrastructure layer.

---

## Product Metrics

### North Star Metric — Integrity Determination Accuracy

Measures the proportion of integrity determinations that Certus classifies correctly.

### End-to-End Provenance Continuity

Measures whether continuous provenance is maintained across the entire speech-processing lifecycle.

### Misclassification Rate

Measures the proportion of integrity determinations that Certus classifies incorrectly.

### Latency

Measures processing latency across the real-time media workflow, with a target of **<150 ms one way** to support real-time operation.

---

## Target Users

Certus is designed for organisations building or operating **AI-based speech processing infrastructure**, particularly where speech undergoes multiple transformations before reaching its destination.

Initial product exploration is focused on the **CCaaS and AI speech ecosystem**, including speech technology providers and contact-centre platforms.

---

## Current MVP vs Production Behaviour

The current MVP uses a file-based sequential implementation to validate the provenance architecture.

If required provenance evidence fails verification, the MVP does not create and sign the next transformation attestation.

In a production real-time implementation, **Certus should not block the live media path**. Media processing would continue while Certus records the provenance failure, marks the affected continuity boundary as unverified, and avoids claiming continuous verified provenance across that boundary.

Certus provenance processing is therefore intended to operate **alongside the live media stream without controlling or blocking the transformation path**.

---

## Documentation

Repository artefacts follow the **WXX naming convention**, where **W denotes Work Product** and the number indicates the artefact sequence.

- Product Requirements Document (PRD)
- Product Workflow
- [Certus Figma Demo Portal](https://certus-provenance.figma.site/)

---

## Current Status

**MVP / Functional Prototype**

The current implementation is intended to validate Certus's core product hypothesis:

> **Continuous cryptographic provenance can provide verifiable evidence of speech integrity across an AI-processed speech pipeline without becoming the controller of the underlying media path.**
