# Protocol-independent algorithm or cryptography specifications

> **Last updated:** 2026-03-04

| Draft title | Status | Link | Working Group and/or protocol | Topic | Comments |
|------------ |------- |----- |------------------------------ |------ |--------- |
| Additional Parameter sets for LMS Hash-Based Signatures | IRSG Evaluation (v-19, Feb 2025) | <https://datatracker.ietf.org/doc/draft-fluhrer-lms-more-parm-sets/> | CFRG | Parameter sets for the LMS signature primitive | NIST co-author. Adds SHA-256/192, SHAKE256/256, SHAKE256/192. IANA codepoints already assigned. Provides 35-40% signature size reduction with 192-bit params. |
| Combiner function for hybrid key encapsulation mechanisms (Hybrid KEMs) | Expired (superseded) | <https://datatracker.ietf.org/doc/draft-ounsworth-cfrg-kem-combiners/> | CFRG | | This draft led to the CFRG adopting draft-irtf-cfrg-hybrid-kems as a generic combiner framework |
| X-Wing: general-purpose hybrid post-quantum KEM | Active Individual (v-09, Sep 2025) | <https://datatracker.ietf.org/doc/draft-connolly-cfrg-xwing-kem/> | CFRG | ML-KEM-768 + X25519 hybrid KEM | NOT adopted by CFRG -- they preferred the generic combiner framework. Multiple implementations exist. OpenJDK won't implement until RFC. FIPS certification unclear (awaiting SP 800-227). |
| Hybrid Streamlined NTRU Prime sntrup761 and X25519 with SHA-512: Chempat-X | Expired | <https://datatracker.ietf.org/doc/draft-josefsson-ntruprime-hybrid/> | Independent / CFRG | Hybrids of Streamlined NTRU Prime with X25519 | |
| Kyber Post-Quantum KEM | Expired | <https://datatracker.ietf.org/doc/draft-cfrg-schwabe-kyber/> | CFRG | Description of the Kyber algorithm | No longer needed now that FIPS 203 (ML-KEM) is final (published Aug 2024) |
| Leighton-Micali Hash-Based Signatures | RFC | <https://www.rfc-editor.org/rfc/rfc8554> | CFRG | | RFC |
| Streamlined NTRU Prime: sntrup761 | Expired | <https://datatracker.ietf.org/doc/draft-josefsson-ntruprime-streamlined/> | Independent / CFRG | Streamlined NTRU Prime KEM | |
| NTRU Key Encapsulation | Expired | <https://datatracker.ietf.org/doc/draft-fluhrer-cfrg-ntru/> | CFRG | Description of the NTRU algorithm | |
| Quantum Safe Cryptography Key Information for CRYSTALS-Kyber | Expired | <https://datatracker.ietf.org/doc/draft-uni-qsckeys-kyber/> | Individual | Kyber encodings | No longer needed now that FIPS 203 is final |
| Quantum Safe Cryptography Key Information for CRYSTALS-Dilithium | Expired | <https://datatracker.ietf.org/doc/draft-uni-qsckeys-dilithium/> | Individual | Dilithium encodings | No longer needed now that FIPS 204 is final |
| Quantum Safe Cryptography Key Information for FALCON | Expired | <https://datatracker.ietf.org/doc/draft-uni-qsckeys-falcon/> | Individual | FALCON encodings | No longer needed now that FIPS 206 (FN-DSA) is in draft |
| Quantum Safe Cryptography Key Information for SPHINCS-PLUS | Expired | <https://datatracker.ietf.org/doc/draft-uni-qsckeys-sphincsplus/> | Individual | SPHINCS+ encodings | No longer needed now that FIPS 205 is final |
| X25519Kyber768Draft00 hybrid post-quantum KEM for HPKE | Expired | <https://datatracker.ietf.org/doc/draft-westerbaan-cfrg-hpke-xyber768d00/> | CFRG | Adds Kyber support (as a hybrid with X25519) to HPKE RFC 9180 | Superseded. This was for the NIST Round 3 Draft version of Kyber for prototyping |
| XMSS: eXtended Merkle Signature Scheme | RFC | <https://www.rfc-editor.org/rfc/rfc8391> | CFRG | | RFC |
| Merkle Tree Ladder Mode (MTL) Signatures | Active | <https://datatracker.ietf.org/doc/draft-harvey-cfrg-mtl-mode/> | CFRG / DNS | Amortize large PQ signatures across a large evolving series of messages | Being explored for DNSSEC use (draft-fregly-dnsop-slh-dsa-mtl-dnssec) |



