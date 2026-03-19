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
7. [HPKE](#hpke)
8. [COSE / JOSE](#cose--jose)
9. [OpenPGP](#openpgp)
10. [MLS (Messaging Layer Security)](#mls-messaging-layer-security)
11. [EAP (Extensible Authentication Protocol)](#eap-extensible-authentication-protocol)
12. [EDHOC / LAKE](#edhoc--lake)
13. [ACME](#acme)
14. [DNSSEC](#dnssec)
15. [UTA (Using TLS in Applications)](#uta-using-tls-in-applications)
16. [CFRG (Crypto Forum Research Group)](#cfrg-crypto-forum-research-group)
17. [PQUIP Working Group](#pquip-working-group)
18. [Non-IETF PQC Standards](#non-ietf-pqc-standards)
19. [Major Cross-WG Debates](#major-cross-wg-debates)
20. [Real-World Deployment Status](#real-world-deployment-status)
21. [Regulatory Drivers](#regulatory-drivers)

---

## Executive Summary

| Area | Readiness | Key Takeaway |
|------|-----------|--------------|
| **KEM Key Exchange** | PRODUCTION READY | X25519+ML-KEM-768 deployed on ~60% of web traffic |
| **PQ Signatures in TLS** | MAJOR UNSOLVED PROBLEM | 14-39 KB handshake overhead; no production solution |
| **PKI / Certificates** | 70-80% READY | Core algorithm bindings published as RFCs; composite debate ongoing |
| **IPsec / IKEv2** | ADVANCING WELL | ML-KEM adopted, PQC auth passed WG Last Call |
| **SSH** | FURTHEST AHEAD | NTRU Prime default in OpenSSH 5+ years; ML-KEM at IETF Last Call |
| **OpenPGP** | IESG APPROVED | Most advanced PQ draft in IETF -- composite approach |
| **HPKE** | PROGRESSING | PQ/hybrid KEMs for HPKE at v-04, WG document |
| **COSE/JOSE** | PROGRESSING | Signature drafts mature; KEM progressing; hybrid HPKE lagging |
| **MLS** | EARLY STAGE | Innovative combiner approach; WG adopted |
| **EAP** | EARLY STAGE | PQC EAP-TLS and EAP-AKA' drafts emerging for enterprise/telecom |
| **EDHOC (LAKE)** | EARLY STAGE | PQ cipher suites and KEM auth for constrained IoT |
| **ACME** | ADVANCING | Profiles mechanism in production (Let's Encrypt); profile sets for PQC transition |
| **DNSSEC** | RESEARCH | Sig sizes far exceed UDP limits; strategy and research agenda drafts |

**Bottom line:** KEM-based key exchange migration is well underway and largely successful. Signature/authentication migration remains the critical unsolved challenge, primarily due to the size impact of post-quantum signatures on latency-sensitive protocols. The ecosystem is broadening with PQC work now active in HPKE, EAP, EDHOC, ACME, and DNSSEC.

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

### draft-ietf-tls-cert-abridge (Certificate Compression / Abridged Certs)
**Status:** WG Document, v-02

Introduces TLS certificate compression using a shared dictionary of WebPKI root and intermediate certificates, eliminating them from the handshake chain to reduce PQ certificate overhead.

| Pros | Cons |
|------|------|
| Addresses critical bandwidth challenge of PQC certificate chains | Requires clients and servers to maintain synchronized dictionaries |
| Dictionary-based approach provides better compression than general-purpose algorithms | Benefits diminish if leaf certificates grow significantly with PQC |
| Applicable beyond TLS to CT logs and other PKI storage systems | Complementary but not sufficient alone for PQ sig size problem |
| Treats CAs and website operators equitably | |

---

### draft-sheffer-tls-pqc-continuity (PQC Downgrade Protection)
**Status:** Individual, v-01 (Mar 2026)

Introduces a TLS extension allowing peers to cache commitments about PQC/composite certificate support for a specified duration, preventing downgrade attacks during PQC migration. Inspired by HSTS.

| Pros | Cons |
|------|------|
| Addresses real downgrade vulnerability during PQC transition | Individual draft, not WG-adopted |
| HSTS-inspired design leverages proven pattern | Requires clients to maintain state/cache for visited servers |
| Works at TLS layer regardless of application protocol | May create issues if cached commitments become stale |
| Protects both client and server certificate scenarios | |

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
| **RFC 9802** | HSS/LMS and XMSS for X.509 | Stateful hash-based sigs in certificates |
| **RFC 9810** | CMP v3 with KEM Support | PKI enrollment infrastructure for PQC |
| **RFC 9814** | SLH-DSA in CMS | First PQC CMS signature RFC |
| **RFC 9881** | ML-DSA for X.509 | Primary NIST PQC sig for certificates |
| **RFC 9882** | ML-DSA in CMS | CMS counterpart to RFC 9881 |
| **RFC 9909** | SLH-DSA for X.509 | Conservative hash-based sig for certificates |

### Near-Complete (IESG Approved)

| Draft | Status | Notes |
|-------|--------|-------|
| **draft-ietf-lamps-kyber-certificates** | IESG Approved, in RFC Editor queue | ML-KEM algorithm IDs for X.509 |

### In Progress

| Draft | Status | Notes |
|-------|--------|-------|
| **draft-ietf-lamps-cms-kyber** v-13 | IESG ballot active | ML-KEM in CMS |
| **draft-ietf-lamps-pq-composite-kem** v-12 | IESG evaluation | Composite ML-KEM + traditional |
| **draft-ietf-lamps-pq-composite-sigs** v-15 | WG Draft, OIDs allocated | Composite ML-DSA + traditional |
| **draft-ietf-lamps-cms-composite-sigs** v-04 | Submitted to IESG | Composite ML-DSA in CMS |

### FN-DSA (FALCON) Drafts

| Draft | Status | Notes |
|-------|--------|-------|
| **draft-turner-lamps-fn-dsa-certificates** v-00 | Individual (Nov 2025) | FN-DSA algorithm IDs for X.509 |
| **draft-turner-lamps-cms-fn-dsa** v-00 | Individual (Nov 2025) | FN-DSA for CMS signatures |

### draft-ietf-lamps-cms-composite-sigs (Composite ML-DSA in CMS)
**Status:** Submitted to IESG for Publication, v-04 (Feb 2026), Proposed Standard

Specifies conventions for using Composite ML-DSA algorithms (ML-DSA combined with RSA, ECDSA, or EdDSA) within CMS.

| Pros | Cons |
|------|------|
| Advanced standardization stage (IESG publication requested) | Double signature overhead significantly increases message size |
| Composite provides security if either algorithm is broken | Implementation complexity managing two signature algorithms |
| Enables gradual migration (legacy verifies traditional, PQ-aware verifies both) | Composite debate ongoing -- necessity questioned |
| Leverages mature CMS infrastructure (S/MIME, signed firmware) | |

### FN-DSA Certificates and CMS -- Pros/Cons

| Pros | Cons |
|------|------|
| FN-DSA offers smallest PQ signature sizes (~690/1,233 bytes) | FIPS 206 not yet finalized -- spec may change |
| Enables early adoption path for upcoming NIST standard | Individual drafts, uncertain standardization timeline |
| Alternative lattice-based sig diversifying PQC portfolio | Adding another sig algorithm increases ecosystem fragmentation |
| Completes PKI ecosystem coverage (X.509 + CMS) | |

### draft-reddy-lamps-x509-pq-commit (PQC Hosting Continuity)
**Status:** Individual, v-01 (Feb 2026)

Introduces X.509 certificate extension enabling subjects to signal intent to maintain PQC/composite certificates beyond standard expiration, helping relying parties detect downgrade and MITM attacks during PQC migration.

| Pros | Cons |
|------|------|
| Addresses downgrade attack vulnerability during PQC transition | Individual draft, uncertain standardization path |
| Certificate-embedded commitment is cryptographically authenticated | Adds complexity to certificate issuance workflows |
| Non-critical extension maintains backward compatibility | Operational burden if organizations can't maintain commitments |
| Complementary to TLS-layer approaches (draft-sheffer) | |

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

### draft-reddy-ipsecme-ikev2-hybrid-reliable (Composite KE + TCP for IKEv2)
**Status:** Individual, v-00 (Jan 2026)

Proposes PQ/traditional hybrid composite key exchange algorithms for IKEv2 with vetted pairings (ecp256-mlkem768, ecp384-mlkem1024, curve25519-mlkem768) and TCP transport to prevent IP fragmentation of large PQC key material.

| Pros | Cons |
|------|------|
| Combined KE payload reduces message count vs separate exchanges | TCP fallback adds implementation complexity and attack surface |
| TCP transport directly solves IP fragmentation problem | Individual draft, not WG-adopted |
| Vetted algorithm pairings reduce configuration errors | Composite approach locks in specific pairings |
| Pathway for pure PQ IKEv2 when traditional algorithms obsolete | |

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

### draft-harrison-sshm-mlkem (Standalone ML-KEM for SSH)
**Status:** Individual, v-01 (Dec 2025)

Proposes pure ML-KEM key exchange methods (mlkem512, mlkem768, mlkem1024) for SSH without hybrid traditional algorithm combinations. Authored by Cisco and AWS.

| Pros | Cons |
|------|------|
| Pure PQ simplifies implementation vs hybrid | Individual draft, not WG-adopted |
| Required for eventual CNSA 2.0 pure-PQ compliance | No hybrid fallback if ML-KEM vulnerability discovered |
| Cisco/AWS authorship signals industry deployment interest | Competes with hybrid approach for adoption priority |
| Three security levels for flexible risk tolerance | |

---

## HPKE

### draft-ietf-hpke-pq (PQ/Hybrid KEMs for HPKE)
**Status:** WG Document, v-04 (Mar 2026), Standards Track

Extends HPKE (RFC 9180) with post-quantum KEMs: pure ML-KEM variants (512/768/1024) and hybrid PQ/traditional combinations (ML-KEM + P-256, X25519, P-384), plus SHA-3 based KDFs (SHAKE, TurboSHAKE).

| Pros | Cons |
|------|------|
| Both pure PQ and hybrid options for different risk profiles | Hybrid combinations increase implementation complexity |
| Leverages widely-implemented HPKE framework | Larger keys/ciphertexts impact bandwidth-constrained applications |
| Comprehensive test vectors and JSON specs for interop | 9 algorithm combinations may fragment deployment |
| SHA-3 KDF options provide quantum-resistant key derivation | Foundation draft -- many protocols depend on this completing |

**Significance:** This is a foundational draft. HPKE is used across JOSE, COSE, MLS, and other protocols. PQ HPKE unblocks PQ support in all downstream protocols.

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

### draft-skokan-jose-hpke-pq-pqt (PQ/T Algorithm Registrations for JOSE)
**Status:** Individual, v-03 (Feb 2026)

Registers post-quantum and hybrid PQ/traditional algorithm identifiers for JSON Web Encryption using the HPKE framework. Defines 12 algorithm combinations (hybrid HPKE-8 through HPKE-13, pure PQ HPKE-14 through HPKE-17) plus key encryption variants.

| Pros | Cons |
|------|------|
| Brings PQC to widely-used JOSE/JWE ecosystem (web tokens, API security) | Individual draft, may face adoption challenges |
| Comprehensive coverage: both hybrid and pure PQ options | 12+ algorithm registrations could create selection confusion |
| Leverages proven HPKE framework rather than new constructions | JWE overhead + PQC key sizes may challenge size-constrained use (cookies, URL tokens) |
| JSON Web Key representation using AKP key type integrates cleanly | |

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

## EAP (Extensible Authentication Protocol)

### draft-reddy-emu-pqc-eap-tls (PQC in EAP-TLS)
**Status:** Individual, v-02 (Jan 2026)

Proposes modifications to EAP-TLS and EAP-TTLS to incorporate PQC, addressing challenges with large PQ certificate sizes and extended certificate chains in enterprise and wireless environments. Builds on RFC 9191 (large certs in EAP).

| Pros | Cons |
|------|------|
| Protects widely-deployed enterprise authentication (802.1X, WPA-Enterprise) | Individual draft, limited deployment momentum |
| Addresses specific PQ cert size challenges identified in RFC 9191 | Large PQ cert chains may exceed MTU in wireless environments |
| Covers both EAP-TLS and EAP-TTLS | Requires coordinated upgrades across RADIUS servers, authenticators, and supplicants |
| High-value enterprise targets vulnerable to HNDL attacks | |

### draft-ietf-emu-pqc-eapaka (PQ KEMs for EAP-AKA')
**Status:** WG Document, v-01 (Feb 2026)

Enhances EAP-AKA' Forward Secrecy with quantum-resistant cryptography by integrating ML-KEM (512/768/1024) through new AT_PUB_KEM and AT_KEM_CT attributes, with attribute-level fragmentation for large key material.

| Pros | Cons |
|------|------|
| WG document indicates EMU consensus on need for PQC in cellular auth | Fragmentation mechanism adds protocol complexity |
| Maintains backward compatibility for gradual deployment | Limited to EAP-AKA' (not broader EAP-AKA family) |
| Attribute-level fragmentation addresses cellular MTU limitations | Cellular infrastructure upgrade cycles are slow |
| Protects cellular authentication against quantum threats | |

**Significance:** Critical for 3GPP/5G PQC migration. Connects IETF PQC work to telecom infrastructure.

---

## EDHOC / LAKE

### draft-spm-lake-pqsuites (PQ Cipher Suites for EDHOC)
**Status:** Individual, v-01 (Oct 2025)

Introduces post-quantum cipher suites for EDHOC using ML-DSA for digital signatures and ML-KEM for key exchange. Supports both signature-based and PSK authentication methods. Ericsson authorship.

| Pros | Cons |
|------|------|
| Brings PQC to EDHOC for IoT/CoAP ecosystems | ML-KEM-512 and ML-DSA-44 introduce "significantly higher overhead" for constrained devices |
| Major IoT vendor (Ericsson) authorship | Individual draft, may slow IoT ecosystem adoption |
| Both signature and PSK auth modes for flexibility | EDHOC itself still relatively new protocol |
| Aligns with NIST-standardized algorithms | |

### draft-lake-pocero-authkem-edhoc (KEM Auth for EDHOC)
**Status:** Individual, v-00 (Oct 2025)

Extends EDHOC with KEM-based authentication method enabling signature-free PQ authentication using ML-KEM. Five-message handshake with ephemeral and static KEM key pairs. Academic origin (R.C. ATHENA).

| Pros | Cons |
|------|------|
| KEM-based auth avoids large PQ signature overhead | Five-message handshake increases round-trips |
| Signature-free reduces computational burden on IoT processors | Academic origin, may lack industry deployment momentum |
| Maintains forward secrecy and mutual authentication | Static KEM keys require different key management than signature PKI |
| Alternative to signature-based approach for constrained devices | Very early stage (v-00) |

---

## ACME

### draft-ietf-acme-profiles (ACME Profiles)
**Status:** WG Document, v-01 (Mar 2026)

Defines mechanism enabling ACME servers to offer different certificate profiles and allowing clients to select preferred profiles during ordering. Moves profile selection from CSR fields into Order resource.

| Pros | Cons |
|------|------|
| **Already in production** (Let's Encrypt, Boulder, Pebble, Certbot, Lego, Caddy) | Profile proliferation may create confusion |
| Simplifies PQC certificate issuance via standardized profile selection | Requires ACME server and client upgrades |
| Reduces security risks by avoiding complex CSR parsing | |
| Strong standardization momentum with multiple implementations | |

**Significance:** This is the key enabler for automated PQC certificate issuance. Without profiles, ACME clients have no standard way to request PQC certificates.

### draft-davidben-acme-profile-sets (ACME Profile Sets)
**Status:** Individual, v-00 (Oct 2025), candidate for ACME WG

Defines mechanism for ACME servers to indicate collections of related certificate profiles, enabling automatic obtainment of multiple certificates to support applications serving both legacy and PQ-capable clients. Google authorship.

| Pros | Cons |
|------|------|
| Directly addresses PQC transition challenge (serve legacy + PQ clients simultaneously) | Individual draft, not yet WG-adopted |
| Automatic multi-certificate issuance reduces operational burden | Managing multiple certificates per domain increases complexity |
| Profile set concept enables coordinated updates during renewal | Could significantly increase CA workload |
| Google authorship suggests strong standardization potential | |

---

## DNSSEC

### draft-sheth-pqc-dnssec-strategy (PQC DNSSEC Strategy)
**Status:** Individual, v-00 (Oct 2025)

Proposes deploying two categories of PQ algorithms for DNSSEC -- conservatively designed algorithms (potentially using Merkle Tree Ladder mode) and low-impact drop-in alternatives. Verisign Labs authorship.

| Pros | Cons |
|------|------|
| Dual-algorithm strategy balances conservative security with practicality | Individual draft, no DNSOP WG consensus yet |
| Merkle Tree Ladder mode addresses DNSSEC sig size challenge | Dual algorithm deployment increases infrastructure complexity |
| Verisign Labs + academic authorship (operational + research) | PQC sig sizes fundamentally challenge UDP transport |
| Fallback mechanism provides resilience if low-impact algorithm breaks | |

### draft-fregly-research-agenda-for-pqc-dnssec (PQC DNSSEC Research Agenda)
**Status:** Individual, v-02, **expired** (Dec 2024)

Proposed collaborative research framework examining how PQ signatures will impact DNSSEC operations. Concluded that either DNS protocol enhancements or shift away from UDP (or combination) will be needed.

| Pros | Cons |
|------|------|
| Research-oriented approach acknowledges fundamental uncertainties | Expired -- lack of continued development |
| Multi-institution collaboration (6 authors) | Research agenda rather than solution |
| Identified critical question about UDP viability for PQC DNSSEC | Conclusion that major DNS changes needed is concerning for timeline |

---

## UTA (Using TLS in Applications)

### draft-ietf-uta-pqc-app (PQC Recommendations for TLS Applications)
**Status:** WG Document, v-01 (Feb 2026)

Provides best practices for device manufacturers and application developers implementing PQC in TLS-based applications. Covers hybrid/pure PQ key exchange, authentication mechanisms, certificate optimization, DNS encryption, and operational guidance.

| Pros | Cons |
|------|------|
| Comprehensive guidance covering entire TLS ecosystem | Best practices may become outdated quickly as PQC matures |
| Addresses practical deployment challenges (cert size optimization) | Balancing hybrid vs pure PQ recs may create confusion |
| Provides migration timeline recommendations | Heavy focus on cert optimization confirms PQ cert size pain points |
| Covers both confidentiality (KEX) and authentication (certs/sigs) | |

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

## PQUIP Working Group

### draft-ietf-pquip-pqc-engineers (PQC for Engineers)
**Status:** WG Adopted

Engineer-focused PQC primer providing practical guidance for protocol implementers.

### draft-ietf-pquip-pqc-hsm-constrained (PQC for HSMs/Constrained Devices)
**Status:** WG Document, v-03 (Jan 2026), **Waiting for Write-Up**

Provides guidance for integrating PQC into resource-limited devices (IoT nodes, lightweight HSMs). Covers seed-based key generation, ephemeral key handling, cryptographic task offloading, and firmware update considerations.

| Pros | Cons |
|------|------|
| WG consensus indicates broad recognition of constrained device challenges | Constrained devices may still struggle even with optimizations |
| Covers all three NIST algorithms (ML-KEM, ML-DSA, SLH-DSA) | Seed-based approaches introduce key management complexity |
| Practical optimization techniques (lazy expansion, pre-hashing, seed storage) | Firmware update authentication creates circular dependency during PQC transition |
| Addresses critical deployment barrier for IoT PQC adoption | |

### draft-reddy-pquip-pqc-signature-migration (PQC Signature Migration)
**Status:** Individual, v-01 (Oct 2025)

Evaluates three deployment models for PQ signature migration: composite certificates, dual certificates, and PQC-only certificates. Nokia, Citrix, and Zscaler authorship.

| Pros | Cons |
|------|------|
| Directly addresses critical signature migration decision | Individual draft, limited authority vs WG consensus |
| Comparative evaluation helps informed deployment decisions | Three-way comparison may not provide clear recommendation |
| Enterprise deployment perspective (Nokia, Citrix, Zscaler) | Rapid PQC evolution may quickly date analysis |

### draft-kwiatkowski-pquip-pqc-migration (PQC Migration Guidance)
**Status:** Individual, **expired** (Jul 2025)

General PQC migration framework for protocol designers, developers, and implementers.

| Pros | Cons |
|------|------|
| Comprehensive migration guidance targeting multiple stakeholders | Expired, likely superseded by newer PQUIP drafts |
| Addressed full protocol design lifecycle | Content may be outdated |

### draft-li-pquip-teleco-pqc-migration (Telecom PQC Migration)
**Status:** Individual, v-01, **expired** (Jul 2025)

Addressed PQC readiness in telecommunications infrastructure, examining 3GPP-governed telecom networks and proposing migration strategies. Huawei authorship.

| Pros | Cons |
|------|------|
| Focused on critical telecom infrastructure | Expired, possible lack of community interest |
| Integration perspective between 3GPP and IETF standards | 3GPP may be driving telecom PQC independently |
| Major telecom vendor perspective (Huawei) | |

### draft-prabel-pquip-pqc-guidance (PQC Deployment Guidance)
**Status:** Individual, v-01 (Oct 2025)

Comprehensive overview of PQ algorithms covering KEMs (ML-KEM, FrodoKEM, Classic McEliece, HQC, NTRU) and digital signatures (ML-DSA, FN-DSA, SLH-DSA, LMS, XMSS). Includes algorithm parameters and distinguishing features.

| Pros | Cons |
|------|------|
| Comprehensive algorithm catalog including NIST and alternatives | Individual draft, limited authoritative weight |
| Unified presentation format enables direct algorithm comparison | Including non-NIST algorithms may create confusion |
| Broader coverage than NIST-only focus | Algorithm landscape evolving rapidly |

### Other PQUIP Documents

| Draft | Status | Notes |
|-------|--------|-------|
| **draft-hale-pquip-hybrid-signature-spectrums** | Active | Design goals/security analysis for hybrid sigs |
| **draft-vaira-pquip-pqc-use-cases** | Active | Migration strategies |

---

## Non-IETF PQC Standards

### NIST Special Publications

| Document | Status | Significance |
|----------|--------|-------------|
| **SP 800-227** | Final (Nov 2025) | Recommendations for Key-Encapsulation Mechanisms. Defines guidance for hybrid/composite KEM usage. Key for FIPS certification of hybrid approaches. |
| **SP 800-131A Rev.3** | Draft | Transitioning use of crypto algorithms. Updated with PQC transition timelines and deprecation schedules. |
| **IR 8547** | Final (Feb 2025) | Transition to Post-Quantum Cryptography Standards. Formal migration planning guide with 15-year classical crypto phase-out roadmap. |

### ETSI

| Document | Status | Significance |
|----------|--------|-------------|
| **TS 103 744** | Published | Quantum-Safe Hybrid Key Exchanges. Framework for hybrid approaches used across European deployments. |
| **TS 104 015** | In development | Migration Strategies for PQC. Practical roadmap for telecom operators. |
| **TR 103 619** | Published | Quantum-Safe VPN. PQC integration guidance for VPN technologies. |
| **GR QSC 001-007** | Various | QSC group reports covering threat analysis, implementation, and migration. |

### ISO/IEC

| Document | Status | Significance |
|----------|--------|-------------|
| **ISO/IEC 18033-2 Amd** | In development | Adding ML-KEM to encryption algorithms standard. International standardization. |
| **ISO/IEC 14888** | In development | Adding ML-DSA/SLH-DSA to digital signature standard. |

### 3GPP

| Document | Status | Significance |
|----------|--------|-------------|
| **TR 33.703** | Study phase | Study on PQC for 3GPP systems. Evaluates impact on 5G/6G auth, key agreement, and protocol signaling. Connects to IETF EAP-AKA' PQC work. |

### Industry Standards

| Organization | Document | Status | Significance |
|-------------|----------|--------|-------------|
| **W3C** | Web Crypto API Modern Algorithms | Proposal stage | Adding ML-KEM, ML-DSA to Web Cryptography API for browser PQC. |
| **OASIS** | KMIP 3.0 | Published | Key Management Interoperability Protocol with PQC algorithm support. |
| **OASIS** | PKCS#11 v3.2 | In development | Cryptographic token interface adding ML-KEM, ML-DSA, SLH-DSA mechanism types. Critical for HSM interoperability. |
| **TCG** | TPM 2.0 Library Spec V185 | Published | TPM 2.0 with PQC algorithm support (ML-KEM, ML-DSA). Hardware root-of-trust PQC readiness. |
| **IEEE** | P802.11 PQC Study Group | Active (2025) | Studying PQC impact on Wi-Fi (802.11) authentication and key exchange. SAE/OWE implications. |

### National Agency Guidance

| Agency | Document | Status | Significance |
|--------|----------|--------|-------------|
| **BSI** (Germany) | TR-02102-1 | Updated 2025 | Crypto recommendations including PQC. Mandates PQC for critical infrastructure by June 2026. |
| **ANSSI** (France) | PQC Position Papers | Published (2022, updated 2024) | Recommends hybrid approaches. Composite sigs preferred. Three-phase migration timeline through 2030. |
| **EU** | PQC Roadmap | In development | Coordinated member-state PQC migration under EU Cybersecurity Act. |

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
| **EAP** | PQ cert chains exceed wireless MTU limits; require fragmentation |
| **EDHOC** | ML-DSA "significantly higher overhead" for constrained IoT devices |
| **General** | No pre-Q-day security benefit for signatures (only active attack risk) |

**Proposed Mitigations:**
- Merkle Tree Certificates (most promising -- <800 byte proofs)
- KEMTLS (replace handshake signatures with KEM-based auth -- research stage)
- FN-DSA / FALCON (smaller signatures but FIPS 206 delayed)
- Certificate compression (draft-ietf-tls-cert-abridge)
- Merkle Tree Ladder mode for DNSSEC
- QNAME-Based Fragmentation for DNS
- KEM-based auth for EDHOC (draft-lake-pocero-authkem-edhoc)

### 4. Lattice Security Concerns

No practical break of ML-KEM or ML-DSA announced, but active hedging:
- X25519+ML-KEM-768 (not ML-KEM-512) chosen as hedge against lattice cryptanalysis improvements
- HQC selected as backup KEM (code-based, not lattice-based)
- NIST additional signatures competition provides non-lattice diversity
- Community maintains caution due to relative youth of lattice cryptanalysis

### 5. Downgrade Protection During PQC Transition

Emerging concern: how to prevent MITM attackers from stripping PQC support during the transition period when servers support both traditional and PQC certificates.

| Approach | Draft | Mechanism |
|----------|-------|-----------|
| TLS-layer caching | draft-sheffer-tls-pqc-continuity | HSTS-inspired TLS extension caching PQC commitments |
| Certificate-embedded commitment | draft-reddy-lamps-x509-pq-commit | X.509 extension signaling PQC intent |

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

### Certificate Automation (ACME)

- ACME Profiles mechanism already in production at Let's Encrypt
- Profile Sets draft provides multi-certificate issuance for PQC transition
- Automated PQC certificate issuance expected to follow algorithm standardization

---

## Regulatory Drivers

| Regulation | Requirement |
|-----------|-------------|
| **US EO 14144** (Jan 2025) | Agencies must list PQC-ready product categories, mandate support within 90 days |
| **NSA CNSA 2.0** | Software/firmware signing: exclusive ML-DSA by 2030; web/cloud: exclusive ML-KEM-1024 by 2033 |
| **NIST IR 8547** | Formal deprecation schedule: 15-year classical crypto phase-out roadmap |
| **NIST SP 800-227** | Recommendations for KEM usage including hybrid/composite approaches |
| **Germany BSI** | PQC for critical infrastructure by June 2026 |
| **France ANSSI** | Recommends hybrid approaches during transition; three-phase timeline through 2030 |
| **EU PQC Roadmap** | Coordinated member-state PQC migration under EU Cybersecurity Act |
| **3GPP TR 33.703** | PQC study for 5G/6G systems |

---

## Master Status Table

| # | Draft | WG/Area | Status | Track | Maturity |
|---|-------|---------|--------|-------|----------|
| 1 | draft-ietf-tls-ecdhe-mlkem | TLS | WG, v-04 | Standards | **Production** |
| 2 | draft-ietf-tls-mlkem | TLS | IETF Last Call (controversial) | Informational | High |
| 3 | draft-ietf-tls-mldsa | TLS | WG, v-01 | Informational | Medium |
| 4 | draft-ietf-tls-hybrid-design | TLS | WG, v-16 | Informational | High |
| 5 | draft-ietf-tls-cert-abridge | TLS | WG, v-02 | Standards | Medium |
| 6 | draft-sheffer-tls-pqc-continuity | TLS | Individual, v-01 | -- | Early |
| 7 | draft-ietf-plants-merkle-tree-certs | PLANTS | WG, v-02 | Standards | Medium |
| 8 | draft-ietf-tls-key-share-prediction | TLS | WG, v-03 | Standards | Medium-High |
| 9 | draft-reddy-tls-slhdsa | TLS | Individual, adoption call | Standards | Low |
| 10 | draft-reddy-tls-composite-mldsa | TLS | Individual, likely expired | Standards | Stalled |
| 11 | draft-farrell-tls-pqg | TLS | Individual, v-04 | BCP | Medium |
| 12 | RFC 9881 | LAMPS | **Published RFC** | Standards | **DONE** |
| 13 | RFC 9882 | LAMPS | **Published RFC** | Standards | **DONE** |
| 14 | RFC 9909 | LAMPS | **Published RFC** | Standards | **DONE** |
| 15 | RFC 9802 | LAMPS | **Published RFC** | Standards | **DONE** |
| 16 | draft-ietf-lamps-kyber-certificates | LAMPS | **IESG Approved** | Standards | Near-done |
| 17 | draft-ietf-lamps-cms-kyber | LAMPS | IESG ballot | Standards | Near-done |
| 18 | draft-ietf-lamps-pq-composite-kem | LAMPS | IESG evaluation | Standards | Late |
| 19 | draft-ietf-lamps-pq-composite-sigs | LAMPS | WG, v-15, OIDs allocated | Standards | Mid |
| 20 | draft-ietf-lamps-cms-composite-sigs | LAMPS | Submitted to IESG, v-04 | Standards | Late |
| 21 | draft-turner-lamps-fn-dsa-certificates | LAMPS | Individual, v-00 | -- | Early |
| 22 | draft-turner-lamps-cms-fn-dsa | LAMPS | Individual, v-00 | -- | Early |
| 23 | draft-reddy-lamps-x509-pq-commit | LAMPS | Individual, v-01 | -- | Early |
| 24 | RFC 9814 | LAMPS | **Published RFC** | Standards | **DONE** |
| 25 | RFC 9629 | LAMPS | **Published RFC** | Standards | **DONE** |
| 26 | RFC 9763 | LAMPS | **Published RFC** | Standards | **DONE** |
| 27 | RFC 9810 | LAMPS | **Published RFC** | Standards | **DONE** |
| 28 | draft-bonnell-lamps-chameleon-certs | LAMPS | Individual, v-07 | -- | Early |
| 29 | draft-ietf-ipsecme-ikev2-mlkem | IPSECME | WG, v-04 | Standards | High |
| 30 | draft-ietf-ipsecme-ikev2-pqc-auth | IPSECME | WG Last Call passed, v-06 | Standards | High |
| 31 | draft-hu-ipsecme-pqt-hybrid-auth | IPSECME | Individual, v-04 | Standards | Medium |
| 32 | draft-wang-ipsecme-hybrid-kem-ikev2-frodo | IPSECME | Adoption call (20+ support) | -- | Medium |
| 33 | draft-reddy-ipsecme-ikev2-hybrid-reliable | IPSECME | Individual, v-00 | -- | Early |
| 34 | draft-ietf-sshm-mlkem-hybrid-kex | SSHM | **IETF Last Call complete** | Standards | Very High |
| 35 | draft-ietf-sshm-ntruprime-ssh | SSHM | **IESG Approved (Informational)** | Informational | **DONE** |
| 36 | draft-harrison-sshm-mlkem | SSHM | Individual, v-01 | -- | Early |
| 37 | draft-ietf-hpke-pq | HPKE | WG, v-04 | Standards | Medium-High |
| 38 | draft-ietf-cose-dilithium | COSE | WG, v-11 | Standards | High |
| 39 | draft-ietf-cose-sphincs-plus | COSE | WG, v-06 | Standards | Medium-High |
| 40 | draft-ietf-jose-pqc-kem | JOSE | WG, v-05 | Standards | Medium-High |
| 41 | draft-reddy-cose-jose-pqc-hybrid-hpke | COSE | Individual, v-11 | -- | Medium |
| 42 | draft-skokan-jose-hpke-pq-pqt | JOSE | Individual, v-03 | -- | Early |
| 43 | draft-ietf-openpgp-pqc | OpenPGP | **IESG Approved (Proposed Standard)** | Standards | **DONE** |
| 44 | draft-ietf-mls-combiner | MLS | WG, v-01 | Standards | Low-Medium |
| 45 | draft-reddy-emu-pqc-eap-tls | EMU | Individual, v-02 | -- | Early |
| 46 | draft-ietf-emu-pqc-eapaka | EMU | WG, v-01 | Standards | Early |
| 47 | draft-spm-lake-pqsuites | LAKE | Individual, v-01 | -- | Early |
| 48 | draft-lake-pocero-authkem-edhoc | LAKE | Individual, v-00 | -- | Very Early |
| 49 | draft-ietf-acme-profiles | ACME | WG, v-01 | Standards | **Production** |
| 50 | draft-davidben-acme-profile-sets | ACME | Individual, v-00 | -- | Early |
| 51 | draft-sheth-pqc-dnssec-strategy | DNSOP | Individual, v-00 | -- | Early |
| 52 | draft-fregly-research-agenda-for-pqc-dnssec | DNSOP | Individual, expired | -- | Stalled |
| 53 | draft-ietf-uta-pqc-app | UTA | WG, v-01 | -- | Early |
| 54 | draft-connolly-cfrg-xwing-kem | CFRG | Individual, v-09 | Informational | Medium |
| 55 | draft-fluhrer-lms-more-parm-sets | CFRG | IRSG evaluation, v-19 | Informational | High |
| 56 | RFC 9794 | PQUIP | **Published RFC** | Informational | **DONE** |
| 57 | draft-ietf-pquip-pqc-engineers | PQUIP | WG Adopted | Informational | Medium |
| 58 | draft-ietf-pquip-pqc-hsm-constrained | PQUIP | WG, v-03, waiting write-up | Informational | Medium-High |
| 59 | draft-reddy-pquip-pqc-signature-migration | PQUIP | Individual, v-01 | -- | Early |
| 60 | draft-prabel-pquip-pqc-guidance | PQUIP | Individual, v-01 | -- | Early |

---

*Generated 2026-03-04. Sources: IETF Datatracker, Cloudflare blog, DJB blog series, LWN, PKI Consortium, NIST CSRC, ETSI, ISO, 3GPP, various WG mailing lists.*
