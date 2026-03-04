# State of Protocols and Post-Quantum Cryptography

> **Personal tracking fork** of [ietf-wg-pquip/state-of-protocols-and-pqc](https://github.com/ietf-wg-pquip/state-of-protocols-and-pqc)
>
> **Last updated:** 2026-03-04

This repository tracks the status of IETF drafts and RFCs related to post-quantum cryptography (PQC) migration across Internet protocols. It is a fork of the IETF PQUIP working group's official tracker, augmented with:

- Current draft statuses verified against the [IETF Datatracker](https://datatracker.ietf.org/)
- Working group criticism and controversy notes
- Real-world deployment status
- NIST standardization updates (FIPS 203/204/205/206, HQC, additional signature candidates)
- Regulatory driver context (CNSA 2.0, EO 14144, BSI, ANSSI)

For a detailed analysis with pros/cons for each draft and cross-WG debate summaries, see [PQC-Status-Report-2026-03-04.md](PQC-Status-Report-2026-03-04.md).

---

## At a Glance (March 2026)

| Area | Readiness | Key Signal |
|------|-----------|------------|
| KEM Key Exchange | **Production** | X25519+ML-KEM-768 on ~60% of web traffic |
| PQ Signatures in TLS | Unsolved | 14-39 KB handshake overhead; no production solution |
| PKI / Certificates | ~80% ready | Core algorithm bindings IESG-approved; composite debate ongoing |
| IPsec / IKEv2 | Advancing | ML-KEM adopted, PQC auth passed WG Last Call |
| SSH | Near complete | NTRU Prime in OpenSSH 5+ years; ML-KEM at IETF Last Call |
| OpenPGP | **Done** | IESG Approved as Proposed Standard |
| COSE / JOSE | Progressing | Signature drafts mature; KEM progressing; hybrid HPKE lagging |
| MLS | Early | Innovative combiner approach; WG adopted |

---

## NIST PQC Standards

| Standard | Algorithm | Basis | Status |
|----------|-----------|-------|--------|
| FIPS 203 | ML-KEM | CRYSTALS-Kyber | **Final** (Aug 2024) |
| FIPS 204 | ML-DSA | CRYSTALS-Dilithium | **Final** (Aug 2024) |
| FIPS 205 | SLH-DSA | SPHINCS+ | **Final** (Aug 2024) |
| FIPS 206 | FN-DSA | FALCON | Draft pending (expected late 2026) |
| -- | HQC | Error-correcting codes | Selected Mar 2025, backup KEM (~2027) |

**Additional Signature Schemes (Round 2):** 14 candidates -- CROSS, FAEST, HAWK, LESS, MAYO, Mirath, MQOM, PERK, QR-UOV, RYDE, SDitH, SNOVA, SQIsign, UOV. Round 3 down-select planned 2026.

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
| Additional LMS Parameter Sets | IRSG Evaluation (v-19) | [Datatracker](https://datatracker.ietf.org/doc/draft-fluhrer-lms-more-parm-sets/) | CFRG | LMS parameter sets | NIST co-author. 35-40% sig size reduction with 192-bit params. IANA codepoints assigned. |
| Hybrid KEM Combiners | Expired (superseded) | [Datatracker](https://datatracker.ietf.org/doc/draft-ounsworth-cfrg-kem-combiners/) | CFRG | Generic KEM combiners | Led to CFRG adopting draft-irtf-cfrg-hybrid-kems |
| X-Wing Hybrid KEM | Individual (v-09, Sep 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-connolly-cfrg-xwing-kem/) | CFRG | ML-KEM-768 + X25519 | NOT CFRG-adopted. Generic combiner framework preferred. FIPS cert unclear (awaiting SP 800-227). |
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
| ECDHE-MLKEM Hybrid KEX | **WG Adopted, Standards Track** (v-04, Feb 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-tls-ecdhe-mlkem/) | X25519MLKEM768, SecP256r1MLKEM768, SecP384r1MLKEM1024 | **PRODUCTION DEPLOYED.** ~60% of Cloudflare traffic. Default in Chrome/Firefox/Edge. |
| Pure ML-KEM KEX | **WG Adopted, IETF Last Call** (v-07, Feb 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-tls-mlkem/) | Non-hybrid ML-KEM for TLS | **CONTROVERSIAL.** DJB formal IETF appeals. Required for CNSA 2.0 by 2033. |
| ML-DSA Authentication | **WG Adopted** (v-01, Sep 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-tls-mldsa/) | ML-DSA for TLS 1.3 auth | Signature size crisis: ~17 KB added to handshakes exceeds TCP initcwnd. |
| SLH-DSA Authentication | Individual (v-02, Nov 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-reddy-tls-slhdsa/) | SLH-DSA for TLS 1.3 auth | Sigs 7,856-49,856 bytes. Adoption call issued May 2025. |
| Composite ML-DSA | Individual (v-09), likely expired | [Datatracker](https://datatracker.ietf.org/doc/draft-reddy-tls-composite-mldsa/) | Composite hybrid sigs in TLS | NOT adopted by WG. Doubles signature overhead. |
| Hybrid KEX Framework | WG Document (v-16, Sep 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-tls-hybrid-design/) | Hybrid key exchange design | Foundational. Concatenation-based. Informational. |
| PQ Guidance | Individual (v-04, Dec 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-farrell-tls-pqg/) | Deployment recommendations | Hybrid KEMs now; no action on sigs yet. Not a WG document. |
| KEMTLS (KEM-based Auth) | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-celi-wiggers-tls-authkem/) | KEM-based auth | Research-stage. Would eliminate PQ sig size problem. |
| X25519Kyber768Draft00 | Expired (superseded) | [Datatracker](https://datatracker.ietf.org/doc/draft-tls-westerbaan-xyber768d00) | Pre-standard Kyber hybrid | Superseded by draft-ietf-tls-ecdhe-mlkem |
| SecP256r1Kyber768Draft00 | Expired (superseded) | [Datatracker](https://datatracker.ietf.org/doc/draft-kwiatkowski-tls-ecdhe-kyber/) | ECDHE-Kyber for TLS | Superseded by draft-ietf-tls-ecdhe-mlkem |

### LAMPS / X.509 / CMS (PKI)

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| ML-DSA for X.509 | **IESG Approved** (v-13, Sep 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-lamps-dilithium-certificates/) | ML-DSA algorithm IDs for X.509 | In RFC Editor queue. Primary NIST PQC sig for PKI. |
| ML-KEM for X.509 | **IESG Approved** (v-11, Jul 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-lamps-kyber-certificates/) | ML-KEM algorithm IDs for X.509 | In RFC Editor queue. |
| Composite ML-DSA Sigs | WG Document (v-15, Feb 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-lamps-pq-composite-sigs/) | ML-DSA + traditional composite | OIDs early-allocated. Epicenter of composite debate. ANSSI/BSI favor; NSA opposes. |
| Composite ML-KEM | IESG Evaluation (v-12, Jan 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-lamps-pq-composite-kem/) | ML-KEM + traditional composite | Less contentious than composite sigs. |
| ML-KEM in CMS | IESG Ballot Active (v-13, Sep 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-lamps-cms-kyber/) | ML-KEM via KEMRecipientInfo | Near completion. |
| SLH-DSA in CMS | **RFC 9814** | [Datatracker](https://datatracker.ietf.org/doc/rfc9814/) | SLH-DSA in CMS | First PQC CMS signature RFC. |
| HSS/LMS in CMS | **RFC 8708** | [RFC](https://www.rfc-editor.org/rfc/rfc8708.html) | HSS/LMS in CMS | |
| Hash-Based Sigs for X.509 | IESG Evaluation (v-13) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-lamps-x509-shbs/) | HSS/LMS, XMSS OIDs | Stateful -- firmware/code signing only. |
| NIST PQC KEM Certs | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-turner-lamps-nist-pqc-kem-certificates/) | | Superseded by ML-KEM certs draft |

### IPsec / IKEv2

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| ML-KEM in IKEv2 | **WG Adopted** (v-04, Feb 2026), Standards Track | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-ipsecme-ikev2-mlkem/) | ML-KEM key exchange | Built on RFC 9370. Most mature PQ KEX for IKEv2. |
| PQC Auth in IKEv2 | **WG Adopted, Passed WG Last Call** (v-06, Oct 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-ipsecme-ikev2-pqc-auth/) | ML-DSA + SLH-DSA auth | Uses RFC 8420 framework. Furthest along for auth. |
| PQ/T Hybrid Auth | Individual (v-04, Feb 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-hu-ipsecme-pqt-hybrid-auth/) | Hybrid auth via composite/multi-cert | Not WG-adopted. |
| FrodoKEM in IKEv2 | Individual (v-03, Dec 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-wang-ipsecme-hybrid-kem-ikev2-frodo/) | FrodoKEM alternative KEM | Adoption call ended Feb 2026, 20+ support. BSI/ANSSI recommended. |

### SSH

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| ML-KEM Hybrid KEX | **IETF Last Call Complete** (v-09, Feb 2026), Standards Track | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-sshm-mlkem-hybrid-kex/) | mlkem768nistp256, mlkem1024nistp384, mlkem768x25519 | Gen-ART: "Ready with Nits." CNSA 2.0 compliant. Awaiting IESG approval. |
| NTRU Prime + X25519 | **IESG Approved** (v-06, Sep 2025), Informational | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-sshm-ntruprime-ssh/) | sntrup761x25519-sha512 | Default in OpenSSH ~5 years. NTRU Prime not a NIST winner. Appeal filed; IESG approved as Informational. |

### COSE / JOSE

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| ML-DSA in COSE/JOSE | WG Document (v-11, Nov 2025), Standards Track | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-cose-dilithium) | ML-DSA serialization | Defines AKP key type used across PQ COSE drafts. |
| SLH-DSA in COSE/JOSE | WG Document (v-06, Oct 2025), Standards Track | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-cose-sphincs-plus) | SLH-DSA serialization | Conservative hash-based. |
| PQ KEMs for JOSE/COSE | WG Document (v-05, Dec 2025), Standards Track | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-jose-pqc-kem/) | ML-KEM in JOSE/COSE | Fills KEM gap. |
| Hybrid HPKE in JOSE/COSE | Individual (v-11, Feb 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-reddy-cose-jose-pqc-hybrid-hpke/) | PQ/T hybrid HPKE | Still not WG-adopted after 11 revisions. |
| FALCON in COSE/JOSE | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-cose-falcon) | FALCON serialization | Awaiting FIPS 206 finalization. |
| COSE Kyber | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-steele-cose-kyber/) | Kyber encoding | Superseded by draft-ietf-jose-pqc-kem |
| HPKE KEM wrapper | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-ajitomi-cose-cose-key-jwk-hpke-kem/) | KEM wrapper for COSE | |
| PQ Sigs (omnibus) | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-cose-post-quantum-signatures/) | Multi-algo serializations | Split into per-algorithm drafts |
| Hybrid KEX in JOSE/COSE | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-ra-cose-hybrid-encrypt/) | KEM combiner hybrid | |
| HSS/LMS in COSE | **RFC 8778** | [Datatracker](https://datatracker.ietf.org/doc/rfc8778/) | HSS/LMS for COSE | |

### OpenPGP

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| PQC in OpenPGP | **IESG Approved, Proposed Standard** (v-17, Jan 2026) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-openpgp-pqc/) | PQC extension for OpenPGP | **Most advanced PQ draft in IETF.** Composite: ML-DSA-65+Ed25519 (MUST), ML-DSA-87+Ed448 (SHOULD). ML-KEM+ECDH. SLH-DSA standalone (MAY). BSI-backed. Multiple interop implementations. |

