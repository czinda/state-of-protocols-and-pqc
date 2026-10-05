# PQC Status Report — August 21, 2026

> **Change summary** covering developments from June 10, 2026 to August 21, 2026.
> For the full protocol tracker, see [README.md](README.md).

---

## Executive Summary

The PQC landscape accelerated dramatically since the June 2026 update. Four new RFCs published, including the flagship **RFC 10024** (ECDHE-MLKEM hybrid key exchange for TLS). The DJB pure-vs-hybrid controversy reached procedural closure with the IAB denying the final appeal, leading to a pragmatic split: hybrid = recommended default, pure = informational compliance profile. IETF 126 in Vienna produced several new proposals and advanced multiple drafts. NIST Round 3 additional signatures lost HAWK to a lattice attack. Regulatory momentum continued with OMB M-26-15 and Australia's aggressive 2030 timeline.

**Critical date: September 3, 2026 IESG telechat** — both pure ML-KEM for TLS and composite ML-KEM for PKI are scheduled.

---

## New RFCs Published (Jun-Aug 2026)

| RFC | Title | Type | Published |
|-----|-------|------|-----------|
| **RFC 10024** | ECDHE-MLKEM Hybrid Key Exchange for TLS | Proposed Standard | Aug 2026 |
| **RFC 9980** | Post-Quantum Cryptography in OpenPGP | Proposed Standard | Jun 30, 2026 |
| **RFC 9958** | Post-Quantum Cryptography for Engineers | Informational | Jun 2026 |
| **RFC 9954** | Hybrid Key Exchange in TLS 1.3 | Informational | Jul 2026 |
| **RFC 9955** | Hybrid Signature Spectrums | Informational | 2026 |

---

## Major Draft Status Changes

### Graduated to RFC
- **ECDHE-MLKEM → RFC 10024**: The most widely deployed PQC protocol (>65% of Cloudflare traffic) now has a final RFC number. Proposed Standard. IANA code points registered with X25519MLKEM768 as **Recommended=Y**.
- **OpenPGP PQC → RFC 9980**: All four authors approved. Published Jun 30. First PQC extension for OpenPGP — ML-DSA, ML-KEM, SLH-DSA composite constructions.
- **TLS Hybrid Design → RFC 9954**: Unblocked by publication of RFC 9846. Framework document for hybrid key exchange.
- **PQC for Engineers → RFC 9958**: Engineer primer now a published Informational RFC.
- **Hybrid Signature Spectrums → RFC 9955**: Design goals and security analysis for hybrid signature approaches.

### Advanced to IESG Evaluation (Sep 3 telechat)
- **Pure ML-KEM for TLS (v-09)**: From "Revised I-D Needed" to IESG Evaluation. Three WGLCs completed. DJB appeals exhausted. "Has enough positions to pass." **Informational**, not Standards Track. All NamedGroups **Recommended=N**.
- **Composite ML-KEM (v-19)**: From stalled at AD (71+ days) to IESG Evaluation. Five new revisions (v-14→v-19). SECDIR/GENART reviews completed.

### Advanced to IESG Approved / IESG Evaluation
- **ML-DSA TLS Auth (v-05)**: IESG Approved. Awaiting AD (Deb Cooley) to release announcement (43+ days as action holder).
- **IKEv2 PQC Auth (v-12)**: Jumped from "Awaiting Write-Up" to IESG Evaluation with a DISCUSS. Has enough positions to pass once DISCUSS resolved.
- **PQUIP HSM/Constrained (v-06)**: Advanced to IESG Evaluation. "Has enough positions to pass." SECDIR flagged issues; AD action holder 31+ days.

### Advanced to RFC Editor Queue
- **IKEv2 ML-KEM (v-09)**: Cleared IESG. Awaiting first RFC Editor assignment.

### Advanced to Publication Requested
- **SLH-DSA in COSE/JOSE (v-10)**: Major jump from WG Document (v-07) to Publication Requested. Submitted to IESG as Proposed Standard.

### Newly WG-Adopted
- **FN-DSA for X.509**: Individual → WG Document (LAMPS).
- **FN-DSA in CMS**: Individual → WG Document (LAMPS).

### New Drafts
- **draft-yusef-tls-pqt-dual-certs** (v-00, Jul 2026): PQ/T Hybrid Auth with Dual Certificates in TLS 1.3. Presented at IETF 126.
- **draft-bokovoy-kitten-pkinit-pqc** (v-01, Jun 2026): ML-KEM for Kerberos PKINIT. The critical PKINIT PQC gap now has an active proposal.

### Expired / Dead
- **SLH-DSA TLS Auth**: Dropped from ISE review. Now expired/archived.
- **Chameleon Certificates**: Expired Apr 2026. Never WG-adopted. Appears dead as an approach.

---

## DJB / Pure ML-KEM Resolution

**IAB denied DJB's final IETF appeal on June 30, 2026.** All formal IETF governance levels exhausted.

The community landed on a pragmatic split:
- **Hybrid (X25519MLKEM768)** = recommended default. Standards Track **RFC 10024**, Recommended=Y.
- **Pure ML-KEM** = compliance profile. Informational RFC (pending Sep 3 telechat), Recommended=N.
- **ML-KEM-1024** = NSS-only via CNSA 2.0; commercial world uses ML-KEM-768.

Key conditions that enabled the pure ML-KEM draft to advance:
1. Hybrid promoted to Recommended=Y first (RFC 10024).
2. Key share reuse changed from SHOULD NOT to MUST NOT.
3. Liaison statements from O-RAN, IEEE 802.11, 3GPP requesting stable reference.
4. Classified as Informational (not Standards Track).

