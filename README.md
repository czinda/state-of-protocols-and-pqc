# State of Protocols and Post-Quantum Cryptography

> **Personal tracking fork** of [ietf-wg-pquip/state-of-protocols-and-pqc](https://github.com/ietf-wg-pquip/state-of-protocols-and-pqc)
>
> **Last updated:** 2026-10-05

This repository tracks the status of IETF drafts and RFCs related to post-quantum cryptography (PQC) migration across Internet protocols. It is a fork of the IETF PQUIP working group's official tracker, augmented with:

- Current draft statuses verified against the [IETF Datatracker](https://datatracker.ietf.org/)
- Working group criticism and controversy notes
- Real-world deployment status
- NIST standardization updates (FIPS 203/204/205/206, HQC, additional signature candidates)
- Regulatory driver context (CNSA 2.0, EO 14144, OMB M-26-15, BSI, ANSSI)
- Non-IETF standards tracking (ETSI, ISO, 3GPP, NIST SP, BSI, ANSSI, OASIS, TCG, W3C, IEEE)

For a detailed analysis with pros/cons for each draft and cross-WG debate summaries, see [PQC-Status-Report-2026-08-21.md](PQC-Status-Report-2026-08-21.md).

---

## At a Glance (October 2026)

| Area | Readiness | Key Signal |
|------|-----------|------------|
| KEM Key Exchange | **Production** | **RFC 10024** published (ECDHE-MLKEM). >65% of web traffic. Standalone ML-KEM now in **RFC Editor Queue** (Informational). |
| PQ Signatures in TLS | **Advancing** | ML-DSA TLS now in **RFC Editor Queue** (Informational). Sig size crisis (~17 KB) unsolved. |
| PKI / Certificates | **~95% ready** | Composite sigs in RFC Ed Queue; composite KEM at **IESG Evaluation (Revised I-D Needed; DISCUSS)**; **FN-DSA WG-adopted**. MTC v-05. **Let's Encrypt committed to MTCs**. |
| IPsec / IKEv2 | **RFC Ed Queue** | ML-KEM in RFC Ed Queue; PQC auth now also in **RFC Editor Queue**. |
| SSH | **Published** | ML-KEM hybrid key exchange published as **RFC 10042** (Aug 2026). NTRU Prime published (RFC 9941). |
| OpenPGP | **RFC 9980** | **Published Jun 30.** ML-DSA+Ed25519, ML-KEM+ECDH, SLH-DSA. BSI-backed. Multiple interop implementations. |
| COSE / JOSE | **Publication Requested** | RFC 9964 (ML-DSA). **SLH-DSA submitted to IESG** (v-10). KEM and composite sigs progressing. |
| MLS | **Mixed** | Combiner still expired; PQ ciphersuites v-06 under revision ("Revised I-D Needed"). |
| HPKE | Active | PQ/hybrid KEMs v-05 for HPKE in progress. |
| EAP | Progressing | EAP-AKA' PQ KEM awaiting WG chair go-ahead. |
| EDHOC (LAKE) | Early | PQ cipher suites and KEM auth drafts. |
| DNSSEC | Research | Sig sizes far exceed UDP limits; MTL mode and strategy drafts. |
| Kerberos/PKINIT | **Draft Active** | **PKINIT PQC draft v-01** published (Jun 26). Gap now has an active proposal. GSS-API PQC SSH draft (Red Hat). |
| Signal | **Production** | PQXDH (2023) + Triple Ratchet/SPQR (2025). Most advanced consumer PQC. |
| WireGuard | Workaround | PSK+PQC delivery deployed (ExpressVPN). No crypto agility by design. |

---

## NIST PQC Standards

| Standard | Algorithm | Basis | Status |
|----------|-----------|-------|--------|
| FIPS 203 | ML-KEM | CRYSTALS-Kyber | **Final** (Aug 2024) |
| FIPS 204 | ML-DSA | CRYSTALS-Dilithium | **Final** (Aug 2024) |
| FIPS 205 | SLH-DSA | SPHINCS+ | **Final** (Aug 2024) |
| FIPS 206 | FN-DSA | FALCON | IPD stuck in DoC clearance (submitted Aug 2025). **NSA: FN-DSA excluded from CNSA 2.0 permanently** (implementation error susceptibility). Final expected late 2026-early 2027 at earliest. |
| -- | HQC | Error-correcting codes | Selected Mar 2025, backup KEM (~2027) |

**Additional Signature Schemes (Round 3, May 2026):** NIST advanced **9 candidates** from Round 2 (NIST IR 8610, May 14, 2026). **HAWK withdrawn Jul 29, 2026** after a lattice automorphism attack was discovered that halved effective key strength. **8 candidates remaining:** FAEST, MAYO, MQOM, QR-UOV, SDitH, SNOVA, SQIsign, UOV. *Eliminated from Round 2:* CROSS, LESS, Mirath, PERK, RYDE. Specification tweaks due **Aug 14, 2026**. 7th PQC Conference planned spring/summer 2027.

### IETF Algorithm Naming Convention

| Name | Meaning |
|------|---------|
| _Dilithium_ (or _Dilithium_round3_) | As submitted to NIST Round 3 |
| _ML-DSA-ipd_ | FIPS 204 Initial Public Draft |
| _ML-DSA_ | Final FIPS 204 (now published) |

Same convention applies to Kyber/ML-KEM, SPHINCS+/SLH-DSA, and FALCON/FN-DSA.

---

## Protocol-Independent Algorithm Specifications

| Draft | Status | Link | WG | Topic | Notes |
|-------|--------|------|----|-------|-------|
| Additional LMS Parameter Sets | **RFC 9858** (Oct 2025) | [RFC](https://www.rfc-editor.org/rfc/rfc9858) | CFRG | LMS parameter sets | **Published RFC.** NIST co-author. 35-40% sig size reduction with 192-bit params. |
| Hybrid KEM Combiners | Expired (superseded) | [Datatracker](https://datatracker.ietf.org/doc/draft-ounsworth-cfrg-kem-combiners/) | CFRG | Generic KEM combiners | Led to CFRG adopting draft-irtf-cfrg-hybrid-kems |
| CFRG Hybrid KEMs | **Waiting for Document Shepherd** (v-12, Jul 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-irtf-cfrg-hybrid-kems/) | CFRG | Generic hybrid KEM combiner framework | CFRG-adopted. Preferred over X-Wing for generic use. One IPR disclosure. |
| X-Wing Hybrid KEM | Individual (v-10, Mar 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-connolly-cfrg-xwing-kem/) | CFRG | ML-KEM-768 + X25519 | NOT CFRG-adopted. Generic combiner framework preferred. SP 800-227 published (Nov 2025). |
| Chempat-X (NTRU Prime + X25519) | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-josefsson-ntruprime-hybrid/) | Independent | NTRU Prime hybrid | |
| Kyber KEM | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-cfrg-schwabe-kyber/) | CFRG | Kyber algorithm | Superseded by FIPS 203 |
| LMS (Leighton-Micali) | **RFC 8554** | [RFC](https://www.rfc-editor.org/rfc/rfc8554) | CFRG | Hash-based sigs | |
| NTRU Prime sntrup761 | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-josefsson-ntruprime-streamlined/) | Independent | NTRU Prime KEM | |
| NTRU Key Encapsulation | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-fluhrer-cfrg-ntru/) | CFRG | NTRU algorithm | |
| QSC Keys: Kyber | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-uni-qsckeys-kyber/) | Individual | Kyber encodings | Superseded by FIPS 203 |
| QSC Keys: Dilithium | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-uni-qsckeys-dilithium/) | Individual | Dilithium encodings | Superseded by FIPS 204 |
| QSC Keys: FALCON | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-uni-qsckeys-falcon/) | Individual | FALCON encodings | Superseded by FIPS 206 |
| QSC Keys: SPHINCS+ | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-uni-qsckeys-sphincsplus/) | Individual | SPHINCS+ encodings | Superseded by FIPS 205 |
| X25519Kyber768 for HPKE | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-westerbaan-cfrg-hpke-xyber768d00/) | CFRG | Kyber hybrid for HPKE | Pre-standard Kyber; superseded |
| XMSS | **RFC 8391** | [RFC](https://www.rfc-editor.org/rfc/rfc8391) | CFRG | Extended Merkle sigs | |
| MTL Mode Signatures | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-harvey-cfrg-mtl-mode/) | CFRG/DNS | Amortized PQ sigs | Being explored for DNSSEC (draft-fregly-dnsop-slh-dsa-mtl-dnssec) |