### MLS

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| Hybrid PQ MLS Combiner | **WG Adopted** (v-01, Jun 2025), Standards Track | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-mls-combiner/) | Combines traditional + PQ MLS sessions | Amortized PQ operations. PARTIAL/FULL modes. Anti-downgrade. |
| X-Wing for MLS | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-mahy-mls-xwing/) | X-Wing KEM ciphersuite | Superseded by combiner approach |

### ACME

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| ACME PQC Negotiation | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-giron-acme-pqcnegotiation/) | Algorithm negotiation | Unclear whether ACME benefits from this |

### DNSSEC

| Draft | Status | Link | Topic | Notes |
|-------|--------|------|-------|-------|
| Stateful HBS for DNSSEC | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-afrvrd-dnsop-stateful-hbs-for-dnssec/) | Hash-based sigs for DNSSEC | PQ sig sizes (7,856+ bytes) far exceed DNS UDP limits (~1,500 bytes) |

---

## PQC-Adjacent Improvements

Infrastructure work that enables or smooths PQC migration.

### Published RFCs

| Title | RFC | WG | Purpose |
|-------|-----|-----|---------|
| KEMRecipientInfo for CMS | **RFC 9629** (Aug 2024) | LAMPS | Foundation for all CMS KEM usage. Algorithm-agnostic. |
| Related Certificates for Multi-Auth | **RFC 9763** | LAMPS | Non-composite hybrid cert binding. NSA-authored. CNSA 2.0 aligned. |
| CMP v3 with KEM Support | **RFC 9810** (Jul 2025) | LAMPS | PKI enrollment infrastructure for PQC. Obsoletes RFC 4210/9480. |
| IKEv2 Intermediate Exchange | **RFC 9242** | IPSECME | Handles large PQC data in SA establishment. |
| IKEv2 Message Fragmentation | **RFC 7383** | IPSECME | Packet fragmentation for PQC key sizes. |
| Multiple Key Exchanges in IKEv2 | **RFC 9370** | IPSECME | Hybrid key exchange framework. Foundation for ML-KEM in IKEv2. |
| PSK Mixing in IKEv2 | **RFC 8784** | IPSECME | Post-quantum security via PSK mixing. |
| PSK in CMS | **RFC 8696** | LAMPS | Quantum-safe CMS via PSK. |
| PSK in TLS 1.3 | **RFC 8773** | TLS | Quantum-safe TLS via external PSK. |
| Large Certs in EAP | **RFC 9191** | EMU | Solutions for large PQC certificates in EAP. |