# PQC support in protocols and migration techniques

| WG / Protocol | Draft title | Status | Link | Topic | Comments |
|------------ |------- |----- |------------------------------ |------ |--------- |
| ACME | ACME PQC Algorithm Negotiation | Expired | <https://datatracker.ietf.org/doc/draft-giron-acme-pqcnegotiation/> | Adds algorithm negotiation to ACME, including adding KEM PoP mechanisms | From on-list discussion, unclear whether ACME actually benefits from this. |
| COSE | COSE Key and JSON Web Key Representation for Key Encapsulation Mechanism (KEM) of Hybrid Public Key Encryption (HPKE) | Expired | <https://datatracker.ietf.org/doc/draft-ajitomi-cose-cose-key-jwk-hpke-kem/> | KEM wrapper for COSE | |
| COSE | COSE Kyber | Expired | <https://datatracker.ietf.org/doc/draft-steele-cose-kyber/> | Defines encoding and registry for Kyber | Superseded by draft-ietf-jose-pqc-kem |
| COSE | JOSE and COSE Encoding for Post-Quantum Signatures | Expired | <https://datatracker.ietf.org/doc/draft-ietf-cose-post-quantum-signatures/> | JSON and CBOR serializations for several Post-Quantum Cryptography (PQC) based suites including CRYSTALS Dilithium, Falcon, and SPHINCS+. | No longer current. Split into one draft per signature suite -- see relevant COSE items below |
| COSE | JOSE and COSE Encoding for ML-DSA | WG Document (v-11, Nov 2025), Standards Track | <https://datatracker.ietf.org/doc/draft-ietf-cose-dilithium> | ML-DSA (FIPS 204) in COSE/JOSE | Foundational -- defines the Algorithm Key Pair (AKP) key type used by companion PQ drafts. Uses seed-only private key format (32 bytes). |
| COSE | JOSE and COSE Encoding for Falcon | Expired | <https://datatracker.ietf.org/doc/draft-ietf-cose-falcon> | FALCON in COSE/JOSE | Awaiting FIPS 206 (FN-DSA) finalization |
| COSE | JOSE and COSE Encoding for SPHINCS+ | WG Document (v-06, Oct 2025), Standards Track | <https://datatracker.ietf.org/doc/draft-ietf-cose-sphincs-plus> | SLH-DSA (FIPS 205) in COSE/JOSE | Hash-based -- most conservative security assumptions. Builds on AKP key type from ML-DSA draft. |
| COSE | PQ/T Hybrid KEM: HPKE with JOSE/COSE | Individual (v-11, Feb 2026), NOT adopted | <https://datatracker.ietf.org/doc/draft-reddy-cose-jose-pqc-hybrid-hpke/> | HPKE PQ/T Hybrids in COSE/JOSE | 11 revisions but still not WG-adopted. Requesting second adoption call. Depends on COSE-HPKE, JOSE-HPKE, HPKE-PQ drafts. |
| COSE | Use of the HSS/LMS Hash-Based Signature Algorithm with CBOR Object Signing and Encryption (COSE) | RFC | <https://datatracker.ietf.org/doc/rfc8778/> | HSS / LMS in COSE | |
| COSE | Hybrid key exchange in JOSE and COSE | Expired | <https://datatracker.ietf.org/doc/draft-ra-cose-hybrid-encrypt/> | Using KEM combiners to compute Hybrid (PQC + Traditional) shared secret | |
| DNSOP | Stateful Hash-based Signatures For DNSSEC | Expired | <https://datatracker.ietf.org/doc/draft-afrvrd-dnsop-stateful-hbs-for-dnssec/> | Stateful Hash-based Signatures For DNSSEC | |
| IPSECME / IPsec / IKEv2 | ML-KEM in IKEv2 | **WG Adopted** (v-04, Feb 2026), Standards Track | <https://datatracker.ietf.org/doc/draft-ietf-ipsecme-ikev2-mlkem/> | ML-KEM for IKEv2 key exchange | Supersedes individual draft-kampanakis-ml-kem-ikev2. Built on RFC 9370 (Multiple Key Exchanges). Most mature PQ KEX draft for IKEv2. |
| IPSECME / IPsec / IKEv2 | PQC Signature Authentication in IKEv2 | **WG Adopted, Passed WG Last Call** (v-06, Oct 2025), Standards Track | <https://datatracker.ietf.org/doc/draft-ietf-ipsecme-ikev2-pqc-auth/> | ML-DSA and SLH-DSA authentication in IKEv2 | Supersedes individual draft-reddy-ipsecme-ikev2-pqc-auth. Uses RFC 8420 framework. Furthest along among IKEv2 auth drafts. |
| IPSECME / IPsec / IKEv2 | Post-Quantum Traditional (PQ/T) Hybrid PKI Authentication in IKEv2 | Individual (v-04, Feb 2026) | <https://datatracker.ietf.org/doc/draft-hu-ipsecme-pqt-hybrid-auth/> | PQ/T hybrid auth via composite or multiple certificates | Not yet WG-adopted. Lacks guidance on algorithm combination selection per Fluhrer review. |
| IPSECME / IPsec / IKEv2 | FrodoKEM in IKEv2 | Individual (v-03, Dec 2025), Call for Adoption ended Feb 2026 | <https://datatracker.ietf.org/doc/draft-wang-ipsecme-hybrid-kem-ikev2-frodo/> | FrodoKEM as alternative PQ KEM for IKEv2 | 20+ supportive comments in adoption call. Plain LWE (more conservative than structured lattices). Recommended by BSI/ANSSI. ISO standardization expected 2026 Q1. |
| JOSE | Post-Quantum Key Encapsulation Mechanisms (PQ KEMs) for JOSE and COSE | WG Document (v-05, Dec 2025), Standards Track | <https://datatracker.ietf.org/doc/draft-ietf-jose-pqc-kem/> | ML-KEM in COSE/JOSE | Fills the key encapsulation gap for JOSE/COSE. Steady progression. |
| LAMPS / X.509 | Algorithm Identifiers for Hash-based Signatures for Use in the Internet X.509 PKI | IESG Evaluation (v-13, Dec 2024) | <https://datatracker.ietf.org/doc/draft-ietf-lamps-x509-shbs/> | OIDs for HSS/LMS, XMSS in PKIX | Stateful hash-based sigs -- suitable for firmware/code signing, not general CA use. |
| LAMPS / X.509 | Algorithm Identifiers for NIST's PQC Algorithms for Use in the Internet X.509 PKI | Expired | <https://datatracker.ietf.org/doc/draft-turner-lamps-nist-pqc-kem-certificates/> | | Superseded by draft-ietf-lamps-kyber-certificates |
| LAMPS / X.509 | Internet X.509 PKI: Algorithm Identifiers for ML-DSA | **IESG Approved** (v-13, Sep 2025), Proposed Standard | <https://datatracker.ietf.org/doc/draft-ietf-lamps-dilithium-certificates/> | ML-DSA (FIPS 204) algorithm IDs for X.509 certificates | In RFC Editor queue. Primary NIST PQC signature standard for PKI. |
| LAMPS / X.509 | Internet X.509 PKI: Algorithm Identifiers for ML-KEM | **IESG Approved** (v-11, Jul 2025), Proposed Standard | <https://datatracker.ietf.org/doc/draft-ietf-lamps-kyber-certificates/> | ML-KEM (FIPS 203) algorithm IDs for X.509 certificates | In RFC Editor queue. |
| LAMPS / X.509 | Composite ML-DSA for use in Internet PKI | WG Document (v-15, Feb 2026), OIDs early-allocated | <https://datatracker.ietf.org/doc/draft-ietf-lamps-pq-composite-sigs/> | Composite PQ/T hybrid signatures (ML-DSA + RSA/ECDSA/Ed25519/Ed448) | Epicenter of composite vs. non-composite debate. Aligned with ANSSI/BSI hybrid recommendations. NSA/CNSA 2.0 does NOT recommend hybrid. |
| LAMPS / X.509 | Composite ML-KEM for Use in X.509 PKI and CMS | IESG Evaluation (v-12, Jan 2026) | <https://datatracker.ietf.org/doc/draft-ietf-lamps-pq-composite-kem/> | Composite PQ/T hybrid KEM (ML-KEM + RSA-OAEP/ECDH/X25519/X448) | Less contentious than composite sigs since hybrid KEM is broadly accepted. |
| LAMPS / CMS | Use of ML-KEM in the Cryptographic Message Syntax (CMS) | IESG Ballot Active (v-13, Sep 2025) | <https://datatracker.ietf.org/doc/draft-ietf-lamps-cms-kyber/> | ML-KEM in CMS using KEMRecipientInfo (RFC 9629) | Covers ML-KEM-512/768/1024. Near completion. |
| LAMPS / CMS | Use of the HSS/LMS Hash-Based Signature Algorithm in the CMS | RFC | <https://www.rfc-editor.org/rfc/rfc8708.html> | | |
| LAMPS / CMS | Use of the SLH-DSA Signature Algorithm in the CMS | **RFC 9814** | <https://datatracker.ietf.org/doc/rfc9814/> | SLH-DSA (FIPS 205) in CMS | First PQC CMS signature algorithm to reach RFC status. Stateless hash-based. |
| MLS | MLS Ciphersuite using X-Wing KEM | Expired | <https://datatracker.ietf.org/doc/draft-mahy-mls-xwing/> | X25519Kyber768 / X-Wing for MLS | Superseded by draft-ietf-mls-combiner approach |
| MLS | Flexible Hybrid PQ MLS Combiner | **WG Adopted** (v-01, Jun 2025), Standards Track | <https://datatracker.ietf.org/doc/draft-ietf-mls-combiner/> | Combines traditional MLS session with a PQ session | Innovative amortized approach -- PQ operations less frequent than traditional. Two modes: PARTIAL (traditional-only) and FULL (hybrid PQ). Anti-downgrade protections. Companion draft-ietf-mls-pq-ciphersuites defines ML-KEM cipher suites. |
| OpenPGP | Post-Quantum Cryptography in OpenPGP | **IESG Approved as Proposed Standard** (v-17, Jan 2026) | <https://datatracker.ietf.org/doc/draft-ietf-openpgp-pqc/> | PQC extension for OpenPGP | **Most advanced PQ draft in all of IETF.** Composite sigs: ML-DSA-65+Ed25519 (MUST), ML-DSA-87+Ed448 (SHOULD). Composite encryption: ML-KEM+ECDH. SLH-DSA standalone: MAY. Multiple interoperable implementations. BSI-backed. |
| TLS | KEM-based Authentication for TLS 1.3 | Expired | <https://datatracker.ietf.org/doc/draft-celi-wiggers-tls-authkem/> | KEM-based authentication (KEMTLS) | Research-stage alternative to signature-based TLS auth. Would eliminate PQ signature size problem. |
| TLS | ML-KEM Post-Quantum Key Agreement for TLS 1.3 | **WG Adopted, IETF Last Call** (v-07, Feb 2026), Informational | <https://datatracker.ietf.org/doc/draft-ietf-tls-mlkem/> | Adds pure (non-hybrid) ML-KEM to TLS | **HIGHLY CONTROVERSIAL.** DJB filed formal IETF appeals alleging false consensus. DJB moderated from TLS list during Last Call. Required for CNSA 2.0 compliance by 2033. Ongoing "NSA and IETF" blog series. |
| TLS | X25519Kyber768Draft00 hybrid post-quantum key agreement | Expired (superseded) | <https://datatracker.ietf.org/doc/draft-tls-westerbaan-xyber768d00> | X25519Kyber768 for TLS 1.3 | Superseded by draft-ietf-tls-ecdhe-mlkem. Pre-standard Kyber version. Cloudflare phasing out in favor of final ML-KEM. |
| TLS | Post-quantum hybrid ECDHE-MLKEM Key Agreement for TLSv1.3 | **WG Adopted** (v-04, Feb 2026), **Standards Track** | <https://datatracker.ietf.org/doc/draft-ietf-tls-ecdhe-mlkem/> | X25519MLKEM768, SecP256r1MLKEM768, SecP384r1MLKEM1024 | **PRODUCTION DEPLOYED.** ~60% of Cloudflare traffic. Default in Chrome/Firefox/Edge. Promoted from Informational to Standards Track. No adoption controversy. |
| TLS | SecP256r1Kyber768Draft00 hybrid post-quantum key agreement | Expired (superseded) | <https://datatracker.ietf.org/doc/draft-kwiatkowski-tls-ecdhe-kyber/> | ECDHE-Kyber for TLS 1.3 | Superseded by draft-ietf-tls-ecdhe-mlkem |
| TLS | Post-Quantum Guidance for TLS | Individual (v-04, Dec 2025), Best Current Practice | <https://datatracker.ietf.org/doc/draft-farrell-tls-pqg/> | Recommendations for deploying PQC in TLS | Title broadened to cover all IETF protocols. Recommends hybrid KEMs now; take no action on PQ signatures yet. Not a WG document. |
| TLS | Use of ML-DSA in TLS 1.3 | **WG Adopted** (v-01, Sep 2025), Informational | <https://datatracker.ietf.org/doc/draft-ietf-tls-mldsa/> | ML-DSA (FIPS 204) for TLS 1.3 authentication | Signature size crisis: ML-DSA-65 sigs are 3,309 bytes. Certificate chains add ~17 KB, exceeding TCP initcwnd (~14 KB). TLS 1.2 use explicitly prohibited. |
| TLS | Use of Composite ML-DSA in TLS 1.3 | Individual (v-09), likely expired Dec 2025 | <https://datatracker.ietf.org/doc/draft-reddy-tls-composite-mldsa/> | Composite ML-DSA + traditional sigs in TLS 1.3 | NOT adopted by WG. Doubles already-excessive signature overhead. TLS WG reluctant given signature size crisis. |
| TLS | Use of SLH-DSA in TLS 1.3 | Individual (v-02, Nov 2025), Call for Adoption issued May 2025 | <https://datatracker.ietf.org/doc/draft-reddy-tls-slhdsa/> | SLH-DSA (FIPS 205) for TLS 1.3 authentication | Hash-based only (no lattice assumptions). Signatures 7,856-49,856 bytes -- impractical for most TLS use. Adoption outcome unclear. |
| SSH | Post-quantum Hybrid Key Exchange in SSH | **WG Adopted, IETF Last Call complete** (v-09, Feb 2026), Standards Track | <https://datatracker.ietf.org/doc/draft-ietf-sshm-mlkem-hybrid-kex/> | mlkem768nistp256, mlkem1024nistp384, mlkem768x25519 | Supersedes individual draft-kampanakis-curdle-ssh-pq-ke. Gen-ART review: "Ready with Nits." CNSA 2.0 compliant. Likely awaiting IESG approval. |
| SSH | SSH using Hybrid Streamlined NTRU Prime sntrup761 and X25519 with SHA-512 | **IESG Approved** (v-06, Sep 2025), Informational RFC | <https://datatracker.ietf.org/doc/draft-ietf-sshm-ntruprime-ssh/> | sntrup761x25519-sha512 | Default in OpenSSH for ~5 years. Formal appeal filed over publishing RFC for non-NIST algorithm. IESG approved as Informational acknowledging deployment reality. NTRU Prime NOT a NIST winner. |