DJB's "Understanding lattice risks" blog post (Jun 30) argues the security framework for excluding hybrid systematically discounts every documented attack category. His 59-page "Exploiting ML-DSA bugs" paper details implementation vulnerabilities. The technical debate is unresolved; only the procedural path is closed.

---

## NIST Updates

- **FIPS 206 (FN-DSA)**: Still in DoC clearance. Final standard now estimated late 2026-early 2027.
- **HAWK withdrawn Jul 29, 2026**: Lattice automorphism attack discovered that halved effective key strength. HAWK team withdrew; parameter doubling would make it uncompetitive. **8 candidates remain**: FAEST, MAYO, MQOM, QR-UOV, SDitH, SNOVA, SQIsign, UOV.
- **Specification tweaks due Aug 14, 2026** for remaining candidates.
- **7th PQC Conference**: Planned late spring/early summer 2027 in Gaithersburg, MD.

---

## IETF 126 (Vienna, Jul 18-24, 2026) Outcomes

- **CFRG**: Formal poll on adopting hybrid PQ/T signatures. Presentations on Silithium (non-separable hybrid sigs), PQuAKE (compact PQ mutual auth), hybrid PAKE.
- **TLS WG** (Jul 24): PQC Continuity/downgrade protection presented (v-02). PQ/T Dual Certificates in TLS 1.3 (new draft by Tschofenig/Shekh-Yusef). Pure ML-KEM third WGLC concluded; chairs declared rough consensus Jul 19.
- **CURRENT BoF**: New BoF proposing WG for two-party security protocol using MLS for key management, explicitly supporting PQC.
- **PQC Handshake Testing Initiative**: Nalini Elkins proposing enterprise test websites to measure PQC handshake overhead (MAPRG session).

---

## Regulatory Updates

- **OMB M-26-15** (Jun 2026): "Execution of the Migration to Post-Quantum Cryptography." Name PQC lead by late Jul, migration plan by late Oct. Key establishment by 2030, signatures by 2031, remaining by 2035.
- **US Draft EO** (May 2026): Federal key establishment by Dec 31, 2030; signatures by Dec 31, 2031. Contractors face 2030 deadline.
- **US DoD PQC Strategy** (Jun 2026): All systems support PQC by end 2030, use PQC by end 2031.
- **Australia ISM**: Cease ALL traditional asymmetric crypto by end 2030. Most aggressive national timeline.
- **South Korea KCMVP**: First PQC certifications (Exgate, ITCEN PNS). Pilot expanding to telecom, finance, transportation, defense, space. 620 specialists training Jul-Nov 2026.
- **Canada**: Treasury Board PQC procurement guidance live Apr 1, 2026.

---

## Deployment Updates

- **DigiCert survey** (Jul 2026): 87% of organizations planning/piloting PQC, but only 7% have deployed quantum-safe crypto broadly.
- **FIPS 140-2 → Historical**: Sep 2026 — convergence with CNSA 2.0 and EU deadlines.
- **Apple corecrypto**: Open-sourced May 22 with ML-KEM + ML-DSA. Formal verification caught implementation bug.
- **Microsoft AD CS**: ML-DSA GA (May 2026 KB5087539). Composite ML-DSA+ECDSA in Windows Insider.

---

## Upcoming Key Dates

| Date | Event |
|------|-------|
| **Sep 3, 2026** | IESG telechat: Pure ML-KEM TLS + Composite ML-KEM |
| **Sep 2026** | FIPS 140-2 → Historical status |
| **Late Oct 2026** | OMB M-26-15: Agency migration plans due |
| **Nov 14-20, 2026** | IETF 127 (San Francisco) |
| **Jan 1, 2027** | CNSA 2.0: All new NSS acquisitions must be compliant |
| **Late 2026-early 2027** | FIPS 206 (FN-DSA) expected final |
| **Spring/Summer 2027** | NIST 7th PQC Conference (Gaithersburg) |
| **Late 2026** | Let's Encrypt MTC staging environment |
| **Dec 2026** | MLS combiner WG milestone |

---

## Per-Protocol Status Summary (August 2026)

| Protocol Area | Key Drafts | Current Status |
|--------------|------------|----------------|
| **TLS KEX** | RFC 10024 (hybrid), v-09 (pure) | **RFC** / **IESG telechat Sep 3** |
| **TLS Auth** | v-05 (ML-DSA) | **IESG Approved** (AD followup) |
| **PKI Composite Sigs** | v-19 | **RFC Ed Queue** |
| **PKI Composite KEM** | v-19 | **IESG telechat Sep 3** |
| **PKI CMS Composite** | v-05 sigs, v-01 KEM | **RFC Ed Queue** / **Waiting for AD** |
| **IPsec KEX** | v-09 | **RFC Ed Queue** |
| **IPsec Auth** | v-12 | **IESG Evaluation** (DISCUSS) |
| **SSH KEX** | v-10 | **RFC Ed Queue** (second edit) |
| **OpenPGP** | RFC 9980 | **Published** |
| **COSE/JOSE** | RFC 9964, v-10 SLH-DSA | **RFC** / **Publication Requested** |
| **HPKE** | v-05 | WG Document |
| **MLS** | v-06 ciphersuites, v-02 combiner | **Revised I-D** / **Expired** |
| **EAP** | v-02 | **Waiting for WG Chair** |
| **Kerberos/PKINIT** | v-01 (new!) | **Individual draft** |
| **PLANTS/MTC** | v-05 | **WG Document** |
