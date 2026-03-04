# IETF Post-Quantum Cryptography: Comprehensive Status Report
**Date:** 2026-03-04
**Source:** https://github.com/ietf-wg-pquip/state-of-protocols-and-pqc (forked)

---

## Table of Contents
1. [Executive Summary](#executive-summary)
2. [NIST Standards Status](#nist-standards-status)
3. [TLS](#tls)
4. [LAMPS / X.509 / CMS (PKI)](#lamps--x509--cms-pki)
5. [IPsec / IKEv2](#ipsec--ikev2)
6. [SSH](#ssh)
7. [COSE / JOSE](#cose--jose)
8. [OpenPGP](#openpgp)
9. [MLS (Messaging Layer Security)](#mls-messaging-layer-security)
10. [CFRG (Crypto Forum Research Group)](#cfrg-crypto-forum-research-group)
11. [Major Cross-WG Debates](#major-cross-wg-debates)
12. [Real-World Deployment Status](#real-world-deployment-status)
13. [Regulatory Drivers](#regulatory-drivers)

---

## Executive Summary

| Area | Readiness | Key Takeaway |
|------|-----------|--------------|
| **KEM Key Exchange** | PRODUCTION READY | X25519+ML-KEM-768 deployed on ~60% of web traffic |
| **PQ Signatures in TLS** | MAJOR UNSOLVED PROBLEM | 14-39 KB handshake overhead; no production solution |
| **PKI / Certificates** | 70-80% READY | Core algorithm bindings nearly done; composite debate ongoing |
| **IPsec / IKEv2** | ADVANCING WELL | ML-KEM adopted, PQC auth passed WG Last Call |
| **SSH** | FURTHEST AHEAD | NTRU Prime default in OpenSSH 5+ years; ML-KEM at IETF Last Call |
| **OpenPGP** | IESG APPROVED | Most advanced PQ draft in IETF -- composite approach |
| **COSE/JOSE** | PROGRESSING | Signature drafts mature; KEM progressing; hybrid HPKE lagging |
| **MLS** | EARLY STAGE | Innovative combiner approach; WG adopted |

**Bottom line:** KEM-based key exchange migration is well underway and largely successful. Signature/authentication migration remains the critical unsolved challenge, primarily due to the size impact of post-quantum signatures on latency-sensitive protocols.

---

## NIST Standards Status

| Standard | Algorithm | Based On | Status | Published |
|----------|-----------|----------|--------|-----------|
| **FIPS 203** | ML-KEM | CRYSTALS-Kyber | **FINAL** | August 13, 2024 |
| **FIPS 204** | ML-DSA | CRYSTALS-Dilithium | **FINAL** | August 13, 2024 |
| **FIPS 205** | SLH-DSA | SPHINCS+ | **FINAL** | August 13, 2024 |
| **FIPS 206** | FN-DSA | FALCON | Draft pending | Expected late 2026/early 2027 |
| **HQC** | HQC | Error-correcting codes | Selected March 2025 | Draft standard ~2027 |

**Additional Signature Candidates (Round 2):** 14 candidates advancing -- CROSS, FAEST, HAWK, LESS, MAYO, Mirath, MQOM, PERK, QR-UOV, RYDE, SDitH, SNOVA, SQIsign, UOV. Round 3 down-select planned for 2026.

---

## TLS

### draft-ietf-tls-ecdhe-mlkem (Hybrid ECDHE+ML-KEM)
**Status:** WG Document, v-04 (Feb 2026), **Standards Track**

Defines X25519MLKEM768, SecP256r1MLKEM768, SecP384r1MLKEM1024.

| Pros | Cons |
|------|------|
| Already massively deployed (~60% of Cloudflare traffic) | Larger ClientHello can exceed MTU |
| Belt-and-suspenders security | Combined key shares ~1,600 bytes |
| No adoption controversy; uncontested consensus | May trigger HelloRetryRequest round-trips |
| Promoted to Standards Track | |

---

### draft-ietf-tls-mlkem (Pure ML-KEM for TLS 1.3)
**Status:** WG Document, v-07 (Feb 2026), passed IETF Last Call. **HIGHLY CONTROVERSIAL.**

Defines pure (non-hybrid) ML-KEM-512/768/1024 as NamedGroups.

| Pros | Cons |
|------|------|
| Required for CNSA 2.0/FIPS compliance by 2033 | DJB filed formal IETF appeals alleging false consensus |
| Necessary migration path beyond hybrids | DJB moderated from TLS list during Last Call period |
| Clean specification | SIKE precedent argues hybrid is essential safety net |
| | Ongoing "NSA and IETF" blog series (parts 1-5) |

---

### draft-ietf-tls-mldsa (ML-DSA in TLS 1.3)
**Status:** WG Document, v-01 (Sep 2025)

| Pros | Cons |
|------|------|
| NIST's primary PQ signature standard | ML-DSA-65 signatures: 3,309 bytes |
| Clean WG adoption | Certificate chains add ~17 KB to handshakes |
| Fast sign/verify vs SLH-DSA | Exceeds TCP initcwnd (~14 KB), adds round-trips |
| | No hybrid/composite mechanism defined here |

---

### draft-reddy-tls-slhdsa (SLH-DSA in TLS 1.3)
**Status:** Individual, v-02 (Nov 2025), Call for Adoption issued May 2025

| Pros | Cons |
|------|------|
| Hash-based only -- no lattice assumptions | Signatures 7,856 to **49,856 bytes** |
| If ML-DSA broken, SLH-DSA survives | Much slower than ML-DSA |
| NIST standardized (FIPS 205) | Impractical for most TLS use cases |
| Smaller key sizes than ML-DSA | Adoption outcome unclear |

---

### draft-reddy-tls-composite-mldsa (Composite ML-DSA in TLS 1.3)
**Status:** Individual, v-09, **likely expired** (Dec 2025 expiry). NOT adopted by WG.

| Pros | Cons |
|------|------|
| Belt-and-suspenders for authentication | Doubles already-excessive signature overhead |
| Protection against ML-DSA bugs/breaks | Not adopted by WG |
| | TLS WG reluctant given signature size crisis |

---

### draft-ietf-tls-hybrid-design (Hybrid Key Exchange Framework)
**Status:** WG Document, v-16 (Sep 2025), Informational

| Pros | Cons |
|------|------|
| Foundational document for all hybrid KEX | Still Informational, not Standards Track |
| Clean concatenation-based design | Framework only -- no specific algorithms |
| 16 revisions, well-established | |

---

### draft-ietf-plants-merkle-tree-certs (Merkle Tree Certificates)
**Status:** Adopted by new **PLANTS WG**, v-02 (Mar 2026), **Standards Track**

**The most promising solution to the PQ signature size crisis.**

| Pros | Cons |
|------|------|
| Eliminates signatures from certs for modern clients | Only works for "up-to-date" relying parties |
| <800 byte Merkle proofs instead of ~17 KB signatures | Requires new infrastructure (tree publication) |
| Chrome designated MTCs as "preferred option for PQ certs" | Full ecosystem rollout could take 10-15 years |
| Standards Track, Google+Cloudflare backing | Limited implementation experience |

---

### draft-farrell-tls-pqg (Post-Quantum Guidance)
**Status:** Individual, v-04 (Dec 2025), Best Current Practice

**Key recommendations:** (1) Move to hybrid KEMs now. (2) Take no action on PQ signatures yet.

| Pros | Cons |
|------|------|
| Pragmatic, operationally focused | Not a WG document -- no formal weight |
| Correctly prioritizes KEX over signatures | "No action on signatures" may be too conservative |
| | Delays needed PQ signature work |

---

### draft-ietf-tls-key-share-prediction (Key Share Prediction)
**Status:** WG Document, v-03 (Aug 2025), Standards Track (updates RFC 8446)

| Pros | Cons |
|------|------|
| Avoids HelloRetryRequest round-trips | Depends on DNS infrastructure (SVCB/HTTPS) |
| DNS-based signaling mechanism | Stale DNS can cause mispredictions |
| Already adopted by WG | Codepoint assignment pending |

---

## LAMPS / X.509 / CMS (PKI)

### Completed (Published RFCs)

| RFC | Title | Significance |
|-----|-------|-------------|
| **RFC 9629** | KEMRecipientInfo for CMS | Foundation for all CMS KEM usage |
| **RFC 9763** | Related Certificates for Multi-Auth | Non-composite hybrid binding (NSA approach) |
| **RFC 9810** | CMP v3 with KEM Support | PKI enrollment infrastructure for PQC |
| **RFC 9814** | SLH-DSA in CMS | First PQC CMS signature RFC |
| **RFC (published)** | SLH-DSA in X.509 | Hash-based signatures in certificates |

### Near-Complete (IESG Approved)

| Draft | Status | Notes |
|-------|--------|-------|
| **draft-ietf-lamps-dilithium-certificates** | IESG Approved, in RFC Editor queue | ML-DSA algorithm IDs for X.509 |
| **draft-ietf-lamps-kyber-certificates** | IESG Approved, in RFC Editor queue | ML-KEM algorithm IDs for X.509 |

### In Progress

| Draft | Status | Notes |
|-------|--------|-------|
| **draft-ietf-lamps-cms-kyber** v-13 | IESG ballot active | ML-KEM in CMS |
| **draft-ietf-lamps-pq-composite-kem** v-12 | IESG evaluation | Composite ML-KEM + traditional |
| **draft-ietf-lamps-pq-composite-sigs** v-15 | WG Draft, OIDs allocated | Composite ML-DSA + traditional |
| **draft-ietf-lamps-x509-shbs** v-13 | IESG evaluation | Stateful hash-based sigs (HSS/XMSS) |

### Early / Stalled

| Draft | Status | Notes |
|-------|--------|-------|
| **draft-bonnell-lamps-chameleon-certs** v-07 | Individual | Encodes cert differences efficiently |
| **draft-ounsworth-lamps-pq-external-pubkeys** v-05 | Individual, **expired** | External key references by hash+URL |

### Composite ML-DSA Signatures -- Pros/Cons

| Pros | Cons |
|------|------|
| Defense-in-depth (secure if either algorithm holds) | Significantly larger certificates |
| Protocol backwards-compatible (looks like single algorithm) | Combinatorial explosion of OIDs |
| Aligned with ANSSI/BSI hybrid recommendations | Added implementation complexity |
| Addresses "what if ML-DSA is broken?" | NSA/CNSA 2.0 does NOT recommend hybrid |

### Composite ML-KEM -- Pros/Cons

| Pros | Cons |
|------|------|
| Same defense-in-depth as composite signatures | Same size/complexity concerns |
| Composite remains strong if one component holds | KEM composition raises KDF binding questions |
| Less contentious than signature composites | NSA opposition to hybrid approaches |

---

## IPsec / IKEv2

### draft-ietf-ipsecme-ikev2-mlkem (ML-KEM for IKEv2)
**Status:** WG Document, v-04 (Feb 2026), Standards Track

| Pros | Cons |
|------|------|
| Built on RFC 9370 (Multiple Key Exchanges) | Increased packet sizes, potential IP fragmentation |
| NIST-standardized (FIPS 203) | Downgrade attack prevention requires careful deployment |
| Most mature PQ KEX draft for IKEv2 | |
| Addresses fragmentation via IKE_INTERMEDIATE (RFC 9242) | |

### draft-ietf-ipsecme-ikev2-pqc-auth (PQC Auth in IKEv2)
**Status:** WG Document, v-06 (Oct 2025), Standards Track, **Passed WG Last Call**

| Pros | Cons |
|------|------|
| Clean integration via RFC 8420 framework | Large signatures (ML-DSA: 2420-4627 bytes) |
| Furthest along among auth drafts | SLH-DSA signatures even larger |
| Supports both ML-DSA and SLH-DSA | Does not address hybrid authentication |
| Deemed most cryptographically secure by WG | |

### draft-hu-ipsecme-pqt-hybrid-auth (Hybrid Auth in IKEv2)
**Status:** Individual, v-04 (Feb 2026), NOT adopted

| Pros | Cons |
|------|------|
| Safety net if PQC algorithms prove vulnerable | No WG adoption after 4 revisions |
| Aligns with LAMPS composite work | Lacks algorithm combination selection guidance |
| | Complex certificate management |

### draft-wang-ipsecme-hybrid-kem-ikev2-frodo (FrodoKEM for IKEv2)
**Status:** Individual, v-03 (Dec 2025), Call for Adoption ended Feb 2026 (20+ supportive comments)

| Pros | Cons |
|------|------|
| Plain LWE -- more conservative than structured lattices | Much larger key/ciphertext than ML-KEM |
| Recommended by EU (BSI, ANSSI) | Not NIST-selected (ISO only) |
| ISO standardization expected 2026 Q1 | Adds complexity: ML-KEM vs FrodoKEM choice |
| Strong support in adoption call | |

---

## SSH

### draft-ietf-sshm-mlkem-hybrid-kex (ML-KEM Hybrid KEX for SSH)
**Status:** WG Document, v-09 (Feb 2026), Standards Track, **IETF Last Call complete**, Gen-ART: "Ready with Nits"

Defines: `mlkem768nistp256-sha256`, `mlkem1024nistp384-sha384`, `mlkem768x25519-sha256`

| Pros | Cons |
|------|------|
| Extremely mature -- IETF Last Call complete | ML-KEM adds ~1.5 KB to key exchange |
| CNSA 2.0 compliant (mlkem1024nistp384) | Ephemeral keypair per connection required |
| Clear specification, positive reviews | |
| Multiple implementations expected | |

### draft-ietf-sshm-ntruprime-ssh (NTRU Prime + X25519 in SSH)
**Status:** **IESG Approved** as Informational RFC, v-06 (Sep 2025)

| Pros | Cons |
|------|------|
| Default in OpenSSH for ~5 years | NTRU Prime NOT a NIST winner |
| Documents existing massive deployment | Informational only, not Standards Track |
| Hybrid with X25519 provides safety net | Long-term future unclear as ML-KEM gains traction |

**Controversy:** Formal appeal filed over publishing RFC for non-NIST algorithm. IESG approved as Informational, acknowledging deployment reality.

---

## COSE / JOSE

### draft-ietf-cose-dilithium v-11 (ML-DSA in COSE/JOSE)
**Status:** WG Document, Standards Track, Active

| Pros | Cons |
|------|------|
| Foundational -- defines AKP key type for all PQ COSE drafts | Large pub keys (1312-2592 bytes) and sigs (2420-4627 bytes) |
| NIST-standardized (FIPS 204) | Seed-only private key format may conflict with expanded format impls |
| Compact private key representation (32 bytes seed) | |

### draft-ietf-cose-sphincs-plus v-06 (SLH-DSA in COSE/JOSE)
**Status:** WG Document, Standards Track, Active

| Pros | Cons |
|------|------|
| Hash-based -- most conservative security | Very large signatures |
| Good companion to ML-DSA for diversity | Slower than ML-DSA |
| NIST standardized (FIPS 205) | |

### draft-ietf-jose-pqc-kem v-05 (PQ KEMs for JOSE/COSE)
**Status:** WG Document, Standards Track, Active

| Pros | Cons |
|------|------|
| Fills KEM gap for JOSE/COSE | Does not address hybrid PQ/traditional |
| Steady progression through 5 revisions | |

### draft-reddy-cose-jose-pqc-hybrid-hpke v-11 (Hybrid HPKE)
**Status:** Individual, NOT adopted, requesting second adoption call

| Pros | Cons |
|------|------|
| Addresses critical hybrid use case | Still individual after 11 revisions |
| 11 revisions show sustained effort | Complex dependency chain (COSE-HPKE, JOSE-HPKE, HPKE-PQ) |
| | First adoption call deferred |

---

## OpenPGP

### draft-ietf-openpgp-pqc (PQC in OpenPGP)
**Status:** **IESG Approved as Proposed Standard**, v-17 (Jan 2026). **THE MOST ADVANCED PQ DRAFT IN IETF.**

Defines:
- Composite signatures: ML-DSA-65+Ed25519 (MUST), ML-DSA-87+Ed448 (SHOULD)
- Composite encryption: ML-KEM + ECDH (X25519/X448)
- SLH-DSA standalone: MAY implement

| Pros | Cons |
|------|------|
| IESG approved -- most advanced PQ draft anywhere | Composite keys/sigs significantly larger than classical |
| Multiple interoperable implementations | SLH-DSA support only MAY |
| Strong European institutional backing (BSI) | Implementations waiting for final RFC before persistent artifacts |
| Clear algorithm hierarchy (MUST/SHOULD/MAY) | |
| Sets template for other protocol areas | |

---

## MLS (Messaging Layer Security)

### draft-ietf-mls-combiner v-01 (Flexible Hybrid PQ MLS Combiner)
**Status:** WG Document, Standards Track, early stage

Combines traditional MLS session with PQ session. Two modes: PARTIAL (traditional-only, lower cost) and FULL (hybrid PQ).

| Pros | Cons |
|------|------|
| Elegant amortized approach -- PQ operations less frequent | Only at v-01 since adoption |
| Anti-downgrade protections built in | Complex two-session architecture |
| Companion draft defines ML-KEM cipher suites | Expired Dec 2025 (continued development indicated) |

---

## CFRG (Crypto Forum Research Group)

### draft-connolly-cfrg-xwing-kem v-09 (X-Wing Hybrid KEM)
**Status:** Individual, Informational. **NOT adopted by CFRG.**

Combines ML-KEM-768 + X25519 into a specific, optimized hybrid KEM.

| Pros | Cons |
|------|------|
| Simple, opinionated "just use X-Wing" | Not CFRG-adopted (generic combiner framework preferred) |
| Strong security proof | Informational only |
| Backed by prominent cryptographers | Only one security level (no P-384 variant) |
| Multiple implementations | FIPS certification unclear (awaiting SP 800-227) |
| | OpenJDK won't implement until RFC exists |

**Controversy:** CFRG adopted a broader `draft-irtf-cfrg-hybrid-kems` framework instead. Debate: concrete construction (X-Wing) vs. generic combiner that produces multiple hybrids.

### draft-fluhrer-lms-more-parm-sets v-19 (Additional LMS Parameters)
**Status:** Individual, IRSG evaluation, Informational. 19 revisions. NIST co-author.

| Pros | Cons |
|------|------|
| 35-40% signature size reduction with 192-bit params | 192-bit may face resistance from 256-bit mandaters |
| IANA code points already assigned | IRTF Informational only |
| Extremely mature (19 revisions) | |

---

## Major Cross-WG Debates

### 1. Hybrid vs. Pure PQC

| For Hybrid | Against Hybrid |
|------------|----------------|
| Defense-in-depth during transition | Implementation/operational complexity |
| SIKE precedent (broken after deployment) | Double migration cost (ECC -> hybrid -> PQC) |
| EUCC/ANSSI/BSI recommend hybrid | Some governments ban hybrid |
| Protects against undiscovered PQC weaknesses | Once one component broken, security properties degrade |

**Current consensus:** Hybrid key exchange is settled (X25519+ML-KEM-768 deployed at scale). Hybrid *signatures* remain deeply contentious.

### 2. Composite vs. Non-Composite Certificates

| Composite (Single cert, combined keys/sigs) | Non-Composite (Separate certs, protocol-level binding) |
|--------------------------------------------|-------------------------------------------------------|
| Protocol backwards-compatible | Keeps cert tooling simple |
| Atomic revocation of all components | More flexible -- independent algorithm lifecycle |
| Prevents cross-protocol key reuse | NSA/CNSA 2.0 preferred approach (RFC 9763) |
| ANSSI/BSI aligned | Moves complexity to management layer |
| LAMPS WG primary focus | Risk: attackers may separate/combine certs unexpectedly |

**DJB Escalation:** 6-part blog series (Oct 2025-Feb 2026) alleging NSA is spending money to push non-hybrid standards at IETF, drawing parallels to historical weak crypto influence. Was moderated from TLS mailing list during a Last Call period.

### 3. PQ Signature Size Crisis

| Protocol | Impact |
|----------|--------|
| **TLS** | ML-DSA adds ~17 KB; exceeds TCP initcwnd; extra round-trip |
| **DNSSEC** | SLH-DSA 7,856 bytes vs ECDSA 64 bytes (123x increase); exceeds UDP MTU |
| **General** | No pre-Q-day security benefit for signatures (only active attack risk) |

**Proposed Mitigations:**
- Merkle Tree Certificates (most promising -- <800 byte proofs)
- KEMTLS (replace handshake signatures with KEM-based auth -- research stage)
- FN-DSA / FALCON (smaller signatures but FIPS 206 delayed)
- Certificate compression
- Merkle Tree Ladder mode for DNSSEC
- QNAME-Based Fragmentation for DNS

### 4. Lattice Security Concerns

No practical break of ML-KEM or ML-DSA announced, but active hedging:
- X25519+ML-KEM-768 (not ML-KEM-512) chosen as hedge against lattice cryptanalysis improvements
- HQC selected as backup KEM (code-based, not lattice-based)
- NIST additional signatures competition provides non-lattice diversity
- Community maintains caution due to relative youth of lattice cryptanalysis

---

## Real-World Deployment Status

### Key Exchange (Largely Deployed)

| Vendor | Status |
|--------|--------|
| **Cloudflare** | 60% of human TLS traffic uses hybrid ML-KEM. PQ automatic for all origins by June 2026. |
| **Chrome** | X25519+ML-KEM enabled by default since Aug 2023 |
| **Firefox** | Testing hybrid X25519+Kyber-768 with Cloudflare |
| **AWS** | ML-KEM PQ TLS in KMS, ACM, Secrets Manager |
| **OpenSSH** | sntrup761x25519 default for ~5 years |

### Signatures / Certificates (NOT Yet Deployed)

- No public post-quantum certificates in use as of early 2026
- First PQ certificates expected in 2026; broad browser trust unlikely before 2027
- Cloudflare will add ML-DSA certificate support when CAs support them (estimated 2026)
- MTC rollout phases: feasibility now, CT Log onboarding Q1 2027, Chrome CQRS Q3 2027

---

## Regulatory Drivers

| Regulation | Requirement |
|-----------|-------------|
| **US EO 14144** (Jan 2025) | Agencies must list PQC-ready product categories, mandate support within 90 days |
| **NSA CNSA 2.0** | Software/firmware signing: exclusive ML-DSA by 2030; web/cloud: exclusive ML-KEM-1024 by 2033 |
| **Germany BSI** | PQC for critical infrastructure by June 2026 |
| **France ANSSI** | Recommends hybrid approaches during transition |

---

## Master Status Table

| # | Draft | WG/Area | Status | Track | Maturity |
|---|-------|---------|--------|-------|----------|
| 1 | draft-ietf-tls-ecdhe-mlkem | TLS | WG, v-04 | Standards | **Production** |
| 2 | draft-ietf-tls-mlkem | TLS | IETF Last Call (controversial) | Informational | High |
| 3 | draft-ietf-tls-mldsa | TLS | WG, v-01 | Informational | Medium |
| 4 | draft-ietf-tls-hybrid-design | TLS | WG, v-16 | Informational | High |
| 5 | draft-ietf-plants-merkle-tree-certs | PLANTS | WG, v-02 | Standards | Medium |
| 6 | draft-ietf-tls-key-share-prediction | TLS | WG, v-03 | Standards | Medium-High |
| 7 | draft-reddy-tls-slhdsa | TLS | Individual, adoption call | Standards | Low |
| 8 | draft-reddy-tls-composite-mldsa | TLS | Individual, likely expired | Standards | Stalled |
| 9 | draft-farrell-tls-pqg | TLS | Individual, v-04 | BCP | Medium |
| 10 | draft-ietf-lamps-dilithium-certificates | LAMPS | **IESG Approved** | Standards | Near-done |
| 11 | draft-ietf-lamps-kyber-certificates | LAMPS | **IESG Approved** | Standards | Near-done |
| 12 | draft-ietf-lamps-cms-kyber | LAMPS | IESG ballot | Standards | Near-done |
| 13 | draft-ietf-lamps-pq-composite-kem | LAMPS | IESG evaluation | Standards | Late |
| 14 | draft-ietf-lamps-pq-composite-sigs | LAMPS | WG, v-15, OIDs allocated | Standards | Mid |
| 15 | draft-ietf-lamps-x509-shbs | LAMPS | IESG evaluation | Standards | Late |
| 16 | draft-ietf-lamps-cms-sphincs-plus | LAMPS | **RFC 9814** | Standards | **DONE** |
| 17 | draft-ietf-lamps-cms-kemri | LAMPS | **RFC 9629** | Standards | **DONE** |
| 18 | draft-ietf-lamps-cert-binding-for-multi-auth | LAMPS | **RFC 9763** | Standards | **DONE** |
| 19 | draft-ietf-lamps-rfc4210bis | LAMPS | **RFC 9810** | Standards | **DONE** |
| 20 | draft-bonnell-lamps-chameleon-certs | LAMPS | Individual, v-07 | -- | Early |
| 21 | draft-ietf-ipsecme-ikev2-mlkem | IPSECME | WG, v-04 | Standards | High |
| 22 | draft-ietf-ipsecme-ikev2-pqc-auth | IPSECME | WG Last Call passed, v-06 | Standards | High |
| 23 | draft-hu-ipsecme-pqt-hybrid-auth | IPSECME | Individual, v-04 | Standards | Medium |
| 24 | draft-wang-ipsecme-hybrid-kem-ikev2-frodo | IPSECME | Adoption call (20+ support) | -- | Medium |
| 25 | draft-ietf-sshm-mlkem-hybrid-kex | SSHM | **IETF Last Call complete** | Standards | Very High |
| 26 | draft-ietf-sshm-ntruprime-ssh | SSHM | **IESG Approved (Informational)** | Informational | **DONE** |
| 27 | draft-ietf-cose-dilithium | COSE | WG, v-11 | Standards | High |
| 28 | draft-ietf-cose-sphincs-plus | COSE | WG, v-06 | Standards | Medium-High |
| 29 | draft-ietf-jose-pqc-kem | JOSE | WG, v-05 | Standards | Medium-High |
| 30 | draft-reddy-cose-jose-pqc-hybrid-hpke | COSE | Individual, v-11 | -- | Medium |
| 31 | draft-ietf-openpgp-pqc | OpenPGP | **IESG Approved (Proposed Standard)** | Standards | **DONE** |
| 32 | draft-ietf-mls-combiner | MLS | WG, v-01 | Standards | Low-Medium |
| 33 | draft-connolly-cfrg-xwing-kem | CFRG | Individual, v-09 | Informational | Medium |
| 34 | draft-fluhrer-lms-more-parm-sets | CFRG | IRSG evaluation, v-19 | Informational | High |

---

*Generated 2026-03-04. Sources: IETF Datatracker, Cloudflare blog, DJB blog series, LWN, PKI Consortium, NIST CSRC, various WG mailing lists.*