# Improvements adjacent to PQC

| Draft title | Status | Link | Working Group and/or protocol | Topic | Comments |
|------------ |------- |----- |------------------------------ |------ |--------- |
| Handling Large Certificates and Long Certificate Chains in TLS-Based EAP Methods | RFC 9191 | <https://datatracker.ietf.org/doc/rfc9191/> | EMU | Explores problems and solutions related to large PQC certificates in EAP | RFC |
| A Larger Internet Key Exchange version 2 (IKEv2) Payload | Active | <https://datatracker.ietf.org/doc/draft-nir-ipsecme-big-payload/> | IPSECME | Increase payload size to handle PQC keys | |
| Alternative Approach for Mixing Preshared Keys in IKEv2 for Post-quantum Security | Active | <https://datatracker.ietf.org/doc/draft-smyslov-ipsecme-ikev2-qr-alt/> | IPSECME | | |
| Announcing Supported Authentication Methods in IKEv2 | Active | <https://datatracker.ietf.org/doc/draft-ietf-ipsecme-ikev2-auth-announce/> | IPSECME | Allows IKEv2 implementations to indicate the list of supported authentication methods | |
| Hybrid Non-Composite Authentication in IKEv2 | Expired 2022-09-26 | <https://datatracker.ietf.org/doc/draft-guthrie-ipsecme-ikev2-hybrid-auth/> | IPSECME | Non-composite hybrid authentication for IKEv2 | |
| Intermediate Exchange in the Internet Key Exchange Protocol Version 2 (IKEv2) | RFC 9242 | <https://datatracker.ietf.org/doc/rfc9242/> | IPSECME | Defines a new "Intermediate Exchange" to handle large amounts of data in SA establishment to handle PQC keys | RFC |
| Internet Key Exchange Protocol Version 2 (IKEv2) Message Fragmentation | RFC 7383 | <https://datatracker.ietf.org/doc/rfc7383/> | IPSECME | Packet fragmentation on larger packets to handle PQC keys | RFC |
| Using Pre-Shared Key (PSK) in the Cryptographic Message Syntax (CMS) | RFC 8696 | <https://datatracker.ietf.org/doc/rfc8696/> | LAMPS | Use PSK in CMS to be quantum safe | RFC |
| External Keys And Signatures For Use In Internet PKI | Expired (v-05, Apr 2025) | <https://datatracker.ietf.org/doc/html/draft-ounsworth-lamps-pq-external-pubkeys> | LAMPS | Certificate contains link+hash of public key instead of entire object | Limited adoption interest. Size concerns better addressed by other mechanisms. |
| TLS 1.3 Extension for Certificate-Based Authentication with an External Pre-Shared Key | RFC 8773 | <https://datatracker.ietf.org/doc/rfc8773/> | TLS | Use PSK in TLS 1.3 to be quantum safe | RFC |
| Mixing Preshared Keys in the Internet Key Exchange Protocol Version 2 (IKEv2) for Post-quantum Security | RFC 8784 | <https://datatracker.ietf.org/doc/rfc8784/> | IPSECME | | RFC |
| Multiple Key Exchanges in IKEv2 | RFC 9370 | <https://datatracker.ietf.org/doc/rfc9370/> | IPSECME | Defines multiple (hybrid) key exchange, with applications to PQ among other things | RFC. Foundation for draft-ietf-ipsecme-ikev2-mlkem. |
| Non-Composite Hybrid Authentication in PKIX and Applications to Internet Protocols | Expired 2022-09-26 | <https://datatracker.ietf.org/doc/draft-becker-guthrie-noncomposite-hybrid-auth/> | LAMPS | Non-composite hybrid authentication for X.509 | Superseded by RFC 9763 (Related Certificates) |
| Related Certificates for Use in Multiple Authentications within a Protocol | **RFC 9763** | <https://datatracker.ietf.org/doc/rfc9763/> | LAMPS | Bind certs across non-composite parallel PKIs | NSA-authored (Becker, Guthrie, Jenkins). Aligned with CNSA 2.0 non-composite hybrid approach. |
| Hybrid key exchange in TLS 1.3 | WG Document (v-16, Sep 2025), Informational | <https://datatracker.ietf.org/doc/draft-ietf-tls-hybrid-design/> | TLS | Hybrid TLS key exchange framework | Foundational document referenced by draft-ietf-tls-ecdhe-mlkem. Concatenation-based design. 16 revisions. |
| Suppressing CA Certificates in TLS 1.3 | Active | <https://datatracker.ietf.org/doc/draft-kampanakis-tls-scas-latest/> | TLS | TLS authentication and certificate chain | Helps reduce PQ certificate chain overhead |
| Terminology for Post-Quantum Traditional Hybrid Schemes | Active | <https://datatracker.ietf.org/doc/draft-driscoll-pqt-hybrid-terminology/> | PQUIP | | |
| Post-Quantum Cryptography for Engineers | WG Adopted | <https://datatracker.ietf.org/doc/draft-ietf-pquip-pqc-engineers/> | PQUIP | This document explains why engineers need to be aware of and understand post-quantum cryptography | Adopted by the WG |
| Quantum Relief with TLS and Kerberos | Expired | <https://datatracker.ietf.org/doc/draft-vanrein-tls-kdh/> | Individual | Kerberos-over-TLS. | Unclear that this is directly PQC-related |
| Using Key Encapsulation Mechanism (KEM) Algorithms in the Cryptographic Message Syntax (CMS) | **RFC 9629** (Aug 2024) | <https://datatracker.ietf.org/doc/rfc9629/> | LAMPS | Adds KEMRecipientInfo to CMS, to enable PQ KEMs | Foundation for all CMS KEM usage. Algorithm-agnostic design. |
| A Mechanism for Encoding Differences in Paired Certificates | Individual (v-07, Oct 2025) | <https://datatracker.ietf.org/doc/draft-bonnell-lamps-chameleon-certs/> | LAMPS | Efficiently encode differences between paired PQ/traditional certificates | Bandwidth-efficient hybrid migration. Competing with RFC 9763 and composite approaches. |
| Merkle Tree Certificates for TLS | **WG Adopted by PLANTS WG** (v-02, Mar 2026), Standards Track | <https://datatracker.ietf.org/doc/draft-ietf-plants-merkle-tree-certs/> | PLANTS (new WG) | Merkle tree proofs replace PQ signatures (<800 bytes vs ~17 KB) | **Most promising solution to PQ signature size crisis.** Chrome designated as "preferred option for PQ certs." Google+Cloudflare collaboration. Rollout: feasibility now, CT Logs Q1 2027, Chrome CQRS Q3 2027. Full ecosystem 10-15 years. |
| Internet X.509 Public Key Infrastructure -- Certificate Management Protocol (CMP) | **RFC 9810** (Jul 2025) | <https://datatracker.ietf.org/doc/rfc9810/> | LAMPS | CMP v3 with native KEM support | Obsoletes RFC 4210/9480. PKI enrollment infrastructure for PQC is ready. |
| A Hybrid Signature Method with Strong Non-Separability | Active | <https://datatracker.ietf.org/doc/draft-nir-lamps-altcompsigs/> | LAMPS | An alternative scheme of composing classic and post-quantum signature algorithms with different security properties. | |
| Hybrid signature spectrums | Active | <https://datatracker.ietf.org/doc/draft-hale-pquip-hybrid-signature-spectrums/> | PQUIP | Classification of design goals and security considerations for hybrid digital signature schemes | |
| Post-quantum cryptography use cases | Active | <https://datatracker.ietf.org/doc/draft-vaira-pquip-pqc-use-cases/> | PQUIP | PQ use cases and migration strategies | |
| TLS Key Share Prediction | WG Document (v-03, Aug 2025), Standards Track | <https://datatracker.ietf.org/doc/draft-ietf-tls-key-share-prediction/> | TLS | Server advertises keyshare preference in DNS to avoid HelloRetryRequest round-trips | Updates RFC 8446. Critical for PQ because PQ key shares are large and misprediction means sending large data twice. Codepoint assignment pending. |


