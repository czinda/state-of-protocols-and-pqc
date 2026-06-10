# IETF Post-Quantum Cryptography: Comprehensive Status Report
**Date:** 2026-06-10
**Source:** https://github.com/ietf-wg-pquip/state-of-protocols-and-pqc (forked)

---

## Table of Contents
1. [Executive Summary](#executive-summary)
2. [Changes Since March 2026](#changes-since-march-2026)
3. [NIST Standards Status](#nist-standards-status)
4. [Composite & Hybrid — Deep Dive](#composite--hybrid--deep-dive)
5. [TLS](#tls)
6. [LAMPS / X.509 / CMS (PKI)](#lamps--x509--cms-pki)
7. [IPsec / IKEv2](#ipsec--ikev2)
8. [SSH](#ssh)
9. [HPKE](#hpke)
10. [COSE / JOSE](#cose--jose)
11. [OpenPGP](#openpgp)
12. [MLS (Messaging Layer Security)](#mls-messaging-layer-security)
13. [EAP (Extensible Authentication Protocol)](#eap-extensible-authentication-protocol)
14. [EDHOC / LAKE](#edhoc--lake)
15. [ACME](#acme)
16. [DNSSEC](#dnssec)
17. [UTA (Using TLS in Applications)](#uta-using-tls-in-applications)
18. [CFRG (Crypto Forum Research Group)](#cfrg-crypto-forum-research-group)
19. [PQUIP Working Group](#pquip-working-group)
20. [Non-IETF PQC Standards](#non-ietf-pqc-standards)
21. [Major Cross-WG Debates](#major-cross-wg-debates)
22. [Real-World Deployment Status](#real-world-deployment-status)
23. [Regulatory Drivers](#regulatory-drivers)

---

## Executive Summary

| Area | Readiness | Key Takeaway |
|------|-----------|--------------|
| **KEM Key Exchange** | PRODUCTION READY | X25519+ML-KEM-768 deployed on >65% of web traffic; ECDHE-MLKEM IESG telechat Jun 18 |
| **PQ Signatures in TLS** | WAITING FOR AD | ML-DSA TLS v-03 **passed IETF Last Call**; awaiting AD go-ahead; sig size crisis unsolved |
| **PKI / Certificates** | ~95% READY | Composite sigs + CMS composite sigs in RFC Ed Queue; composite KEM stalled 71 days; **Let's Encrypt commits to MTCs** |
| **IPsec / IKEv2** | IETF LAST CALL | ML-KEM **advanced to IETF Last Call** (ends Jun 15); PQC auth write-up in progress |
| **SSH** | RFC ED QUEUE | ML-KEM hybrid in active RFC Editor processing; NTRU Prime published (RFC 9941) |
| **OpenPGP** | AUTH48 → RFC 9980 | All authors approved. Blocked on AD editorial response. Publication imminent. |
| **COSE/JOSE** | RFC 9964 PUBLISHED | ML-DSA COSE/JOSE now published RFC; KEM progressing; hybrid HPKE lagging |
| **MLS** | EXPIRED | Combiner draft expired Apr 25; needs new revision |
| **HPKE** | PROGRESSING | PQ/hybrid KEMs for HPKE at v-04, WG document |
| **EAP** | EARLY STAGE | PQC EAP-TLS and EAP-AKA' drafts active for enterprise/telecom |
| **EDHOC (LAKE)** | EARLY STAGE | PQ cipher suites and KEM auth for constrained IoT |
| **ACME** | IN PRODUCTION | Profiles mechanism in production (Let's Encrypt); profile sets for PQC transition |
| **DNSSEC** | RESEARCH | Sig sizes far exceed UDP limits; strategy and research agenda drafts |

**Bottom line:** The IETF PQC pipeline is moving at velocity — ML-DSA in TLS passed Last Call, IKEv2 ML-KEM entered Last Call, ECDHE-MLKEM has a June 18 IESG telechat. NIST advanced 9 of 14 additional signature candidates to Round 3 (May 14). Let's Encrypt committed to Merkle Tree Certificates (Jun 3), the most significant PQ cert ecosystem signal to date. Microsoft AD CS shipped ML-DSA support (May 2026). Google and Cloudflare accelerated PQC timelines to 2029 hard deadlines. The composite KEM draft remains the key bottleneck — stalled 71+ days awaiting AD action.

---

## Changes Since May 2026

| Item | May 2026 Status | June 2026 Status | Significance |
|------|-----------------|------------------|--------------|
| **NIST Additional Sigs** | Round 2 (14 candidates) | **Round 3 (9 candidates, May 14)** | CROSS, LESS, Mirath, PERK, RYDE eliminated. Spec tweaks due Aug 14. |
| **ECDHE-MLKEM** | RFC Ed Queue (v-04) | **IESG Evaluation (v-05; telechat Jun 18)** | New version; Standards Track; obsoletes pre-standard Kyber768 |
| **ML-DSA TLS** | IETF Last Call (v-03; ends Jun 1) | **Waiting for AD Go-Ahead** | Passed LC. GENART "Ready." SECDIR "Has nits." Awaiting AD. |
| **IKEv2 ML-KEM** | AD Evaluation (v-05) | **IETF Last Call (ends Jun 15)** | TSVART: "Ready w/nits." Major advancement. |
| **TLS Hybrid Design** | RFC Ed Queue (v-16) | **AUTH48 → RFC 9954** | Assigned RFC 9954. All authors approved. Blocked on normative ref. |
| **OpenPGP PQC** | AUTH48 → RFC 9980 | AUTH48 → RFC 9980 (all authors approved) | Blocked on AD editorial response (questions #8, #12). |
| **CMS Composite KEM** | WG Last Call (v-01) | **Publication Requested** | Advanced from WG LC. Shepherd write-up updated Jun 3. |
| **Composite ML-KEM** | Publication Requested (v-14) | Publication Requested (v-14) | **Stalled 71+ days** — no telechat scheduled. Key bottleneck. |
| **Let's Encrypt** | ACME Profiles in production | **Committed to Merkle Tree Certs (Jun 3)** | Staging late 2026, production 2027. Massive ecosystem signal. |
| **Microsoft AD CS** | PQC in ADCS targeted early 2026 | **ML-DSA GA (May 2026 KB5087539)** | ML-DSA-44/65/87 for CA signing, code signing, OCSP. |
| **Apple** | PQ TLS in macOS/iOS 26 | **Corecrypto open-sourced (May 22)** | ML-KEM + ML-DSA implementations on GitHub. CryptoKit APIs. |
| **Google** | Chrome PQC default | **2029 hard deadline for full PQC migration** | Android 17 ML-DSA via KeyMint. Chrome 154 HTTPS-only default. |
| **Cloudflare** | >60% PQ traffic | **>65% PQ traffic; roadmap accelerated to 2029** | ML-DSA origin support mid-2026; MTC mid-2027. |
| **FN-DSA (FIPS 206)** | IPD pending clearance | Still stuck in DoC clearance | **NSA: FN-DSA permanently excluded from CNSA 2.0.** |
| **BSI** | CRQC horizon ~20 years | **CRQC horizon shortened to 10-15 years** | <5% of German orgs have migration plans. |

---

## NIST Standards Status

| Standard | Algorithm | Based On | Status | Published |
|----------|-----------|----------|--------|-----------|
| **FIPS 203** | ML-KEM | CRYSTALS-Kyber | **FINAL** | August 13, 2024 |
| **FIPS 204** | ML-DSA | CRYSTALS-Dilithium | **FINAL** | August 13, 2024 |
| **FIPS 205** | SLH-DSA | SPHINCS+ | **FINAL** | August 13, 2024 |
| **FIPS 206** | FN-DSA | FALCON | Stuck in DoC clearance (since Aug 2025) | Expected late 2026/early 2027 at earliest. **NSA: permanently excluded from CNSA 2.0.** |
| **HQC** | HQC | Error-correcting codes | Selected March 2025 | Draft standard ~2027 |

**Additional Signature Candidates (Round 3, selected May 14, 2026 — NIST IR 8610):** 9 candidates advanced from 14: FAEST, HAWK, MAYO, MQOM, QR-UOV, SDitH, SNOVA, SQIsign, UOV. *Eliminated:* CROSS, LESS, Mirath, PERK, RYDE. Specification tweaks due **Aug 14, 2026**. 7th PQC Standardization Conference planned spring/summer 2027. Notable: SQIsign has ~212 bytes combined pubkey+signature at Level I (isogeny-based, extremely compact).

---

## Composite & Hybrid — Deep Dive

This section provides focused analysis of the composite and hybrid approaches, which are the most actively debated design decisions in the PQC migration.

### Terminology

| Term | Definition |
|------|-----------|
| **Composite** | Single certificate/object containing both PQ and traditional algorithms, bound together cryptographically. One OID, one cert, atomic operations. |
| **Non-composite hybrid** | Separate certificates for PQ and traditional, bound at the protocol level (e.g., RFC 9763 "Related Certificates"). |
| **Hybrid KEM** | Combining key shares from PQ and traditional KEMs in a single key exchange message (concatenation approach). |
| **PQ/T** | Post-Quantum/Traditional — the general concept of combining quantum-resistant and classical algorithms. RFC 9794 formalizes terminology. |

### Composite Standards Pipeline — June 2026

| Draft | Area | Status | Track | Next Step |
|-------|------|--------|-------|-----------|
| **Composite ML-DSA Sigs** (lamps-pq-composite-sigs) | X.509/PKI | **RFC Ed Queue** (v-19) | Standards | RFC publication |
| **Composite ML-KEM** (lamps-pq-composite-kem) | X.509/PKI | Publication Requested (v-14) | Standards | **Stalled 71+ days awaiting AD** — key bottleneck |
| **CMS Composite Sigs** (lamps-cms-composite-sigs) | CMS | **RFC Ed Queue** (v-05) | Standards | Unblock normative refs → publish |
| **CMS Composite KEM** (lamps-cms-composite-kem) | CMS | **Publication Requested** (v-01) | Standards | Awaiting AD action |
| **JOSE Composite Sigs** (jose-pq-composite-sigs) | JOSE | WG Document (v-01) | Standards | WG development |
| **OpenPGP PQC** (openpgp-pqc) | OpenPGP | **AUTH48 → RFC 9980** | Standards | Author approvals + AD editorial response |
| **TLS Composite ML-DSA** (reddy-tls-composite-mldsa) | TLS | Expired | -- | Dead. TLS WG rejected. |

### Non-Composite / Related Certificates Pipeline

| Draft/RFC | Area | Status | Notes |
|-----------|------|--------|-------|
| **RFC 9763** | X.509/PKI | **Published RFC** | Non-composite hybrid cert binding. NSA-authored. CNSA 2.0 aligned. |
| **Chameleon Certs** (bonnell-lamps-chameleon-certs) | X.509/PKI | Individual (v-07) | Efficient paired cert encoding. Competing with composites. Stalled. |

### Hybrid KEM Combiner Pipeline

| Draft | Area | Status | Notes |
|-------|------|--------|-------|
| **CFRG Hybrid KEMs** (irtf-cfrg-hybrid-kems) | CFRG | Active RG Doc (v-11) | Generic combiner framework. Blocking ref for OpenPGP. |
| **X-Wing** (connolly-cfrg-xwing-kem) | CFRG | Individual (v-10) | NOT adopted. Concrete ML-KEM-768+X25519 construction. |

### Hybrid Key Exchange — Settled (Production)

| Protocol | Draft | Status | Deployment |
|----------|-------|--------|------------|
| **TLS** | ECDHE-MLKEM | **IESG Evaluation** (v-05; telechat Jun 18) | >65% of web traffic |
| **TLS** | Hybrid KEX Framework | **AUTH48 → RFC 9954** (v-16) | All authors approved; blocked on normative ref |
| **SSH** | ML-KEM Hybrid KEX | RFC Ed Queue (v-10) | Publication imminent |
| **IPsec** | ML-KEM in IKEv2 | **IETF Last Call** (v-05; ends Jun 15) | 4 implementations |
| **IPsec** | FrodoKEM in IKEv2 | WG Adopted (v-00) | BSI/ANSSI conservative alternative |

### Geopolitical Split on Composite vs. Non-Composite

| Camp | Position | Mechanism |
|------|----------|-----------|
| **ANSSI/BSI (EU)** | Composite required during transition | Composite certs (single OID, atomic) |
| **NSA/CNSA 2.0 (US)** | Non-composite preferred; pure PQ goal | RFC 9763 Related Certs; skip hybrid entirely |
| **IETF LAMPS WG** | Both, but composite is primary track | Composite drafts furthest ahead |
| **OpenPGP** | Composite mandatory | ML-DSA-65+Ed25519 is MUST implement |

### Composite ML-DSA Signatures — Pros/Cons (Updated)

| Pros | Cons |
|------|------|
| **Cleared IESG** — on track for RFC publication | Significantly larger certificates |
| Defense-in-depth (secure if either algorithm holds) | Combinatorial explosion of OIDs |
| Protocol backwards-compatible (looks like single algorithm) | Added implementation complexity |
| Aligned with ANSSI/BSI hybrid recommendations | NSA/CNSA 2.0 does NOT recommend hybrid |
| OIDs early-allocated; multiple implementations | |

### Composite ML-KEM — Pros/Cons

| Pros | Cons |
|------|------|
| Same defense-in-depth as composite signatures | Same size/complexity concerns |
| Composite remains strong if one component holds | KEM composition raises KDF binding questions |
| Less contentious than signature composites | NSA opposition to hybrid approaches |
| | **Stalled 71+ days awaiting AD processing — key bottleneck** |

---

## TLS

### draft-ietf-tls-ecdhe-mlkem (Hybrid ECDHE+ML-KEM)
**Status:** **IESG Evaluation**, v-05 (May 2026), **Standards Track**. Telechat **June 18, 2026**.

Defines X25519MLKEM768, SecP256r1MLKEM768, SecP384r1MLKEM1024. v-05 changes: Standards Track, normative hybrid design framework ref, added NIST SP 800-227 context, obsoletes pre-standard Kyber768 code points.

| Pros | Cons |
|------|------|
| Already massively deployed (>65% of Cloudflare traffic) | Larger ClientHello can exceed MTU |
| Belt-and-suspenders security | Combined key shares ~1,600 bytes |
| No adoption controversy; uncontested consensus | May trigger HelloRetryRequest round-trips |
| Standards Track; **IESG telechat scheduled Jun 18** | |

---

### draft-ietf-tls-mlkem (Pure ML-KEM for TLS 1.3)
**Status:** Stalled — Revised I-D Needed (v-07, Feb 2026). **HIGHLY CONTROVERSIAL.**

Defines pure (non-hybrid) ML-KEM-512/768/1024 as NamedGroups. No progress since February.

| Pros | Cons |
|------|------|
| Required for CNSA 2.0/FIPS compliance by 2033 | DJB filed formal IETF appeals alleging false consensus |
| Necessary migration path beyond hybrids | DJB moderated from TLS list during Last Call period |
| Clean specification | SIKE precedent argues hybrid is essential safety net |
| | Ongoing "NSA and IETF" blog series (parts 1-5) |
| | **Stalled since Feb 2026 — WGLC issues unresolved** |

---

### draft-ietf-tls-mldsa (ML-DSA in TLS 1.3)
**Status:** **Waiting for AD Go-Ahead** (v-03, May 2026). **Passed IETF Last Call (Jun 1).**

| Pros | Cons |
|------|------|
| NIST's primary PQ signature standard | ML-DSA-65 signatures: 3,309 bytes |
| **Passed IETF Last Call** — GENART: "Ready", SECDIR: "Has nits" | Certificate chains add ~17 KB to handshakes |
| Awaiting AD (Deb Cooley) go-ahead for IESG | Exceeds TCP initcwnd (~14 KB), adds round-trips |
| Fast sign/verify vs SLH-DSA | No hybrid/composite mechanism defined here |

---

### draft-ietf-tls-cert-abridge (Certificate Compression / Abridged Certs)
**Status:** Expired (v-02, Mar 2025)

| Pros | Cons |
|------|------|
| Addresses critical bandwidth challenge of PQC certificate chains | Expired — no recent activity |
| Dictionary-based approach provides better compression | Benefits diminish if leaf certificates grow significantly with PQC |
| Applicable beyond TLS to CT logs and other PKI storage systems | Complementary but not sufficient alone for PQ sig size problem |

---

### draft-sheffer-tls-pqc-continuity (PQC Downgrade Protection)
**Status:** Individual, v-01 (Mar 2026)

| Pros | Cons |
|------|------|
| Addresses real downgrade vulnerability during PQC transition | Individual draft, not WG-adopted |
| HSTS-inspired design leverages proven pattern | Requires clients to maintain state/cache for visited servers |
| Works at TLS layer regardless of application protocol | May create issues if cached commitments become stale |

---

### draft-reddy-tls-slhdsa (SLH-DSA in TLS 1.3)
**Status:** In ISE Review (v-02, Nov 2025)

| Pros | Cons |
|------|------|
| Hash-based only -- no lattice assumptions | Signatures 7,856 to **49,856 bytes** |
| If ML-DSA broken, SLH-DSA survives | Much slower than ML-DSA |
| NIST standardized (FIPS 205) | Impractical for most TLS use cases |

---

### draft-reddy-tls-composite-mldsa (Composite ML-DSA in TLS 1.3)
**Status:** Expired. NOT adopted by WG. Effectively dead.

| Pros | Cons |
|------|------|
| Belt-and-suspenders for authentication | Doubles already-excessive signature overhead |
| Protection against ML-DSA bugs/breaks | Not adopted by WG |
| | TLS WG reluctant given signature size crisis |

---

### draft-ietf-tls-hybrid-design (Hybrid Key Exchange Framework)
**Status:** **AUTH48 → RFC 9954** (v-16, Sep 2025), Informational. All authors approved. Blocked on normative ref dependency (RFC-to-be 9846, Cluster 553).

| Pros | Cons |
|------|------|
| **Assigned RFC 9954** — publication imminent | Still Informational, not Standards Track |
| Clean concatenation-based design | Blocked on normative reference dependency |
| 16 revisions, well-established | |

---

### draft-ietf-plants-merkle-tree-certs (Merkle Tree Certificates)
**Status:** PLANTS WG Adopted, v-04 (May 2026), **Standards Track**. Fresh revision May 24.

**The most promising solution to the PQ signature size crisis. Let's Encrypt committed June 3, 2026.**

| Pros | Cons |
|------|------|
| Eliminates signatures from certs for modern clients | Only works for "up-to-date" relying parties |
| <800 byte Merkle proofs instead of ~17 KB signatures | Requires new infrastructure (tree publication) |
| Chrome designated MTCs as "preferred option for PQ certs" | Full ecosystem rollout could take 10-15 years |
| **Let's Encrypt committed** (staging late 2026, production 2027) | Active development (v-04 May 24) |
| Google+Cloudflare+Let's Encrypt backing — critical mass reached | |

---

### draft-ietf-tls-key-share-prediction (Key Share Prediction)
**Status:** WG Document, v-04 (Mar 2026), Standards Track

| Pros | Cons |
|------|------|
| Avoids HelloRetryRequest round-trips | Depends on DNS infrastructure (SVCB/HTTPS) |
| DNS-based signaling mechanism | Stale DNS can cause mispredictions |
| Already adopted by WG | |

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
| **RFC 9935** | ML-KEM for X.509 | ML-KEM algorithm IDs for X.509 (Mar 2026) |
| **RFC 9936** | ML-KEM in CMS | ML-KEM via KEMRecipientInfo (Mar 2026) |
| **RFC 9964** | ML-DSA in COSE/JOSE | ML-DSA serialization; defines AKP key type (May 2026) |

### Near-Complete (RFC Ed Queue)

| Draft | Status | Notes |
|-------|--------|-------|
| **draft-ietf-lamps-pq-composite-sigs** v-19 | **RFC Ed Queue** | Composite ML-DSA sigs. Cleared IESG May 2026. |
| **draft-ietf-lamps-cms-composite-sigs** v-05 | **RFC Ed Queue (blocked)** | Composite ML-DSA in CMS. Blocked on normative refs. |

### In IESG / Advanced WG Stage

| Draft | Status | Notes |
|-------|--------|-------|
| **draft-ietf-lamps-pq-composite-kem** v-14 | Publication Requested | Composite ML-KEM. **Stalled 71+ days awaiting AD. Key bottleneck.** |
| **draft-ietf-lamps-cms-composite-kem** v-01 | **Publication Requested** | Composite ML-KEM in CMS. Advanced from WG LC. Shepherd write-up Jun 3. |

### FN-DSA (FALCON) Drafts

| Draft | Status | Notes |
|-------|--------|-------|
| **draft-turner-lamps-fn-dsa-certificates** v-00 | Individual (Nov 2025) | FN-DSA algorithm IDs for X.509 |
| **draft-turner-lamps-cms-fn-dsa** v-00 | Individual (Nov 2025) | FN-DSA for CMS signatures |

### FN-DSA -- Pros/Cons

| Pros | Cons |
|------|------|
| FN-DSA offers smallest PQ signature sizes (~690/1,233 bytes) | FIPS 206 stuck in DoC clearance (since Aug 2025) |
| Enables early adoption path for upcoming NIST standard | **NSA: FN-DSA permanently excluded from CNSA 2.0** (implementation error susceptibility) |
| Alternative lattice-based sig diversifying PQC portfolio | Adding another sig algorithm increases ecosystem fragmentation |

### Early / Stalled

| Draft | Status | Notes |
|-------|--------|-------|
| **draft-bonnell-lamps-chameleon-certs** v-07 | Individual | Encodes cert differences efficiently. Competing with composites. |
| **draft-ounsworth-lamps-pq-external-pubkeys** v-05 | Individual, expired | External key references by hash+URL. Limited interest. |
| **draft-reddy-lamps-x509-pq-commit** v-01 | Individual | PQC hosting continuity / downgrade protection via X.509 extension. |

---

## IPsec / IKEv2

### draft-ietf-ipsecme-ikev2-mlkem (ML-KEM for IKEv2)
**Status:** **IETF Last Call** (v-05, Mar 2026; LC ends **Jun 15**), Standards Track. Advanced from AD Evaluation.

| Pros | Cons |
|------|------|
| Built on RFC 9370 (Multiple Key Exchanges) | Increased packet sizes, potential IP fragmentation |
| NIST-standardized (FIPS 203) | Downgrade attack prevention requires careful deployment |
| 4 known implementations (Cisco, Palo Alto, strongSwan, Apple) | |
| **In IETF Last Call** — TSVART: "Ready w/nits" | |

### draft-ietf-ipsecme-ikev2-pqc-auth (PQC Auth in IKEv2)
**Status:** Awaiting Shepherd Write-Up (v-06, Oct 2025), Standards Track

| Pros | Cons |
|------|------|
| Clean integration via RFC 8420 framework | Large signatures (ML-DSA: 2420-4627 bytes) |
| Passed WG Last Call | SLH-DSA signatures even larger |
| Supports both ML-DSA and SLH-DSA | Does not address hybrid authentication |

### draft-hu-ipsecme-pqt-hybrid-auth (Hybrid Auth in IKEv2)
**Status:** Individual, v-04 (Feb 2026), NOT adopted

| Pros | Cons |
|------|------|
| Safety net if PQC algorithms prove vulnerable | No WG adoption after 4 revisions |
| Aligns with LAMPS composite work | Lacks algorithm combination selection guidance |

### draft-ietf-ipsecme-hybrid-kem-ikev2-frodo (FrodoKEM for IKEv2)
**Status:** **WG Adopted** (v-00, Mar 2026)

| Pros | Cons |
|------|------|
| Plain LWE -- more conservative than structured lattices | Much larger key/ciphertext than ML-KEM |
| Recommended by EU (BSI, ANSSI) | Not NIST-selected (ISO only) |
| 20+ supportive comments in adoption call | Adds complexity: ML-KEM vs FrodoKEM choice |

### draft-reddy-ipsecme-ikev2-hybrid-reliable (Composite KE + TCP for IKEv2)
**Status:** Individual, v-00 (Jan 2026)

| Pros | Cons |
|------|------|
| Combined KE payload reduces message count | TCP fallback adds implementation complexity |
| TCP transport solves IP fragmentation | Individual draft, not WG-adopted |
| Vetted algorithm pairings reduce config errors | Composite approach locks in specific pairings |

---

## SSH

### draft-ietf-sshm-mlkem-hybrid-kex (ML-KEM Hybrid KEX for SSH)
**Status:** **RFC Ed Queue** (v-10, Feb 2026), Standards Track. Publication imminent.

Defines: `mlkem768nistp256-sha256`, `mlkem1024nistp384-sha384`, `mlkem768x25519-sha256`

| Pros | Cons |
|------|------|
| In RFC Editor queue — publication imminent | ML-KEM adds ~1.5 KB to key exchange |
| CNSA 2.0 compliant (mlkem1024nistp384) | Ephemeral keypair per connection required |
| Clear specification, positive reviews | |
| Multiple implementations expected | |

### RFC 9941 (NTRU Prime + X25519 in SSH)
**Status:** **Published RFC**, Informational

| Pros | Cons |
|------|------|
| **Published as RFC 9941** | NTRU Prime NOT a NIST winner |
| Default in OpenSSH for ~5 years | Informational only, not Standards Track |
| Documents existing massive deployment | Long-term future unclear as ML-KEM gains traction |

### draft-harrison-sshm-mlkem (Standalone ML-KEM for SSH)
**Status:** Individual, v-01 (Dec 2025)

| Pros | Cons |
|------|------|
| Pure PQ simplifies implementation vs hybrid | Individual draft, not WG-adopted |
| Required for eventual CNSA 2.0 pure-PQ compliance | No hybrid fallback if ML-KEM vulnerability discovered |
| Cisco/AWS authorship signals industry deployment interest | |

---

## HPKE

### draft-ietf-hpke-pq (PQ/Hybrid KEMs for HPKE)
**Status:** WG Document, v-04 (Mar 2026), Standards Track. No change since March.

| Pros | Cons |
|------|------|
| Both pure PQ and hybrid options | Hybrid combinations increase implementation complexity |
| Leverages widely-implemented HPKE framework | 9 algorithm combinations may fragment deployment |
| SHA-3 KDF options for quantum-resistant key derivation | Foundation draft -- many protocols depend on this completing |

**Significance:** Foundational. HPKE is used across JOSE, COSE, MLS, and other protocols. PQ HPKE unblocks PQ support in all downstream protocols.

---

## COSE / JOSE

### RFC 9964 (ML-DSA in COSE/JOSE) — PUBLISHED
**Status:** **Published RFC** (May 2026), Proposed Standard

Defines AKP key type and ML-DSA-44/65/87 algorithm identifiers for COSE and JOSE.

| Pros | Cons |
|------|------|
| **Published RFC** — foundational for all PQ COSE/JOSE | Large pub keys (1312-2592 bytes) and sigs (2420-4627 bytes) |
| Defines AKP key type used across all PQ COSE drafts | Seed-only private key format may conflict with expanded format impls |
| NIST-standardized (FIPS 204) | |

### draft-ietf-cose-sphincs-plus v-07 (SLH-DSA in COSE/JOSE)
**Status:** WG Document, Standards Track

| Pros | Cons |
|------|------|
| Hash-based -- most conservative security | Very large signatures |
| Good companion to ML-DSA for diversity | Slower than ML-DSA |
| NIST standardized (FIPS 205) | |

### draft-ietf-jose-pqc-kem v-05 (PQ KEMs for JOSE/COSE)
**Status:** WG Document, Standards Track

| Pros | Cons |
|------|------|
| Fills KEM gap for JOSE/COSE | Does not address hybrid PQ/traditional |
| Steady progression through 5 revisions | |

### draft-reddy-cose-jose-pqc-hybrid-hpke v-11 (Hybrid HPKE)
**Status:** Individual, NOT adopted

| Pros | Cons |
|------|------|
| Addresses critical hybrid use case | Still individual after 11 revisions |
| 11 revisions show sustained effort | Complex dependency chain |
| | First adoption call deferred |

### draft-ietf-jose-pq-composite-sigs v-01 (Composite ML-DSA in JOSE)
**Status:** WG Document (v-01, Feb 2026). No change since March.

| Pros | Cons |
|------|------|
| JOSE bindings for composite ML-DSA signatures | Early stage, no progress since February |
| WG-adopted, unlike the hybrid HPKE draft | Depends on composite sigs (lamps) draft finalizing |

### draft-skokan-jose-hpke-pq-pqt v-03 (PQ/T Algorithm Registrations for JOSE)
**Status:** Individual, v-03 (Feb 2026)

| Pros | Cons |
|------|------|
| Comprehensive coverage: both hybrid and pure PQ options | Individual draft, may face adoption challenges |
| Leverages proven HPKE framework | 12+ algorithm registrations could create selection confusion |
| JSON Web Key representation using AKP key type | |

---

## OpenPGP

### draft-ietf-openpgp-pqc → RFC 9980 (PQC in OpenPGP)
**Status:** **AUTH48 → RFC 9980**, Proposed Standard, v-17 (Jan 2026). **All authors approved. Blocked on AD.**

Assigned RFC 9980. All four authors approved (May 28-30, 2026). **Blocked on AD (Paul Wouters)** responding to editorial questions #8 and #12. No normative reference blockers — all refs are published documents (FIPS 203/204/205, RFC 9580, etc.).

Defines:
- Composite signatures: ML-DSA-65+Ed25519 (MUST), ML-DSA-87+Ed448 (SHOULD)
- Composite encryption: ML-KEM + ECDH (X25519/X448)
- SLH-DSA standalone: MAY implement

| Pros | Cons |
|------|------|
| **Assigned RFC 9980 — publication imminent** | Composite keys/sigs significantly larger than classical |
| Multiple interoperable implementations | SLH-DSA support only MAY |
| Strong European institutional backing (BSI) | |
| Clear algorithm hierarchy (MUST/SHOULD/MAY) | |
| Sets template for other protocol areas | |
| **First composite PQ signature RFC for a major protocol** | |

---

## MLS (Messaging Layer Security)

### draft-ietf-mls-combiner v-02 (Flexible Hybrid PQ MLS Combiner)
**Status:** **Expired** (Apr 25, 2026). Needs new revision. WG milestone: Dec 2026.

| Pros | Cons |
|------|------|
| Elegant amortized approach -- PQ operations less frequent | **Expired** — needs new revision to continue |
| Anti-downgrade protections built in | Complex two-session architecture |
| Companion draft defines ML-KEM cipher suites | |

### draft-ietf-mls-pq-ciphersuites v-04 (PQ Ciphersuites for MLS)
**Status:** WG Document (v-04, Mar 2026)

| Pros | Cons |
|------|------|
| Defines ML-KEM + ML-DSA cipher suites for MLS | Depends on combiner draft (currently expired) |
| Active development | |

---

## EAP (Extensible Authentication Protocol)

### draft-reddy-emu-pqc-eap-tls (PQC in EAP-TLS)
**Status:** Individual, v-02 (Jan 2026)

| Pros | Cons |
|------|------|
| Protects widely-deployed enterprise authentication | Individual draft, limited deployment momentum |
| Addresses specific PQ cert size challenges | Large PQ cert chains may exceed MTU in wireless environments |
| Covers both EAP-TLS and EAP-TTLS | Requires coordinated upgrades across RADIUS/authenticators/supplicants |

### draft-ietf-emu-pqc-eapaka (PQ KEMs for EAP-AKA')
**Status:** WG Document, v-02 (Mar 2026)

| Pros | Cons |
|------|------|
| WG document indicates EMU consensus on need | Fragmentation mechanism adds protocol complexity |
| Backward compatible for gradual deployment | Cellular infrastructure upgrade cycles are slow |
| Attribute-level fragmentation addresses cellular MTU | |

**Significance:** Critical for 3GPP/5G PQC migration.

---

## EDHOC / LAKE

### draft-spm-lake-pqsuites (PQ Cipher Suites for EDHOC)
**Status:** Individual, v-01 (Oct 2025)

| Pros | Cons |
|------|------|
| Brings PQC to EDHOC for IoT/CoAP ecosystems | ML-KEM/ML-DSA introduce significant overhead for constrained devices |
| Ericsson authorship | Individual draft |

### draft-lake-pocero-authkem-edhoc (KEM Auth for EDHOC)
**Status:** Individual, v-00 (Oct 2025)

| Pros | Cons |
|------|------|
| KEM-based auth avoids large PQ signature overhead | Five-message handshake increases round-trips |
| Signature-free reduces computational burden on IoT | Very early stage (v-00) |

---

## ACME

### draft-ietf-acme-profiles (ACME Profiles)
**Status:** WG Document, v-01 (Mar 2026). **Already in production.**

| Pros | Cons |
|------|------|
| **In production** (Let's Encrypt, Boulder, Certbot, Caddy) | Profile proliferation may create confusion |
| Key enabler for automated PQC certificate issuance | Requires server and client upgrades |
| Strong standardization momentum | |

### draft-davidben-acme-profile-sets (ACME Profile Sets)
**Status:** Individual, v-00 (Oct 2025). Google authorship.

| Pros | Cons |
|------|------|
| Addresses PQC transition (serve legacy + PQ clients) | Individual draft, not yet WG-adopted |
| Automatic multi-certificate issuance | Managing multiple certificates per domain |

---

## DNSSEC

### draft-sheth-pqc-dnssec-strategy (PQC DNSSEC Strategy)
**Status:** Individual, v-00 (Oct 2025). Verisign Labs authorship.

| Pros | Cons |
|------|------|
| Dual-algorithm strategy balances security with practicality | Individual draft, no DNSOP WG consensus |
| Merkle Tree Ladder mode addresses sig size challenge | PQC sig sizes fundamentally challenge UDP transport |

### draft-fregly-research-agenda-for-pqc-dnssec (PQC DNSSEC Research Agenda)
**Status:** Individual, expired (Dec 2024)

| Pros | Cons |
|------|------|
| Multi-institution collaboration | Expired, no continued development |
| Identified critical UDP viability question | Research agenda rather than solution |

---

## UTA (Using TLS in Applications)

### draft-ietf-uta-pqc-app (PQC Recommendations for TLS Applications)
**Status:** WG Document, v-01 (Feb 2026)

| Pros | Cons |
|------|------|
| Comprehensive guidance covering entire TLS ecosystem | Best practices may become outdated quickly |
| Addresses practical deployment challenges | Balancing hybrid vs pure PQ recs may create confusion |

---

## Kerberos / PKINIT / GSS-API

### PKINIT (RFC 4556) -- Kerberos Public Key Pre-Authentication
**Status:** **PQC GAP — No dedicated work exists.** Critical for IdM/FreeIPA.

PKINIT uses DH key exchange and X.509 certificates for Kerberos pre-authentication. Both components are quantum-vulnerable. The Kitten WG has no PQC work items. RFC 4557 (OCSP for PKINIT) explicitly lists RSA, DSA, SHA-1 and needs updating.

| Pros | Cons |
|------|------|
| X.509 certificate auth will inherit PQC from LAMPS WG | **DH key exchange has NO PQ KEM replacement proposed** |
| Well-defined protocol with clear upgrade points | Kitten WG has no active PQC effort |
| FreeIPA/Active Directory both use PKINIT | RFC 4557 (OCSP for PKINIT) lists only legacy algorithms |
| | No IETF draft, no research paper, no proposal |

**Assessment:** This is a genuine standardization gap. The certificate authentication side will eventually inherit PQC, but the DH key exchange requires protocol-level changes that nobody has started. High priority for organizations using PKINIT (IdM, AD).

### Kerberos SPAKE Pre-Auth (RFC 9588)
**Status:** **PQC EXPOSED.** EC groups are quantum-vulnerable. No PQ PAKE draft for Kerberos.

| Pros | Cons |
|------|------|
| Kerberos core (symmetric) is quantum-safe | SPAKE2 EC groups (edwards25519, P-256) are quantum-vulnerable |
| Password-derived keys don't use public-key crypto | No PQ PAKE draft exists for Kerberos specifically |
| draft-vos-cfrg-pqpake (CPaceOQUAKE+, Apr 2026) could be future path | PQ PAKE is early-stage CFRG research |

### draft-kario-gss-keyex-pqc (GSS-API PQC Key Exchange for SSH)
**Status:** Individual (v-00, Apr 2026). **Red Hat authored** (Alicja Kario).

Defines GSS-API key exchange with hybrid ML-KEM for SSH: `gss-mlkem768nistp256-sha256-*` (RECOMMENDED), `gss-mlkem1024nistp384-sha384-*`, `gss-mlkem768x25519-sha256-*`.

| Pros | Cons |
|------|------|
| **Red Hat authored** — direct alignment with IdM/FreeIPA | Individual draft, not WG-adopted |
| Builds on SSH ML-KEM draft (RFC Ed Queue) | GSS-API/Kerberos for non-SSH protocols still exposed |
| Three hybrid ML-KEM combinations defined | Very early stage (v-00) |
| CNSA 2.0 compliant (mlkem1024nistp384) | |

---

## DTLS / WireGuard / Signal Protocol

### DTLS 1.3 (RFC 9147)
**Status:** Inherits from TLS 1.3. All PQC key exchange drafts marked DTLS-OK.

| Pros | Cons |
|------|------|
| All TLS PQC key exchange drafts explicitly support DTLS | Hybrid key shares (~2.3 KB) exceed typical 1,500-byte MTU |
| DTLS handles fragmentation natively | Fragmentation adds round-trips for constrained devices |
| QUIC uses TLS 1.3 directly and inherits PQC | QUIC uses own fragmentation, not DTLS fragmentation |

### WireGuard
**Status:** No crypto agility by design. PSK workaround deployed commercially.

| Pros | Cons |
|------|------|
| PSK feature provides PQ protection when distributed via PQ-safe channel | No crypto agility — cannot add ML-KEM directly |
| ExpressVPN deployed PSK+ML-KEM at scale (15-20ms overhead) | True PQ-WireGuard would be a different protocol |
| PQ-WireGuard research exists (ML-KEM based) | Not an IETF protocol — adoption is implementation-driven |
| Tailscale documenting PQC considerations | |

### Signal Protocol (PQXDH + SPQR)
**Status:** **PRODUCTION DEPLOYED.** Most advanced consumer PQC deployment.

| Pros | Cons |
|------|------|
| **Phase 1 PQXDH** (Sep 2023): ML-KEM hybrid initial key agreement | PQ deniable authentication remains unsolved research problem |
| **Phase 2 SPQR** (Oct 2025): PQ forward secrecy + post-compromise security | Not an IETF/standards-track protocol |
| Formally verified (ProVerif) at Eurocrypt 2025, USENIX Security 2025 | Authentication still classical |
| Sets benchmark for all consumer E2E encryption | |

---

## CFRG (Crypto Forum Research Group)

### draft-irtf-cfrg-hybrid-kems v-11 (CFRG Hybrid KEMs Combiner)
**Status:** Active RG Document (v-11, May 2026).

| Pros | Cons |
|------|------|
| CFRG-adopted generic hybrid KEM combiner framework | IRTF track — Informational only |
| Preferred over X-Wing for generic use | No shepherd or AD assigned yet |
| One new revision since March (v-10→v-11) | |

### draft-connolly-cfrg-xwing-kem v-10 (X-Wing Hybrid KEM)
**Status:** Individual, Informational. NOT adopted by CFRG.

| Pros | Cons |
|------|------|
| Simple, opinionated "just use X-Wing" | Not CFRG-adopted |
| Strong security proof | Only one security level |
| Backed by prominent cryptographers | |

### RFC 9858 (Additional LMS Parameter Sets)
**Status:** Published RFC (Oct 2025)

35-40% signature size reduction with 192-bit params. NIST co-author.

---

## PQUIP Working Group

| Draft | Status | Notes |
|-------|--------|-------|
| **PQC for Engineers** | RFC Ed Queue (v-14) | IESG approved; in RFC Editor queue |
| **PQC for HSMs/Constrained** | Waiting for Write-Up (v-04, Mar 2026) | HSM, smart card, IoT guidance |
| **Hybrid Sig Spectrums** | Active | Design goals/security analysis for hybrid sigs |
| **PQC Use Cases** | Active | Migration strategies |
| **PQC Sig Migration** | Individual (v-01) | Evaluates composite vs dual vs PQC-only certs |
| **PQC Deployment Guidance** | Individual (v-01) | Comprehensive algorithm catalog |
| **PQC Overview** | Individual (v-00, Mar 2026) | PQC landscape overview |
| **RFC 9794** | Published RFC | PQ/T hybrid terminology standard |

---

## Non-IETF PQC Standards

### NIST Special Publications

| Document | Status | Significance |
|----------|--------|-------------|
| **SP 800-227** | Final (Nov 2025) | Recommendations for KEM usage including hybrid/composite. Key for FIPS certification. |
| **SP 800-131A Rev.3** | Draft | Transitioning use of crypto algorithms. Updated with PQC timelines. |
| **IR 8547** | Final (Feb 2025) | 15-year classical crypto phase-out roadmap. |

### ETSI

| Document | Status | Significance |
|----------|--------|-------------|
| **TS 103 744** | Published | Quantum-Safe Hybrid Key Exchanges framework. |
| **TS 104 015** | In development | PQC Migration Strategies for operators. |
| **TR 103 619** | Published | Quantum-Safe VPN guidance. |
| **GR QSC 001-007** | Various | QSC group reports on threats, implementation, migration. |

### ISO/IEC

| Document | Status | Significance |
|----------|--------|-------------|
| **ISO/IEC 18033-2 Amd** | In development | Adding ML-KEM to encryption standard. |
| **ISO/IEC 14888** | In development | Adding ML-DSA/SLH-DSA to digital signature standard. |

### 3GPP

| Document | Status | Significance |
|----------|--------|-------------|
| **TR 33.703** | Study phase | PQC for 5G/6G auth and key agreement. Connects to IETF EAP-AKA' work. |

### Industry Standards

| Organization | Document | Status | Significance |
|-------------|----------|--------|-------------|
| **W3C** | Web Crypto API Modern Algorithms | Proposal stage | ML-KEM, ML-DSA for browser PQC. |
| **OASIS** | KMIP 3.0 | Published | Key management with PQC support. |
| **OASIS** | PKCS#11 v3.2 | In development | HSM interface adding ML-KEM, ML-DSA, SLH-DSA. |
| **TCG** | TPM 2.0 Library Spec V185 | Published | Hardware root-of-trust PQC readiness. |
| **IEEE** | P802.11 PQC Study Group | Active (2025) | PQC impact on Wi-Fi auth. |

### National Agency Guidance

| Agency | Document | Status | Significance |
|--------|----------|--------|-------------|
| **BSI** (Germany) | TR-02102-1 | Updated 2026-01 | PQC for critical infrastructure by **June 2026**. **CRQC horizon shortened to 10-15 years.** <5% of German orgs have migration plans. |
| **ANSSI** (France) | PQC Position Papers | Updated 2024 | Recommends hybrid/composite. Three-phase timeline through 2030. |
| **EU** | NIS2 PQC Amendment (COM(2026) 13) | Proposed Jan 2026 | Mandatory PQC transition policies for Member States. |
| **Canada** | Federal PQC Migration | Mandated Apr 2026 | Migration plans due; critical systems by 2031; full by 2035. |

---

## Major Cross-WG Debates

### 1. Hybrid vs. Pure PQC

| For Hybrid | Against Hybrid |
|------------|----------------|
| Defense-in-depth during transition | Implementation/operational complexity |
| SIKE precedent (broken after deployment) | Double migration cost (ECC → hybrid → PQC) |
| EUCC/ANSSI/BSI recommend hybrid | Some governments ban hybrid |
| Protects against undiscovered PQC weaknesses | Once one component broken, security properties degrade |

**Current consensus (June 2026):** Hybrid key exchange is settled (X25519+ML-KEM-768 deployed on >65% of traffic). Hybrid *signatures* via composite are advancing through RFC Editor — composite ML-DSA sigs cleared IESG. The debate is shifting from "should we do composite?" to "how fast can we publish?" Composite KEM remains the key bottleneck (71+ days stalled at AD). Meanwhile, Let's Encrypt's commitment to MTCs signals the ecosystem is hedging with multiple approaches simultaneously.

### 2. Composite vs. Non-Composite Certificates

| Composite (Single cert, combined keys/sigs) | Non-Composite (Separate certs, protocol-level binding) |
|--------------------------------------------|-------------------------------------------------------|
| Protocol backwards-compatible | Keeps cert tooling simple |
| Atomic revocation of all components | More flexible -- independent algorithm lifecycle |
| Prevents cross-protocol key reuse | NSA/CNSA 2.0 preferred approach (RFC 9763) |
| ANSSI/BSI aligned | Moves complexity to management layer |
| **Composite sigs cleared IESG (May 2026)** | Risk: attackers may separate/combine certs unexpectedly |
| LAMPS WG primary focus | |

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
- Merkle Tree Certificates (most promising -- <800 byte proofs; v-04 May 2026; **Let's Encrypt committed Jun 3**)
- KEMTLS (replace handshake signatures with KEM-based auth -- research stage)
- FN-DSA / FALCON (smaller signatures but FIPS 206 delayed)
- Certificate compression (draft-ietf-tls-cert-abridge -- expired)
- Merkle Tree Ladder mode for DNSSEC
- KEM-based auth for EDHOC (draft-lake-pocero-authkem-edhoc)

### 4. Lattice Security Concerns

No practical break of ML-KEM or ML-DSA announced, but active hedging:
- X25519+ML-KEM-768 (not ML-KEM-512) chosen as hedge against lattice cryptanalysis improvements
- HQC selected as backup KEM (code-based, not lattice-based)
- NIST additional signatures competition provides non-lattice diversity
- Community maintains caution due to relative youth of lattice cryptanalysis

### 5. Downgrade Protection During PQC Transition

| Approach | Draft | Mechanism |
|----------|-------|-----------|
| TLS-layer caching | draft-sheffer-tls-pqc-continuity | HSTS-inspired TLS extension caching PQC commitments |
| Certificate-embedded commitment | draft-reddy-lamps-x509-pq-commit | X.509 extension signaling PQC intent |

---

## Real-World Deployment Status

### Key Exchange (Largely Deployed)

| Vendor | Status |
|--------|--------|
| **Cloudflare** | >65% of human TLS traffic uses hybrid ML-KEM. **Full PQ SASE** (TLS + MASQUE + IPsec) as of Feb 2026. ML-DSA support for origin connections **mid-2026**. MTC support **mid-2027**. Cloudflare One PQ auth **early 2028**. **PQC roadmap accelerated to 2029 hard deadline.** |
| **Chrome** | X25519+ML-KEM default. ML-KEM disable option removed in Chrome 138. Chrome 154 (Oct 2026): "Always Use Secure Connections" default. |
| **Firefox** | X25519MLKEM768 hybrid since Firefox 135. |
| **Edge** | ML-KEM hybrid PQ TLS since Edge 131. |
| **Safari/Apple** | PQ TLS 1.3 key exchange in macOS Tahoe 26 / iOS 26 / visionOS 26. **Corecrypto open-sourced** (May 22) with ML-KEM + ML-DSA. CryptoKit exposes ML-KEM-768/1024, ML-DSA-65/87. PQ in VPN (IKEv2), SSH, Apple Watch. |
| **Google** | **2029 hard deadline** for full PQC migration. Android 17 integrates ML-DSA via KeyMint/KeyPairGenerator API. |
| **Akamai** | PQ mid-tier connections completed across all networks Q1 2026. |
| **AWS** | ML-KEM PQ TLS in KMS, ACM, Secrets Manager. ML-DSA keys via KMS APIs (GA). **IAM Roles Anywhere** supports ML-DSA-signed CA certs (Mar 2026). Secrets Manager hybrid PQ TLS auto-enabled in Agent 2.0.0+. AWS-LC first FIPS-validated open-source crypto with ML-KEM. Pre-standard Kyber being removed 2026. |
| **Microsoft** | PQC APIs GA in Windows Server 2025, Windows 11, .NET 10 (ML-KEM/ML-DSA/SLH-DSA). **AD CS ML-DSA support GA** (May 2026 KB5087539): ML-DSA-44/65/87 for CA cert signing, code signing, OCSP. Composite ML-DSA+ECDSA in Windows Insider. |
| **OpenSSH** | sntrup761x25519 default ~5 years. ML-KEM hybrid in RFC Ed Queue. |

### Signatures / Certificates (NOT Yet Deployed)

- No public PQ certificates in production as of mid-2026.
- First PQ certificates expected late 2026; broad browser trust unlikely before 2027.
- **Merkle Tree Certificates (MTC):** Chrome will NOT use ML-DSA in traditional X.509. MTCs are Chrome's "preferred (or only) option." **Let's Encrypt committed Jun 3** — staging late 2026, production 2027. Phase 1 experiment with Cloudflare underway. Phase 2 (CT Log operators) Q1 2027. Phase 3 (Chrome CQRS) Q3 2027.
- **Microsoft AD CS** shipped ML-DSA support (May 2026) — first major CA platform with PQ signing GA.
- **Google** set 2029 hard deadline; Android 17 ML-DSA via KeyMint API.
- Signature migration is less urgent: active quantum adversary needed (vs. passive harvest-now for KEMs).
- **CA/Browser Forum SC-081v3:** Certificate validity at 200 days (**active** Mar 2026), shrinking to 100 days (Mar 2027), 47 days (Mar 2029).
- **CA/B Forum SMC013 (S/MIME):** Enables single-key/non-hybrid PQC certificates for S/MIME experimentation (adopted Aug 2025).

### Certificate Automation (ACME)

- ACME Profiles mechanism in production at Let's Encrypt.
- Profile Sets draft provides multi-certificate issuance for PQC transition.

---

## Regulatory Drivers

| Regulation | Requirement |
|-----------|-------------|
| **US EO 14144** (Jan 2025) | Agencies list PQC-ready products, mandate support within 90 days |
| **NSA CNSA 2.0** | VPNs/routers: support and prefer CNSA 2.0 **now (2026)**. New acquisitions compliant by **Jan 2027**. ML-DSA exclusive by 2030; ML-KEM-1024 exclusive by 2033. **FN-DSA permanently excluded.** FIPS 140-2 certs Historical **Sep 2026**. |
| **NIST IR 8547** | 15-year classical crypto phase-out roadmap |
| **NIST SP 800-227** | KEM recommendations including hybrid/composite approaches |
| **NIST IR 8610** (May 2026) | Round 3 additional signature selection — 9 of 14 candidates advanced |
| **EU NIS2 PQC Amendment** (COM(2026) 13, Jan 2026) | Mandatory PQC transition policies for all EU Member States |
| **EU Coordinated Roadmap** | National strategies by end 2026; high-risk by 2030; full by 2035 |
| **Germany BSI** | PQC for critical infrastructure by **June 2026**. **CRQC horizon shortened to 10-15 years** (was ~20). <5% of German orgs have migration plans. |
| **France ANSSI** | Hybrid approaches during transition; three-phase timeline through 2030 |
| **Canada** | Federal PQC migration plans due April 2026; critical systems by 2031; full by 2035 |
| **UAE** | National Cryptography Discovery Platform launched Jun 5, 2026 for PQC asset mapping and migration planning |
| **CA/B Forum SC-081v3** | Cert validity: 200 days (**active** Mar 2026) → 100 days (Mar 2027) → 47 days (Mar 2029) |

---

## Master Status Table

| # | Draft/RFC | WG/Area | Status | Track | Maturity |
|---|-----------|---------|--------|-------|----------|
| 1 | draft-ietf-tls-ecdhe-mlkem | TLS | **IESG Evaluation**, v-05 (telechat Jun 18) | Standards | **Production** |
| 2 | draft-ietf-tls-mlkem | TLS | Stalled (WGLC issues), v-07 | Informational | Stalled |
| 3 | draft-ietf-tls-mldsa | TLS | **Waiting for AD Go-Ahead**, v-03 | Informational | High |
| 4 | draft-ietf-tls-hybrid-design | TLS | **AUTH48 → RFC 9954**, v-16 | Informational | **Near-done** |
| 5 | draft-ietf-tls-cert-abridge | TLS | Expired, v-02 | Standards | Stalled |
| 6 | draft-sheffer-tls-pqc-continuity | TLS | Individual, v-01 | -- | Early |
| 7 | draft-ietf-plants-merkle-tree-certs | PLANTS | WG, v-04 | Standards | Medium |
| 8 | draft-ietf-tls-key-share-prediction | TLS | WG, v-04 | Standards | Medium-High |
| 9 | draft-reddy-tls-slhdsa | TLS | ISE Review, v-02 | Standards | Low |
| 10 | draft-reddy-tls-composite-mldsa | TLS | Expired | Standards | Dead |
| 11 | draft-farrell-tls-pqg | TLS | Individual, v-04 | BCP | Medium |
| 12 | RFC 9881 | LAMPS | **Published RFC** | Standards | **DONE** |
| 13 | RFC 9882 | LAMPS | **Published RFC** | Standards | **DONE** |
| 14 | RFC 9909 | LAMPS | **Published RFC** | Standards | **DONE** |
| 15 | RFC 9802 | LAMPS | **Published RFC** | Standards | **DONE** |
| 16 | RFC 9935 | LAMPS | **Published RFC** | Standards | **DONE** |
| 17 | RFC 9936 | LAMPS | **Published RFC** | Standards | **DONE** |
| 18 | draft-ietf-lamps-pq-composite-sigs | LAMPS | **RFC Ed Queue**, v-19 | Standards | **Near-done** |
| 19 | draft-ietf-lamps-pq-composite-kem | LAMPS | Publication Requested, v-14 | Standards | Stalled (AD) |
| 20 | draft-ietf-lamps-cms-composite-sigs | LAMPS | **RFC Ed Queue (blocked)**, v-05 | Standards | **Near-done** |
| 21 | draft-ietf-lamps-cms-composite-kem | LAMPS | **Publication Requested**, v-01 | Standards | Mid-High |
| 22 | draft-turner-lamps-fn-dsa-certificates | LAMPS | Individual, v-00 | -- | Early |
| 23 | draft-turner-lamps-cms-fn-dsa | LAMPS | Individual, v-00 | -- | Early |
| 24 | draft-reddy-lamps-x509-pq-commit | LAMPS | Individual, v-01 | -- | Early |
| 25 | RFC 9814 | LAMPS | **Published RFC** | Standards | **DONE** |
| 26 | RFC 9629 | LAMPS | **Published RFC** | Standards | **DONE** |
| 27 | RFC 9763 | LAMPS | **Published RFC** | Standards | **DONE** |
| 28 | RFC 9810 | LAMPS | **Published RFC** | Standards | **DONE** |
| 29 | draft-bonnell-lamps-chameleon-certs | LAMPS | Individual, v-07 | -- | Stalled |
| 30 | draft-ietf-ipsecme-ikev2-mlkem | IPSECME | **IETF Last Call** (ends Jun 15), v-05 | Standards | High |
| 31 | draft-ietf-ipsecme-ikev2-pqc-auth | IPSECME | Awaiting Write-Up, v-06 | Standards | High |
| 32 | draft-hu-ipsecme-pqt-hybrid-auth | IPSECME | Individual, v-04 | Standards | Medium |
| 33 | draft-ietf-ipsecme-hybrid-kem-ikev2-frodo | IPSECME | **WG Adopted**, v-00 | -- | Medium |
| 34 | draft-reddy-ipsecme-ikev2-hybrid-reliable | IPSECME | Individual, v-00 | -- | Early |
| 35 | draft-ietf-sshm-mlkem-hybrid-kex | SSHM | **RFC Ed Queue**, v-10 | Standards | **Near-done** |
| 36 | RFC 9941 | SSHM | **Published RFC** | Informational | **DONE** |
| 37 | draft-harrison-sshm-mlkem | SSHM | Individual, v-01 | -- | Early |
| 38 | draft-ietf-hpke-pq | HPKE | WG, v-04 | Standards | Medium-High |
| 39 | RFC 9964 | COSE | **Published RFC** | Standards | **DONE** |
| 40 | draft-ietf-cose-sphincs-plus | COSE | WG, v-07 | Standards | Medium-High |
| 41 | draft-ietf-jose-pqc-kem | JOSE | WG, v-05 | Standards | Medium-High |
| 42 | draft-reddy-cose-jose-pqc-hybrid-hpke | COSE | Individual, v-11 | -- | Medium |
| 43 | draft-skokan-jose-hpke-pq-pqt | JOSE | Individual, v-03 | -- | Early |
| 44 | draft-ietf-jose-pq-composite-sigs | JOSE | WG, v-01 | Standards | Early |
| 45 | draft-ietf-openpgp-pqc → RFC 9980 | OpenPGP | **AUTH48**, v-17 | Standards | **Near-done** |
| 46 | draft-ietf-mls-combiner | MLS | **Expired**, v-02 | Standards | Expired |
| 47 | draft-ietf-mls-pq-ciphersuites | MLS | WG, v-04 | Standards | Low-Medium |
| 48 | draft-reddy-emu-pqc-eap-tls | EMU | Individual, v-02 | -- | Early |
| 49 | draft-ietf-emu-pqc-eapaka | EMU | WG, v-02 | Standards | Early |
| 50 | draft-spm-lake-pqsuites | LAKE | Individual, v-01 | -- | Early |
| 51 | draft-lake-pocero-authkem-edhoc | LAKE | Individual, v-00 | -- | Very Early |
| 52 | draft-ietf-acme-profiles | ACME | WG, v-01 | Standards | **Production** |
| 53 | draft-davidben-acme-profile-sets | ACME | Individual, v-00 | -- | Early |
| 54 | draft-sheth-pqc-dnssec-strategy | DNSOP | Individual, v-00 | -- | Early |
| 55 | draft-fregly-research-agenda-for-pqc-dnssec | DNSOP | Individual, expired | -- | Stalled |
| 56 | draft-ietf-uta-pqc-app | UTA | WG, v-01 | -- | Early |
| 57 | draft-irtf-cfrg-hybrid-kems | CFRG | Active RG Doc, v-11 | Informational | Medium-High |
| 58 | draft-connolly-cfrg-xwing-kem | CFRG | Individual, v-10 | Informational | Medium |
| 59 | RFC 9858 | CFRG | **Published RFC** | Informational | **DONE** |
| 60 | RFC 9794 | PQUIP | **Published RFC** | Informational | **DONE** |
| 61 | draft-ietf-pquip-pqc-engineers | PQUIP | RFC Ed Queue, v-14 | Informational | High |
| 62 | draft-ietf-pquip-pqc-hsm-constrained | PQUIP | Waiting Write-Up, v-04 | Informational | Medium-High |
| 63 | draft-reddy-pquip-pqc-signature-migration | PQUIP | Individual, v-01 | -- | Early |
| 64 | draft-prabel-pquip-pqc-guidance | PQUIP | Individual, v-01 | -- | Early |
| 65 | PKINIT (RFC 4556) | Kitten | **PQC GAP — no draft** | -- | **Exposed** |
| 66 | Kerberos SPAKE (RFC 9588) | Kitten | **PQC EXPOSED** (EC groups) | -- | **Exposed** |
| 67 | draft-kario-gss-keyex-pqc | Kitten/SSHM | Individual, v-00 (Red Hat) | -- | Early |
| 68 | draft-vos-cfrg-pqpake | CFRG | Individual, v-01 | -- | Very Early |
| 69 | Signal PQXDH + SPQR | -- | **Production Deployed** | -- | **Production** |
| 70 | WireGuard PSK+PQC | -- | Deployed (PSK workaround) | -- | Workaround |
| 71 | DTLS 1.3 (RFC 9147) | TLS | Inherits from TLS 1.3 PQC | -- | High |

---

*Generated 2026-06-10. Sources: IETF Datatracker, Cloudflare blog, Google Security blog, Let's Encrypt blog, Apple corecrypto, Microsoft Learn, AWS Security blog, DJB blog series, NIST CSRC (IR 8610), NSA CNSA 2.0, BSI TR-02102-1, ETSI, ISO, 3GPP, various WG mailing lists.*