### Active Drafts

| Title | Status | Link | WG | Notes |
|-------|--------|------|----|-------|
| Merkle Tree Certificates | **PLANTS WG Adopted** (v-02, Mar 2026), Standards Track | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-plants-merkle-tree-certs/) | PLANTS | **Most promising PQ sig size solution.** <800 byte proofs vs ~17 KB sigs. Chrome "preferred option for PQ certs." Google+Cloudflare. |
| Hybrid KEX in TLS 1.3 | WG Document (v-16, Sep 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-tls-hybrid-design/) | TLS | Foundational framework. Concatenation-based. |
| TLS Key Share Prediction | WG Document (v-03, Aug 2025), Standards Track | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-tls-key-share-prediction/) | TLS | DNS-based keyshare signaling. Avoids PQ retransmission. Updates RFC 8446. |
| Chameleon Certificates | Individual (v-07, Oct 2025) | [Datatracker](https://datatracker.ietf.org/doc/draft-bonnell-lamps-chameleon-certs/) | LAMPS | Efficient paired cert encoding. Competing with composites/RFC 9763. |
| External Keys for X.509 | Expired (v-05, Apr 2025) | [Datatracker](https://datatracker.ietf.org/doc/html/draft-ounsworth-lamps-pq-external-pubkeys) | LAMPS | External key references by hash+URL. Limited interest. |
| Suppressing CA Certs in TLS | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-kampanakis-tls-scas-latest/) | TLS | Reduces PQ cert chain overhead. |
| Larger IKEv2 Payload | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-nir-ipsecme-big-payload/) | IPSECME | Increased payload for PQC keys. |
| Alt PSK Mixing in IKEv2 | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-smyslov-ipsecme-ikev2-qr-alt/) | IPSECME | Alternative PSK approach. |
| IKEv2 Auth Method Announce | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-ipsecme-ikev2-auth-announce/) | IPSECME | Advertise supported auth methods. |
| Hybrid Non-Composite Auth (IKEv2) | Expired (Sep 2022) | [Datatracker](https://datatracker.ietf.org/doc/draft-guthrie-ipsecme-ikev2-hybrid-auth/) | IPSECME | |
| Non-Composite Hybrid Auth (PKIX) | Expired (Sep 2022) | [Datatracker](https://datatracker.ietf.org/doc/draft-becker-guthrie-noncomposite-hybrid-auth/) | LAMPS | Superseded by RFC 9763. |
| Alt Composite Sigs | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-nir-lamps-altcompsigs/) | LAMPS | Alternative composition with strong non-separability. |

### PQUIP Working Group Documents

| Title | Status | Link | Notes |
|-------|--------|------|-------|
| PQC for Engineers | WG Adopted | [Datatracker](https://datatracker.ietf.org/doc/draft-ietf-pquip-pqc-engineers/) | Engineer-focused PQC primer. |
| PQ/T Hybrid Terminology | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-driscoll-pqt-hybrid-terminology/) | Standardized vocabulary. |
| Hybrid Signature Spectrums | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-hale-pquip-hybrid-signature-spectrums/) | Design goals/security analysis for hybrid sigs. |
| PQC Use Cases | Active | [Datatracker](https://datatracker.ietf.org/doc/draft-vaira-pquip-pqc-use-cases/) | Migration strategies. |
| Kerberos-over-TLS | Expired | [Datatracker](https://datatracker.ietf.org/doc/draft-vanrein-tls-kdh/) | Unclear PQC relevance. |

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
| SCEP | [8894](https://datatracker.ietf.org/doc/rfc8894/) | LAMPS | CMS, PKCS#10 |
| S/MIME | [5751](https://datatracker.ietf.org/doc/rfc5751/) | LAMPS | CMS (Sec 4.1 lists RSA/DSA/SHA-1 -- may need update) |

### Other

| Protocol | RFC | WG | Dependencies |
|----------|-----|-----|-------------|
| SRTP | [3711](https://datatracker.ietf.org/doc/rfc3711/) | AVT | DTLS (KEX via DTLS, symmetric AEAD for media) |

---

## Real-World Deployment (March 2026)

### Key Exchange -- Production

| Platform | Status |
|----------|--------|
| **Cloudflare** | ~60% of human TLS traffic uses hybrid ML-KEM. PQ automatic for all origins by Jun 2026. 6M+ domains. |
| **Chrome** | X25519+ML-KEM default since Aug 2023. Transitioning from pre-standard Kyber to final ML-KEM. |
| **Firefox** | Testing hybrid X25519+Kyber-768. |
| **AWS** | ML-KEM PQ TLS in KMS, ACM, Secrets Manager. Pre-standard Kyber support ends 2026. |
| **OpenSSH** | sntrup761x25519 default ~5 years. |

### Signatures / Certificates -- Not Yet Deployed

- No public PQ certificates in production as of early 2026.
- First PQ certificates expected 2026; broad browser trust unlikely before 2027.
- Merkle Tree Certificates: Cloudflare+Chrome feasibility study; Chrome CQRS targeted Q3 2027.

### Regulatory Drivers

| Regulation | Requirement |
|-----------|-------------|
| **US EO 14144** (Jan 2025) | Agencies list PQC-ready products, mandate support within 90 days |
| **NSA CNSA 2.0** | Software/firmware signing: exclusive ML-DSA by 2030; web/cloud: exclusive ML-KEM-1024 by 2033 |
| **Germany BSI** | PQC for critical infrastructure by June 2026 |
| **France ANSSI** | Recommends hybrid approaches during transition |

---

## Interoperability Testing

- [IETF Hackathon -- PQC Certificates](https://github.com/IETF-Hackathon/pqc-certificates)
- [IETF PQC Hackathon Interoperability Results](https://ietf-hackathon.github.io/pqc-certificates/pqc_hackathon_results_certs_r3.html)

---

## Related Files

- [PQC-Status-Report-2026-03-04.md](PQC-Status-Report-2026-03-04.md) -- Detailed per-draft analysis with pros/cons, WG criticisms, and cross-cutting debate summaries