---

## PQC in Protocols

### TLS

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| ECDHE-MLKEM Hybrid KEX | **RFC 10024** (Aug 2026), Proposed Standard | [RFC](https://www.rfc-editor.org/rfc/rfc10024) | X25519MLKEM768, SecP256r1MLKEM768, SecP384r1MLKEM1024 | **PUBLISHED RFC.** >65% of Cloudflare traffic. Default in Chrome/Firefox/Edge. IANA code points: X25519MLKEM768 (4588, **Recommended=Y**), SecP256r1MLKEM768 (4587), SecP384r1MLKEM1024 (4589). Obsoletes pre-standard Kyber768 code points. |
| Pure ML-KEM KEX | **RFC Ed Queue** (v-11, Sep 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-tls-mlkem/) | Non-hybrid ML-KEM for TLS | **DJB appeals fully exhausted** (IAB denied Jun 30). Three WGLCs (Dec 2025, Feb 2026, Jun-Jul 2026). Chairs declared rough consensus Jul 19. **Informational** (not Standards Track). All NamedGroups registered **Recommended=N**. Required for CNSA 2.0 by 2033. Key share reuse: MUST NOT. Liaison statements from O-RAN, IEEE 802.11, 3GPP. |
| ML-DSA Authentication | **RFC Ed Queue** (v-06, Sep 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-tls-mldsa/) | ML-DSA for TLS 1.3 auth | Informational. Signature size crisis (~17 KB) still unsolved. |
| Certificate Compression (Abridged Certs) | Expired (v-02, Mar 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-tls-cert-abridge/) | Cert chain compression for TLS | Expired. Reduces PQ cert overhead via ICA suppression and compression. Complementary to Merkle Tree Certs. |
| SLH-DSA Authentication | **Expired** (v-02, Nov 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-reddy-tls-slhdsa/) | SLH-DSA for TLS 1.3 auth | Dropped from ISE review; now expired/archived. Sigs 7,856-49,856 bytes. |
| Composite ML-DSA | Individual (v-09), likely expired | [Datatracker](https://datatracker.ietf.org/doc/draft-reddy-tls-composite-mldsa/) | Composite hybrid sigs in TLS | NOT adopted by WG. Doubles signature overhead. |
| PQC Continuity / Downgrade Protection | Individual (v-02, Jun 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-sheffer-tls-pqc-continuity/) | Anti-downgrade for PQC transition | Updated v-02. Cache indexing per RFC 9525, withdrawal semantics, CDN/intermediary sections. Presented at IETF 126. |
| PQ/T Dual Certificates | Individual (v-00, Jul 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-yusef-tls-pqt-dual-certs/) | PQ/T hybrid auth with dual certs | **NEW.** Presented at IETF 126 TLS WG (Jul 24). Tschofenig/Shekh-Yusef. |
| Unheaded PQC Authentication | Individual (v-00, Mar 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-bellis-unheaded-pqc-authentication/) | PQC auth without header overhead | |
| Hybrid KEX Framework | **RFC 9954** (Jul 2026), Informational | [RFC](https://www.rfc-editor.org/rfc/rfc9954) | Hybrid key exchange design | **PUBLISHED RFC.** Was blocked on normative ref dependency (now RFC 9846). |
| PQ Guidance | Individual (v-04, Dec 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-farrell-tls-pqg/) | Deployment recommendations | Hybrid KEMs now; no action on sigs yet. Not a WG document. |
| KEMTLS (KEM-based Auth) | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-celi-wiggers-tls-authkem/) | KEM-based auth | Research-stage. Would eliminate PQ sig size problem. |
| X25519Kyber768Draft00 | Expired (superseded) | [Datatracker](https://datatracker.ietf.org/doc/draft-tls-westerbaan-xyber768d00) | Pre-standard Kyber hybrid | Superseded by RFC 10024 |
| SecP256r1Kyber768Draft00 | Expired (superseded) | [Datatracker](https://datatracker.ietf.org/doc/draft-kwiatkowski-tls-ecdhe-kyber/) | ECDHE-Kyber for TLS | Superseded by RFC 10024 |

### LAMPS / X.509 / CMS (PKI)

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| ML-DSA for X.509 | **RFC 9881** | [RFC](https://www.rfc-editor.org/rfc/rfc9881) | ML-DSA algorithm IDs for X.509 | **Published RFC.** Primary NIST PQC sig for PKI. |
| SLH-DSA for X.509 | **RFC 9909** | [RFC](https://www.rfc-editor.org/rfc/rfc9909) | SLH-DSA algorithm IDs for X.509 | **Published RFC.** Conservative hash-based sig for X.509. |
| ML-DSA in CMS | **RFC 9882** | [RFC](https://www.rfc-editor.org/rfc/rfc9882) | ML-DSA for CMS signatures | **Published RFC.** CMS counterpart to RFC 9881. |
| ML-KEM for X.509 | **RFC 9935** | [RFC](https://www.rfc-editor.org/rfc/rfc9935) | ML-KEM algorithm IDs for X.509 | **Published RFC** (Mar 2026). Proposed Standard. |
| Composite ML-DSA Sigs | **RFC Ed Queue** (v-19, May 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-lamps-pq-composite-sigs/) | ML-DSA + traditional composite | **Cleared IESG.** Awaiting first RFC Editor assignment. OIDs early-allocated. ANSSI/BSI favor; NSA opposes. |
| Composite ML-KEM | **IESG Evaluation — Revised I-D Needed** (v-21, Sep 2026; has DISCUSSes) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-lamps-pq-composite-kem/) | ML-KEM + traditional composite | Advanced through IETF LC to IESG Evaluation; now iterating to resolve DISCUSS positions. |
| Composite ML-DSA in CMS | **RFC Ed Queue** (v-05, May 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-lamps-cms-composite-sigs/) | Composite ML-DSA signatures in CMS | **Cleared IETF LC + IESG.** Blocked in RFC Ed on normative refs (composite sigs draft). All directorate reviews positive. |
| Composite ML-KEM in CMS | **Waiting for AD Go-Ahead** (v-01, May 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-lamps-cms-composite-kem/) | Composite ML-KEM for CMS | Advanced from Publication Requested. SECDIR: "Ready." GENART: "Incomplete." Awaiting AD (Deb Cooley) action. Depends on composite KEM draft. |
| ML-KEM in CMS | **RFC 9936** | [RFC](https://www.rfc-editor.org/rfc/rfc9936) | ML-KEM via KEMRecipientInfo | **Published RFC** (Mar 2026). Standards Track. |
| SLH-DSA in CMS | **RFC 9814** | [Datatracker](https://datatracker.ietf.org/doc/rfc9814/) | SLH-DSA in CMS | First PQC CMS signature RFC. |
| FN-DSA for X.509 | **WG Adopted** (v-00, May 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-lamps-fn-dsa-certificates/) | FN-DSA algorithm IDs for X.509 | **Newly WG-adopted** (was Individual). Awaiting FIPS 206 finalization for OID assignments. Smallest PQ sig sizes. |
| FN-DSA in CMS | **WG Adopted** (v-00, May 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-lamps-cms-fn-dsa/) | FN-DSA for CMS signatures | **Newly WG-adopted** (was Individual). Awaiting FIPS 206 finalization. |
| Hash-Based Sigs for X.509 | **RFC 9802** | [RFC](https://www.rfc-editor.org/rfc/rfc9802) | HSS/LMS, XMSS OIDs for X.509 | **Published RFC.** Stateful -- firmware/code signing only. |
| PQC Hosting Continuity | Individual (v-01) | [Datatracker](https://datatracker.ietf.org/doc/draft-reddy-lamps-x509-pq-commit/) | PQC transition for hosted services | Addresses continuity during multi-tenant PQC migration. |
| HSS/LMS in CMS | **RFC 8708** | [RFC](https://www.rfc-editor.org/rfc/rfc8708.html) | HSS/LMS in CMS | |
| NIST PQC KEM Certs | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-turner-lamps-nist-pqc-kem-certificates/) | | Superseded by ML-KEM certs draft |

### IPsec / IKEv2

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| ML-KEM in IKEv2 | **RFC Ed Queue** (v-09, Jul 2026), Proposed Standard | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-ipsecme-ikev2-mlkem/) | ML-KEM key exchange | **Cleared IESG.** Awaiting first RFC Editor assignment. GENART: "Ready." TSVART: "Ready w/nits." 4 implementations (Cisco, Palo Alto, strongSwan, Apple). Shepherd: Scott Fluhrer. AD: Deb Cooley. |
| PQC Auth in IKEv2 | **RFC Ed Queue** (v-12, Aug 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-ipsecme-ikev2-pqc-auth/) | ML-DSA + SLH-DSA auth | Defines generic PQC signature authentication integration + ML-DSA/SLH-DSA specifics. |
| PQ/T Hybrid Auth | Individual (v-04, Feb 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-hu-ipsecme-pqt-hybrid-auth/) | Hybrid auth via composite/multi-cert | Not WG-adopted. |
| FrodoKEM in IKEv2 | **WG Document** (v-03, Aug 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-ipsecme-hybrid-kem-ikev2-frodo/) | FrodoKEM alternative KEM | WG-adopted Mar 11. Active development (v-00→v-03). BSI/ANSSI recommended conservative KEM. |
| Hybrid KE + TCP Reliability | Individual (v-00) | [Datatracker](https://datatracker.ietf.org/doc/draft-reddy-ipsecme-ikev2-hybrid-reliable/) | Composite KE with TCP fallback | Addresses fragmentation issues with large PQC payloads in IKEv2. |

### SSH

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| ML-KEM Hybrid KEX | **RFC 10042** (Aug 2026), Informational | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-sshm-mlkem-hybrid-kex/) | mlkem768nistp256, mlkem1024nistp384, mlkem768x25519 | **Published RFC.** CNSA 2.0 compliant hybrid KEX options for SSH. |
| NTRU Prime + X25519 | **RFC 9941**, Informational | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-sshm-ntruprime-ssh/) | sntrup761x25519-sha512 | **Published RFC.** Default in OpenSSH ~5 years. Documents existing massive deployment. |
| Standalone ML-KEM KEX | Individual (v-02, May 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-harrison-sshm-mlkem/) | Non-hybrid ML-KEM for SSH | Pure ML-KEM without traditional component. For CNSA 2.0 eventual compliance. No WG adoption. |

### HPKE

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| PQ/Hybrid KEMs for HPKE | WG Document (v-05, Jun 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-hpke-pq/) | ML-KEM and hybrid KEMs for HPKE | Extends RFC 9180 with PQ KEM support. Foundation for PQ HPKE across protocols. |

### COSE / JOSE

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| ML-DSA in COSE/JOSE | **RFC 9964** (May 2026), Standards Track | [Datatracker](https://datatracker.ietf.org/doc/rfc9964/) | ML-DSA serialization | **Published RFC.** Defines AKP key type and ML-DSA-44/65/87 algorithm identifiers used across PQ COSE drafts. |
| SLH-DSA in COSE/JOSE | **AD Evaluation** (v-10, Jul 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-cose-sphincs-plus) | SLH-DSA serialization | Draft defines JOSE/COSE encodings for SLH-DSA (FIPS 205). |
| PQ KEMs for JOSE/COSE | WG Document (v-06, Jun 2026), Standards Track | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-jose-pqc-kem/) | ML-KEM in JOSE/COSE | Open issue on AKP key type alignment. |
| Hybrid HPKE in JOSE/COSE | Individual (v-11, Feb 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-reddy-cose-jose-pqc-hybrid-hpke/) | PQ/T hybrid HPKE | Still not WG-adopted after 11 revisions. |
| PQ/T Algorithm Registrations | Individual (v-02) | [Datatracker](https://datatracker.ietf.org/doc/draft-skokan-jose-hpke-pq-pqt/) | PQ/T hybrid algo registrations for JOSE | JOSE algorithm identifiers for PQ and PQ/T HPKE configurations. |
| Composite ML-DSA in JOSE | WG Document (v-03, Jun 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-jose-pq-composite-sigs/) | Composite ML-DSA sigs for JOSE | Six composite algorithm combos defined. COSE algorithm values requested (-54 through -59). |
| FALCON in COSE/JOSE | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-cose-falcon) | FALCON serialization | Awaiting FIPS 206 finalization. |
| COSE Kyber | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-steele-cose-kyber/) | Kyber encoding | Superseded by draft-ietf-jose-pqc-kem |
| HPKE KEM wrapper | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-ajitomi-cose-cose-key-jwk-hpke-kem/) | KEM wrapper for COSE | |
| PQ Sigs (omnibus) | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-cose-post-quantum-signatures/) | Multi-algo serializations | Split into per-algorithm drafts |
| Hybrid KEX in JOSE/COSE | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-ra-cose-hybrid-encrypt/) | KEM combiner hybrid | |
| HSS/LMS in COSE | **RFC 8778** | [Datatracker](https://datatracker.ietf.org/doc/rfc8778/) | HSS/LMS for COSE | |

### OpenPGP

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| PQC in OpenPGP | **RFC 9980** (Jun 2026), Proposed Standard | [RFC](https://www.rfc-editor.org/rfc/rfc9980) | PQC extension for OpenPGP | **Published Jun 30.** Extends RFC 9580 with ML-KEM, ML-DSA, SLH-DSA for OpenPGP. Composite: ML-DSA-65+Ed25519 (MUST), ML-DSA-87+Ed448 (SHOULD). ML-KEM+ECDH. SLH-DSA standalone (MAY). BSI-backed. Multiple interop implementations. |

### MLS

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| Hybrid PQ MLS Combiner | **Expired** (v-02, Oct 2025; expired Apr 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-mls-combiner/) | Combines traditional + PQ MLS sessions | **Still expired.** Needs new revision. Amortized PQ operations. PARTIAL/FULL modes. Anti-downgrade. WG milestone: Dec 2026. |
| PQ Ciphersuites for MLS | WG Document (v-06, Jun 2026; **Revised I-D Needed**) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-mls-pq-ciphersuites/) | ML-KEM ciphersuites for MLS | Issues raised by WG. Shepherd write-up updated Jul 3. Defines ML-KEM + ML-DSA cipher suites for MLS protocol. |
| X-Wing for MLS | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-mahy-mls-xwing/) | X-Wing KEM ciphersuite | Superseded by combiner approach |

### EAP (Extensible Authentication Protocol)

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| PQC in EAP-TLS | Individual (v-02) | [Datatracker](https://datatracker.ietf.org/doc/draft-reddy-emu-pqc-eap-tls/) | PQC considerations for EAP-TLS | Addresses PQ cert size challenges in EAP-TLS. Builds on RFC 9191 (large certs). |
| PQ KEMs for EAP-AKA' | WG Document (v-02, Mar 2026; **Waiting for WG Chair Go-Ahead**) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-emu-pqc-eapaka/) | PQ KEM integration in EAP-AKA' | 3GPP-relevant. Telecom PQC migration path. |

### EDHOC (LAKE WG)

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| PQ Cipher Suites for EDHOC | Individual (v-01) | [Datatracker](https://datatracker.ietf.org/doc/draft-spm-lake-pqsuites/) | Post-quantum EDHOC cipher suites | Constrained IoT PQC. Defines ML-KEM + ML-DSA suites for EDHOC. |
| KEM Auth for EDHOC | Individual (v-00) | [Datatracker](https://datatracker.ietf.org/doc/draft-lake-pocero-authkem-edhoc/) | KEM-based authentication in EDHOC | Eliminates PQ sig overhead via KEM-auth. IoT-focused. |

### ACME

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| ACME Profiles | WG Document (v-01) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-acme-profiles/) | Profile mechanism for ACME | Enables PQC algorithm negotiation via profiles. Framework for PQC cert issuance. |
| ACME Profile Sets | Individual (v-00) | [Datatracker](https://datatracker.ietf.org/doc/draft-davidben-acme-profile-sets/) | Bundled profile sets for ACME | Groups related profiles (e.g., PQC + traditional). Complementary to ACME Profiles. |
| ACME PQC Negotiation | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-giron-acme-pqcnegotiation/) | Algorithm negotiation | Superseded by profiles approach |

### DNSSEC

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| PQC DNSSEC Strategy | Individual | [Datatracker](https://datatracker.ietf.org/doc/draft-sheth-pqc-dnssec-strategy/) | Strategy for PQC DNSSEC migration | High-level strategy addressing sig size vs UDP limits. |
| Research Agenda for PQC DNSSEC | Individual | [Datatracker](https://datatracker.ietf.org/doc/draft-fregly-research-agenda-for-pqc-dnssec/) | Research directions for PQC in DNSSEC | Identifies open problems: sig size, zone signing, key distribution. Links to MTL mode. |
| Stateful HBS for DNSSEC | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-afrvrd-dnsop-stateful-hbs-for-dnssec/) | Hash-based sigs for DNSSEC | PQ sig sizes (7,856+ bytes) far exceed DNS UDP limits (~1,500 bytes) |

### Routing Security (RPKI / BGPsec)

RPKI is **bulk-validated** by relying parties on a tight cadence; PQ signature sizes and verification costs have outsize operational impact. This tracker includes RPKI-focused PQ work because it is a major Internet security dependency with scaling constraints that differ from typical TLS/PKI.

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| PQ signature experiments + migration considerations for RPKI | Individual (v-02, Aug 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-yoshikawa-sidrops-pqc-rpki/) | PQC migration analysis | Experiments with classical/PQ/composite candidates; compares **Parallel Publication** vs **Mixed Tree** migration; includes RRDP/rsync/Erik transport impact. Informational, test-only. |
| pqRPKI (Merkle Tree Ladder + PQLO) | Individual (v-00, Jul 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-li-sidrops-pqrpki/) | Additive PQ authentication | **Adds one PQ signature per CA** over a Merkle Tree Ladder derived from the Manifest, rather than re-signing every object. Updates RFC 9286 *if approved*. |

**Related/adjacent (not yet fully tracked here):**
- BGPsec UPDATE signature algorithm profile is still classical (see [RFC 8608](https://www.rfc-editor.org/rfc/rfc8608)).

### NTP / Time Synchronization (NTS)

Time security matters for certificate validation (OCSP/CRLs), log timestamping, and general ecosystem correctness. NTS uses **TLS 1.3**, so it inherits PQ KEX improvements automatically, but **deployment and pool scaling** are active work areas.

| Spec | Status | Link | Topic | Notes |
|------|--------|------|-------|-------|
| Network Time Security for NTP | **RFC 8915** | [RFC](https://www.rfc-editor.org/rfc/rfc8915) | NTS key exchange uses TLS 1.3 | PQC impact is primarily inherited via TLS; operationally relevant because many deployments are embedded / constrained. |
| NTS extensions for enabling pools | WG Document (v-01, Jul 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-ntp-nts-keyexchange-pool/) | NTS pool architecture | Defines NTS-KE extensions for pool operation (load distribution). |
| NTS4PTP (NTS for Precision Time Protocol) | WG Document (v-04, Aug 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-ntp-nts-for-ptp/) | Key management for IEEE 1588 | Extends RFC 8915 concepts for PTP. |

### Email Authentication (DKIM / DKIM2)

Email ecosystems are long-lived, high-volume, and strongly impacted by signature size and DNS transport constraints. Even where PQ algorithms are not immediately specified, **protocol evolution + algorithm agility** efforts should be tracked as PQC pressure increases.

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| DKIM2 specification | WG Document (v-06, Aug 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-dkim-dkim2-spec/) | DKIM2 | Defines DKIM2; current algorithms include RSA-SHA256 and Ed25519-SHA256; explicitly anticipates future algorithms (including “post-quantum”). |
| DKIM2 motivation | **Expired** (v-02) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-dkim-dkim2-motivation/) | Rationale | Archived, but still useful background. |

### QUIC / HTTP/3 (PQC constraints)

QUIC inherits TLS 1.3, but PQC affects QUIC more sharply due to handshake packet size constraints (e.g., QUIC Initial). Track as an “inherits TLS + extra constraints” protocol surface.

| Protocol | Status | Link | Notes |
|----------|--------|------|-------|
| QUIC | **RFC 9000** | [RFC](https://www.rfc-editor.org/rfc/rfc9000) | Inherits TLS PQ KEX; PQC key share sizes can stress QUIC handshake size limits and amplification rules. |

### Web Authentication (WebAuthn / FIDO2)

Not an IETF protocol set, but a high-impact ecosystem for PQ signature migration. Track here for situational awareness; see IETF 126 SAAG minutes for PQ web authentication transition discussion.

- SAAG IETF 126 minutes: “transition strategies for post-quantum web authentication” — [minutes-126-saag](https://datatracker.ietf.org/doc/minutes-126-saag/)

### UTA (Using TLS in Applications)

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| PQC Recommendations for TLS Apps | WG Document (v-03, Jul 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-uta-pqc-app/) | PQC deployment guidance for applications | Recommendations for apps using TLS to enable PQC. Practical migration guidance. |

### Kerberos / PKINIT / GSS-API

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| PKINIT PQC (ML-KEM) | **Individual** (v-01, Jun 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-bokovoy-kitten-pkinit-pqc/) | ML-KEM for Kerberos PKINIT | **NEW — Gap now has an active draft.** Proposes ML-KEM replacement for DH key exchange in PKINIT. No WG adoption yet. |
| OCSP for PKINIT (RFC 4557) | **Needs update** | [RFC](https://www.rfc-editor.org/rfc/rfc4557) | OCSP usage in PKINIT | Explicitly lists RSA, DSA, SHA-1. Needs algorithm agility update. |
| GSS-API PQC Key Exchange (SSH) | Individual (v-00, Apr 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-kario-gss-keyex-pqc/) | GSS-API ML-KEM hybrid KEX for SSH | **Red Hat authored (Alicja Kario).** Defines gss-mlkem768nistp256, gss-mlkem1024nistp384, gss-mlkem768x25519. Builds on SSH ML-KEM draft. |
| Kerberos SPAKE Pre-Auth (RFC 9588) | **PQC EXPOSED** | [RFC](https://www.rfc-editor.org/rfc/rfc9588) | SPAKE2 pre-auth for Kerberos | Uses EC groups (edwards25519, P-256) — quantum-vulnerable. No PQ PAKE draft for Kerberos. |
| PQ PAKE (CPaceOQUAKE+) | Individual (v-02, Jul 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-vos-cfrg-pqpake/) | Post-quantum aPAKE | CFRG draft. Incorporated TEMPO timing side-channel fix. Could eventually replace SPAKE for Kerberos. Apple/UC Irvine authored. Early stage. |
| Kerberos-over-TLS | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-vanrein-tls-kdh/) | TLS transport for Kerberos | Would inherit PQC from TLS. Expired 2020, no activity. |

### DTLS / WireGuard / Signal

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| DTLS 1.3 (RFC 9147) | **Inherits from TLS 1.3** | [RFC](https://www.rfc-editor.org/rfc/rfc9147) | Datagram TLS | All TLS PQC key exchange drafts marked DTLS-OK. MTU fragmentation with hybrid key shares is main concern. |
| WireGuard | **PSK workaround deployed** | -- | Modern VPN protocol | No crypto agility by design. PSK feature provides PQ protection when PSKs distributed via PQ-safe channel. ExpressVPN deployed at scale. PQ-WireGuard research exists (ML-KEM). |
| Signal Protocol (PQXDH + SPQR) | **PRODUCTION DEPLOYED** | [Signal](https://signal.org/docs/specifications/pqxdh/) | End-to-end encryption | **Most advanced consumer PQC.** Phase 1: PQXDH (ML-KEM hybrid, Sep 2023). Phase 2: Triple Ratchet/SPQR (PQ forward secrecy, Oct 2025). Formally verified. Only gap: PQ deniable authentication. |

---

## PQC-Adjacent Improvements

Infrastructure work that enables or smooths PQC migration.

### Published RFCs

| Title | RFC | WG | Purpose |
|-------|-----|-----|---------|
| KEMRecipientInfo for CMS | **RFC 9629** (Aug 2024) | LAMPS | Foundation for all CMS KEM usage. Algorithm-agnostic. |
| Related Certificates for Multi-Auth | **RFC 9763** | LAMPS | Non-composite hybrid cert binding. NSA-authored. CNSA 2.0 aligned. |
| ML-DSA in COSE/JOSE | **RFC 9964** (May 2026) | COSE | ML-DSA serialization for COSE/JOSE. Defines AKP key type. |
| PQ/T Hybrid Terminology | **RFC 9794** | PQUIP | Standardized vocabulary for hybrid approaches. Defines key terms. |
| CMP v3 with KEM Support | **RFC 9810** (Jul 2025) | LAMPS | PKI enrollment infrastructure for PQC. Obsoletes RFC 4210/9480. |
| IKEv2 Intermediate Exchange | **RFC 9242** | IPSECME | Handles large PQC data in SA establishment. |
| IKEv2 Message Fragmentation | **RFC 7383** | IPSECME | Packet fragmentation for PQC key sizes. |
| Multiple Key Exchanges in IKEv2 | **RFC 9370** | IPSECME | Hybrid key exchange framework. Foundation for ML-KEM in IKEv2. |
| PSK Mixing in IKEv2 | **RFC 8784** | IPSECME | Post-quantum security via PSK mixing. |
| PSK in CMS | **RFC 8696** | LAMPS | Quantum-safe CMS via PSK. |
| PSK in TLS 1.3 | **RFC 8773** | TLS | Quantum-safe TLS via external PSK. |
| Large Certs in EAP | **RFC 9191** | EMU | Solutions for large PQC certificates in EAP. |
| Hybrid Key Exchange in TLS 1.3 | **RFC 9954** (Jul 2026) | TLS | Hybrid key exchange construction for TLS 1.3. |
| Hybrid Signature Spectrums | **RFC 9955** | PQUIP | Design goals and security analysis for hybrid signature approaches. |
| PQC for Engineers | **RFC 9958** (Jun 2026) | PQUIP | Engineer-focused PQC primer. Informational. |

### Active Drafts

| Title | Status | Link | WG | Notes |
|-------|--------|------|----|-------|
| Merkle Tree Certificates | **PLANTS WG Adopted** (v-05, Jul 2026), Standards Track | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-plants-merkle-tree-certs/) | PLANTS | **Most promising PQ sig size solution.** <800 byte proofs vs ~17 KB sigs. Chrome "preferred option for PQ certs." Google+Cloudflare. **Let's Encrypt committed Jun 3** — staging late 2026, production 2027. |
| TLS Key Share Prediction | WG Document (v-04, Mar 2026), Standards Track | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-tls-key-share-prediction/) | TLS | DNS-based keyshare signaling. Avoids PQ retransmission. Updates RFC 8446. No forward progress since Mar. |
| Chameleon Certificates | **Expired** (v-07, Oct 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-bonnell-lamps-chameleon-certs/) | LAMPS | Expired Apr 2026. Never WG-adopted. Appears dead. |
| External Keys for X.509 | Expired (v-05, Apr 2025) | [Datatracker](https://datatracker.ietf.org/doc/html/draft-ounsworth-lamps-pq-external-pubkeys) | LAMPS | External key references by hash+URL. Limited interest. |
| Suppressing CA Certs in TLS | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-kampanakis-tls-scas-latest/) | TLS | Reduces PQ cert chain overhead. |
| Larger IKEv2 Payload | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-nir-ipsecme-big-payload/) | IPSECME | Increased payload for PQC keys. |
| Alt PSK Mixing in IKEv2 | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-smyslov-ipsecme-ikev2-qr-alt/) | IPSECME | Alternative PSK approach. |
| IKEv2 Auth Method Announce | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-ipsecme-ikev2-auth-announce/) | IPSECME | Advertise supported auth methods. |
| Hybrid Non-Composite Auth (IKEv2) | Expired (Sep 2022) | [Datatracker](https://datatracker.ietf.org/doc/draft-guthrie-ipsecme-ikev2-hybrid-auth/) | IPSECME | |
| Non-Composite Hybrid Auth (PKIX) | Expired (Sep 2022) | [Datatracker](https://datatracker.ietf.org/doc/draft-becker-guthrie-noncomposite-hybrid-auth/) | LAMPS | Superseded by RFC 9763. |
| Alt Composite Sigs | Expired (v-00, Dec 2023) | [Datatracker](https://datatracker.ietf.org/doc/draft-nir-lamps-altcompsigs/) | LAMPS | Alternative composition with strong non-separability. No activity since 2023. |

### PQUIP Working Group Documents

| Title | Status | Link | Notes |
|-------|--------|------|-------|
| PQC for Engineers | **RFC 9958** (Jun 2026), Informational | [RFC](https://www.rfc-editor.org/rfc/rfc9958) | **Published RFC.** Engineer-focused PQC primer. |
| PQC Overview | Individual (v-00, Mar 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-prabel-pquip-pqc-overview/) | PQC landscape overview. |
| Hybrid Signature Spectrums | **RFC 9955** | [RFC](https://www.rfc-editor.org/rfc/rfc9955) | **Published RFC.** Design goals/security analysis for hybrid sigs. |
| PQC Use Cases | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-vaira-pquip-pqc-use-cases/) | Migration strategies. |
| PQC Migration Guidance | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-kwiatkowski-pquip-pqc-migration/) | General PQC migration framework. |
| PQC Signature Migration | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-reddy-pquip-pqc-signature-migration/) | Signature-specific migration strategies. |
| PQC for HSM/Constrained Devices | **IESG Evaluation** (v-06, Jul 2026; "has enough positions to pass") | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-pquip-pqc-hsm-constrained/) | PQC considerations for HSMs, smart cards, constrained environments. SECDIR review flagged issues. AD action holder 31+ days. |
| Telecom PQC Migration | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-li-pquip-teleco-pqc-migration/) | Telecom-specific PQC migration guidance. 3GPP/5G context. |
| PQC Deployment Guidance | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-prabel-pquip-pqc-guidance/) | Practical deployment recommendations. |

---

## Non-IETF PQC Standards

### NIST Special Publications

| Document | Status | Topic |
|----------|--------|-------|
| **SP 800-227** | Final (Nov 2025) | Recommendations for Key-Encapsulation Mechanisms. Guidance on hybrid/composite KEM usage. |
| **SP 800-131A Rev.3** | Draft | Transitioning use of crypto algorithms. Updated with PQC transition timelines. |
| **IR 8547** | Final (Feb 2025) | Transition to Post-Quantum Cryptography Standards. Migration planning guide. |

### ETSI (European Telecommunications Standards Institute)

| Document | Status | Topic |
|----------|--------|-------|
| **TS 103 744** | Published | Quantum-Safe Hybrid Key Exchanges. Framework for hybrid approaches. |
| **TS 104 015** | In development | Migration Strategies for Post-Quantum Cryptography. Practical roadmap for operators. |
| **TR 103 619** | Published | Quantum-Safe VPN. PQC integration in VPN technologies. |
| **GR QSC 001-007** | Various | Quantum-Safe Cryptography (QSC) group reports on threat analysis, implementation, and migration. |

### ISO/IEC

| Document | Status | Topic |
|----------|--------|-------|
| **ISO/IEC 18033-2 Amd** | In development | Amendment adding ML-KEM to encryption algorithms standard. |
| **ISO/IEC 14888** | In development | Amendment adding ML-DSA/SLH-DSA to digital signature standard. |

### 3GPP (Telecom)

| Document | Status | Topic |
|----------|--------|-------|
| **TR 33.703** | Study phase | Study on PQC for 3GPP systems. Evaluates impact on 5G/6G auth, key agreement, and protocol signaling. |

### W3C

| Document | Status | Topic |
|----------|--------|-------|
| **Web Crypto API Modern Algorithms** | Proposal stage | Adding PQC algorithms (ML-KEM, ML-DSA) to Web Cryptography API. |

### OASIS

| Document | Status | Topic |
|----------|--------|-------|
| **KMIP 3.0** | Published | Key Management Interoperability Protocol v3.0 with PQC algorithm support. |
| **PKCS#11 v3.2** | In development | Cryptographic token interface adding ML-KEM, ML-DSA, SLH-DSA mechanism types. |

### TCG (Trusted Computing Group)

| Document | Status | Topic |
|----------|--------|-------|
| **TPM 2.0 Library Spec V185** | Published | TPM 2.0 specification with PQC algorithm support (ML-KEM, ML-DSA). Hardware root-of-trust PQC readiness. |

### BSI (German Federal Office for Information Security)

| Document | Status | Topic |
|----------|--------|-------|
| **TR-02102-1** | Updated 2025 | Crypto recommendations including PQC algorithms. Full PQC migration for critical infrastructure by 2030, all other systems by 2032. |
| **Technical Guidelines** | Ongoing | PQC migration recommendations for German federal IT systems. |

### ANSSI (French National Agency for Security of Information Systems)

| Document | Status | Topic |
|----------|--------|-------|
| **PQC Position Papers** | Published (2022, updated 2024) | Recommends hybrid approaches. Composite sigs preferred. Three-phase migration timeline through 2030. |
| **Security Recommendations** | Ongoing | PQC guidance for French government and critical infrastructure. |

### IEEE

| Document | Status | Topic |
|----------|--------|-------|
| **P802.11 PQC Study Group** | Active (2025) | Studying PQC impact on Wi-Fi (802.11) authentication and key exchange. SAE/OWE implications. |

### South Korea (KCMVP)

| Document | Status | Topic |
|----------|--------|-------|
| **KCMVP PQC Certifications** | First certifications granted (2026) | South Korea's FIPS 140 equivalent. Exgate and ITCEN PNS first PQC-certified products. MSIT expanding PQC pilot to telecom, finance, transportation, defense, space. Training 620 specialists Jul-Nov 2026. |

---

## Protocols Requiring No PQC-Specific Action

These protocols embed IETF cryptographic building blocks and will inherit PQC support automatically.

### Security Area

| Protocol | RFC | WG | Dependencies |
|----------|-----|-----|-------------|
| ACME | [8555](https://datatracker.ietf.org/doc/rfc8555/) | ACME | PKCS#10, JOSE/JWS, TLS |
| CMC | [5272](https://datatracker.ietf.org/doc/rfc5272/) | LAMPS | CMS, PKCS#10 |
| QUIC | [9000](https://datatracker.ietf.org/doc/rfc9000/) | QUIC | TLS 1.3 (may need lockstep update) |
| DoH | [8484](https://datatracker.ietf.org/doc/rfc8484/) | DPRIVE | TLS |
| EST | [7030](https://datatracker.ietf.org/doc/rfc7030/) | LAMPS | CMC, CMS, PKCS#10, TLS |
| HTTPS | [9110](https://datatracker.ietf.org/doc/rfc9110/) | HTTPbis | TLS |
| SCEP | [8894](https://datatracker.ietf.org/doc/rfc8894/) | LAMPS | CMS, PKCS#10 (legacy constraints; prefer EST/CMP for PQC) |
| S/MIME | [5751](https://datatracker.ietf.org/doc/rfc5751/) | LAMPS | CMS (Sec 4.1 lists RSA/DSA/SHA-1 -- may need update) |
| LDAPS/StartTLS | [4511](https://datatracker.ietf.org/doc/rfc4511/) | -- | TLS (inherits PQC from TLS stack) |
| RadSec | [6614](https://datatracker.ietf.org/doc/rfc6614/) | RADEXT | TLS/DTLS (draft-ietf-radext-radiusdtls-bis updates) |

### Other

| Protocol | RFC | WG | Dependencies |
|----------|-----|-----|-------------|
| SRTP | [3711](https://datatracker.ietf.org/doc/rfc3711/) | AVT | DTLS (KEX via DTLS, symmetric AEAD for media) |
| OCSP | [6960](https://datatracker.ietf.org/doc/rfc6960/) | LAMPS | X.509 signatures (PQC sigs work but ~33x size increase; stapling adds to TLS overhead) |
| Kerberos (core) | [4120](https://datatracker.ietf.org/doc/rfc4120/) | Kitten | Symmetric core is quantum-safe; PQ exposure via PKINIT and SPAKE extensions |

---

## DJB Controversy and Pure ML-KEM Resolution

**Status (Aug 2026): Appeals exhausted, pragmatic split adopted.**

- **IAB denied DJB's final appeal Jun 30, 2026.** All IETF appeals governance levels exhausted.
- **Appeal chain:** IESG response (Oct 2025) → IAB response (Jan 2026) → IAB final denial (Jun 30).
- **DJB's last post:** "Understanding lattice risks" (Jun 30) — argues the security justification for excluding hybrid systematically excludes every documented attack category.
- **DJB research paper:** "Exploiting ML-DSA bugs" (Jun 2026, 59 pages) — implementation vulnerability analysis.

**The pragmatic resolution:**
- **Hybrid (X25519MLKEM768)** = default for everyone. **Recommended=Y**, Standards Track **RFC 10024**.
- **Pure ML-KEM** = compliance profile for CNSA 2.0 regimes. **Recommended=N**, **Informational** RFC (pending Sep 3 telechat).
- **ML-KEM-1024** = NSS-only mandate via CNSA 2.0; commercial default remains ML-KEM-768.
- DJB's objections are procedurally exhausted but technically unresolved — the lattice security question remains open-ended by nature.

**ML-KEM-1024 adoption:**
- CNSA 2.0 mandates ML-KEM-1024 (not 768): VPNs/networking support now, exclusive use by 2033.
- **Jan 1, 2027:** All new NSS acquisitions must be CNSA 2.0 compliant — hard procurement gate.
- RFC 10024 defines **SecP384r1MLKEM1024** (code point 4589) — the CNSA 2.0 hybrid target.
- Production deployment minimal: X25519MLKEM768 dominates (~95% per Cloudflare).
- ML-KEM-1024's 1,568-byte public key exceeds QUIC's 1,200-byte Initial packet limit.

---

## Real-World Deployment (August 2026)

### Key Exchange -- Production

| Platform | Status |
|----------|--------|
| **Cloudflare** | >65% of human TLS traffic uses hybrid ML-KEM. **Full PQ SASE** (TLS + MASQUE + IPsec) as of Feb 2026. PQ automatic for all origins mid-2026. ML-DSA support for origin connections **mid-2026**. MTC support **mid-2027**. Cloudflare One PQ auth **early 2028**. PQC roadmap accelerated to **2029 hard deadline**. |
| **Chrome** | X25519+ML-KEM default. ML-KEM disable option removed in Chrome 138. Kyber→ML-KEM transition complete. |
| **Firefox** | X25519MLKEM768 hybrid since Firefox 135. X25519+ML-KEM enabled by default since Firefox 132. |
| **Edge** | ML-KEM hybrid PQ TLS supported since Edge 131. |
| **Safari** | Apple PQ TLS 1.3 key exchange in macOS Tahoe 26 / iOS 26 / visionOS 26. **Corecrypto open-sourced** (May 22) with ML-KEM + ML-DSA. Formal verification via Cryptol, SAW, Isabelle. CryptoKit exposes ML-KEM-768/1024, ML-DSA-65/87. PQ in VPN (IKEv2), SSH, Apple Watch. |
| **Akamai** | PQ mid-tier connections completed across all networks Q1 2026. All Akamai-to-Akamai connections quantum safe. |
| **AWS** | ML-KEM PQ TLS in KMS, ACM, Secrets Manager. ML-DSA keys available via KMS APIs (GA). **IAM Roles Anywhere** supports ML-DSA-signed CA certificates (Mar 2026). **Secrets Manager** hybrid PQ TLS auto-enabled in Agent 2.0.0+. AWS-LC first FIPS-validated open-source crypto with ML-KEM. Pre-standard Kyber being removed 2026. |
| **Microsoft** | PQC APIs GA in Windows Server 2025, Windows 11 (24H2, 25H2), .NET 10. **AD CS ML-DSA support GA** (May 2026 KB5087539): ML-DSA-44/65/87 for CA cert signing, code signing, OCSP. Composite ML-DSA+ECDSA in Windows Insider. |
| **OpenSSH** | sntrup761x25519 default ~5 years. ML-KEM hybrid draft in RFC Ed Queue (second edit pass). |

### Signatures / Certificates -- Not Yet Deployed

- No public PQ certificates in production as of mid-2026.
- First PQ certificates expected late 2026; broad browser trust unlikely before 2027.
- **Merkle Tree Certificates (MTC):** Chrome will NOT use ML-DSA in traditional X.509. MTCs are Chrome's "preferred (or only) option." **Let's Encrypt committed Jun 3** — staging late 2026, production 2027. Phase 1 experiment with Cloudflare (~1,000 certs) underway. Phase 2 (CT Log operators) Q1 2027. Phase 3 (Chrome Quantum-resistant Root Store) Q3 2027. Full ecosystem rollout may take 10-15 years.
- **Google** set **2029 hard deadline** for full PQC migration. Android 17 integrates ML-DSA via KeyMint API.
- Signature migration is less urgent: active quantum adversary needed (vs. passive harvest-now for KEMs).
- **CA/Browser Forum SC-081v3:** Certificate validity 200 days (**active Mar 2026**), 100 days (Mar 2027), 47 days (Mar 2029) — enables crypto-agility for PQC algorithm adoption.
- **CA/B Forum SMC013 (S/MIME):** Enables single-key/non-hybrid PQC certificates for S/MIME experimentation (adopted Aug 2025).
- **DigiCert survey (Jul 2026):** 87% of organizations planning/piloting PQC, but only 7% deployed quantum-safe crypto broadly.

### Regulatory Drivers

| Regulation | Requirement |
|-----------|-------------|
| **US EO 14144** (Jan 2025) | Agencies list PQC-ready products, mandate support within 90 days |
| **US OMB M-26-15** (Jun 2026) | **NEW.** "Execution of the Migration to Post-Quantum Cryptography." Agencies must name PQC migration lead by late Jul, submit full migration plan by late Oct. Key establishment migrated by 2030, signatures by 2031, remaining by 2035. |
| **US Draft EO** (May 2026) | White House circulating draft EO: federal agencies migrate key establishment by Dec 31, 2030; digital signatures by Dec 31, 2031. Contractors face 2030 deadline. |
| **US DoD PQC Strategy** (Jun 2026) | Every DoD system must support PQC or be phased out by end of 2030, use PQC by end of 2031. |
| **NSA CNSA 2.0** | VPNs/routers: support and prefer CNSA 2.0 **now (2026)**. New acquisitions compliant by **Jan 2027**. ML-DSA exclusive by 2030; ML-KEM-1024 exclusive by 2033. **FN-DSA permanently excluded.** FIPS 140-2 certs Historical **Sep 2026**. |
| **NIST IR 8547** | Formal deprecation schedule: 15-year classical crypto phase-out roadmap |
| **EU NIS2 PQC Amendment** (COM(2026) 13, Jan 2026) | Proposes mandatory PQC transition policies for all EU Member States. In legislative process. 12-month transposition deadline after entry into force. Explicitly names "harvest now, decrypt later." |
| **EU Coordinated Roadmap** | National PQC strategies by end 2026; high-risk use cases by 2030; full completion by 2035. Led by BSI with 21 European partners. |
| **Germany BSI** | Full PQC migration for critical infrastructure by 2030, all other systems by 2032. Recommends ML-KEM, ML-DSA, SLH-DSA, FrodoKEM, Classic McEliece. **CRQC horizon shortened to 10-15 years** (was ~20). <5% of German orgs have migration plans. |
| **France ANSSI** | Recommends hybrid approaches during transition; three-phase timeline through 2030. Sector-specific requirements for critical infrastructure. |
| **Canada** | Treasury Board PQC procurement guidance live Apr 1, 2026. Federal departments must submit PQC migration plans by April 2026; critical systems by 2031; full migration by 2035. |
| **Australia ISM** | **NEW.** Recommends ceasing ALL traditional asymmetric crypto by end of 2030. Most aggressive national timeline. |
| **UAE** | National Cryptography Discovery Platform launched Jun 5, 2026 for PQC asset mapping and migration planning. |
| **South Korea** | First KCMVP PQC certifications granted (2026). MSIT expanding PQC pilot to telecom, finance, transportation, defense, space. Training 620 specialists Jul-Nov 2026. |
| **CA/B Forum SC-081v3** | Certificate validity: 200 days (**active** Mar 2026) → 100 days (Mar 2027) → 47 days (Mar 2029) |

---

## IETF Meetings

| Meeting | Location | Dates | PQC Notes |
|---------|----------|-------|-----------|
| **IETF 125** | Shenzhen | Mar 14-20, 2026 | Concluded. Key outcomes: composite sigs cleared IESG, ML-DSA TLS to Last Call. |
| **IETF 126** | Vienna | Jul 18-24, 2026 | **Concluded.** Hosted by Cisco. Key outcomes: CFRG formal poll on hybrid PQ/T sig adoption; TLS WG: PQC Continuity, PQ/T Dual Certs presented; CURRENT BoF (MLS-based two-party security protocol with PQC); PQC handshake testing initiative (MAPRG). Pure ML-KEM third WGLC concluded; chairs declared rough consensus Jul 19. |
| **IETF 127** | San Francisco | Nov 14-20, 2026 | Upcoming |

---

## Interoperability Testing

- [IETF Hackathon -- PQC Certificates](https://github.com/IETF-Hackathon/pqc-certificates)
- [IETF PQC Hackathon Interoperability Results](https://ietf-hackathon.github.io/pqc-certificates/pqc_hackathon_results_certs_r3.html)

---

## Related Files

- [PQC-Status-Report-2026-10-05.md](PQC-Status-Report-2026-10-05.md) -- Change summary and newly-tracked protocol surfaces (October 2026)
- [PQC-Status-Report-2026-08-21.md](PQC-Status-Report-2026-08-21.md) -- Change summary and per-draft analysis (August 2026)
- [PQC-Status-Report-2026-06-10.md](PQC-Status-Report-2026-06-10.md) -- Previous status report (June 2026)
- [PQC-Status-Report-2026-05-26.md](PQC-Status-Report-2026-05-26.md) -- Previous status report (May 2026)
- [PQC-Status-Report-2026-03-19.md](PQC-Status-Report-2026-03-19.md) -- Previous status report (March 2026)