# Algorithm names

The first round of "winners" of the [NIST PQC competition](https://csrc.nist.gov/Projects/post-quantum-cryptography) are Dilithium, Falcon, SPHINCS+, and Kyber.

**FIPS standards are now FINAL (published August 13, 2024):**
- **FIPS 203 -- ML-KEM** (Module-Lattice-Based Key Encapsulation Mechanism), derived from CRYSTALS-Kyber
- **FIPS 204 -- ML-DSA** (Module-Lattice-Based Digital Signature Algorithm), derived from CRYSTALS-Dilithium
- **FIPS 205 -- SLH-DSA** (Stateless Hash-Based Digital Signature Algorithm), derived from SPHINCS+
- **FIPS 206 -- FN-DSA** (FFT over NTRU-Lattice-Based Digital Signature Algorithm), derived from FALCON -- **draft pending NIST/Dept. of Commerce clearance**, expected late 2026 or early 2027

**Additional algorithms:**
- **HQC** (Hamming Quasi-Cyclic) -- selected March 2025 as backup KEM to ML-KEM. Code-based (not lattice), providing algorithmic diversity. Draft standard expected ~2027.
- **Additional Signature Schemes Round 2** -- 14 candidates advancing: CROSS, FAEST, HAWK, LESS, MAYO, Mirath, MQOM, PERK, QR-UOV, RYDE, SDitH, SNOVA, SQIsign, UOV. Round 3 down-select planned for 2026.

The IETF naming convention for algorithm versions:

* _**Dilithium_round1/2/3**_ -- means "as submitted to round 1, 2, or 3 of the NIST PQC competition". "Dilithium" is a short-hand for "Dilithium_round3".

* _**ML-DSA-ipd**_ -- FIPS 204 Initial Public Draft. For protocols that define a separate algorithm identifier per parameter set, it is encouraged to append the parameter ID, e.g., _**ML-DSA-ipd-44**_.

* _**ML-DSA**_ -- final FIPS 204 (now published).

The equivalent names are applied to Falcon, SPHINCS+, and Kyber.


# Implementations and interop testing for these specs

The following is an informal list of implementations of the RFCs and drafts above. This list also is for sites that have interoperability reports for implementations

- [IETF Hackathon - PQC Certificates](https://github.com/IETF-Hackathon/pqc-certificates)
- [IETF PQC Hackathon Interoperability Results](https://ietf-hackathon.github.io/pqc-certificates/pqc_hackathon_results_certs_r3.html)

This is an appropriate place to list interop events and hackathons that relate to PQC algorithms.

## Real-World Deployment (as of March 2026)

**Key Exchange (Production):**
- **Cloudflare:** ~60% of human TLS traffic uses hybrid ML-KEM. PQ automatic for all origins by June 2026. 6M+ domains upgraded by default.
- **Chrome:** X25519+ML-KEM enabled by default since August 2023. Transitioning from pre-standard Kyber to final ML-KEM.
- **Firefox:** Testing hybrid X25519+Kyber-768 with Cloudflare.
- **AWS:** ML-KEM PQ TLS in KMS, ACM, and Secrets Manager. Pre-standard Kyber support ends 2026.
- **OpenSSH:** sntrup761x25519 default for ~5 years.

**Signatures/Certificates (Not Yet Deployed):**
- No public PQ certificates in production as of early 2026.
- First PQ certificates expected 2026; broad browser trust unlikely before 2027.
- Merkle Tree Certificates: Cloudflare+Chrome feasibility study underway; Chrome CQRS targeted Q3 2027.


# Security Area protocols with no PQC-specific action needed

This table lists IETF Security Area protocols with "no action needed", typically because that protocol does not itself specify any cryptographic algorithms but instead embeds other IETF cryptographic protocols. Therefore no action is needed for that protocol because it will inherit PQC as soon as its cryptographic dependencies support it.

| Protocol | RFC       | Working Group  | Cryptographic dependencies | Comment |
|--------- |---------- |--------------- |--------------------------- |-------- |
| ACME     | <https://datatracker.ietf.org/doc/rfc8555/>   | ACME           | PKCS #10 (RFC 2986), JOSE/JWS (RFC 7515), TLS (RFC 8446) | |
| CMC      | <https://datatracker.ietf.org/doc/rfc5272/>   | LAMPS          | CMS (RFC 5652), PKCS #10 (RFC 2986) |         |
| QUIC     | <https://datatracker.ietf.org/doc/rfc9000/>   | QUIC           | TLS 1.3 (RFC 8446)          | QUIC is pretty specific to TLS 1.3, so probably needs an update in lockstep with each TLS update |
| DoH      | <https://datatracker.ietf.org/doc/rfc8484/>   | DPRIVE         | TLS (RFC 8446)              |         |
| EST      | <https://datatracker.ietf.org/doc/rfc7030/>   | LAMPS          | CMC (RFC 5272), CMS (RFC 5652), PKCS #10 (RFC 2986), TLS (RFC 8446) |         |
| HTTPS    | <https://datatracker.ietf.org/doc/rfc9110/>   | HTTPbis       | TLS (RFC 8446) | |
| SCEP     | <https://datatracker.ietf.org/doc/rfc8894/>   | LAMPS (?)      | CMS (RFC 5652), PKCS #10 (RFC 2986) |  |
| S/MIME   | <https://datatracker.ietf.org/doc/rfc5751/>   | LAMPS        | CMS (RFC 5652)               | Section 4.1 explicitly lists RSA, DSA, SHA-1. So maybe this needs an update to only modern non-PQC crypto. |

# Other Security protocols with no PQC-specific action needed
This table lists IETF Security protocols with "no action needed", typically because that protocol does not itself specify any cryptographic algorithms but instead embeds other IETF cryptographic protocols. Therefore no action is needed for that protocol because it will inherit PQC as soon as its cryptographic dependencies support it.

| Protocol | RFC       | Working Group  | Cryptographic dependencies | Comment |
|--------- |---------- |--------------- |--------------------------- |-------- |
| SRTP     | <https://datatracker.ietf.org/doc/rfc3711/> | AVT | DTLS (RFC 9147) | KEX handled by DTLS (RFC 5763/5764), Symmetric key AEAD (AES-GCM) algos available for actual SRTP (RFC 7714) |

# Suggestions for possible additions

The lists above can contain protocols and proposals from other standards organizations, such as IEEE, ETSI, and so on.
