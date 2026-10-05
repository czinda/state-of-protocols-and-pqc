#+#+#+#+#+#+#+#+#+#+#+#+#+#+#+#+#+#+#+#+
# PQC Status Report — October 5, 2026

> **Change summary** covering developments from **August 21, 2026** to **October 5, 2026**.
> For the full protocol tracker, see [README.md](README.md).

---

## Executive Summary

In the last ~6 weeks, several previously “about to land” PQC documents moved into the **RFC Editor Queue**, and the tracker scope expanded to cover **RPKI**, **NTS**, and **DKIM2** as PQC-adjacent protocol surfaces with distinctive scaling/operational constraints.

### Key status moves (Aug → Oct 2026)

| Area | Aug 21 snapshot | Current (Oct 5) | Why it matters |
|------|------------------|-----------------|----------------|
| TLS KEX (standalone ML‑KEM) | IESG telechat pending | **RFC Editor Queue** ([draft-ietf-tls-mlkem](https://datatracker.ietf.org/doc/draft-ietf-tls-mlkem/)) | “Pure” PQ key establishment profile is now in publication pipeline (Informational). |
| TLS auth (ML‑DSA) | IESG Approved | **RFC Editor Queue** ([draft-ietf-tls-mldsa](https://datatracker.ietf.org/doc/draft-ietf-tls-mldsa/)) | TLS signature negotiation spec is now in publication pipeline (Informational). |
| IPsec auth (PQC signatures) | IESG Evaluation | **RFC Editor Queue** ([draft-ietf-ipsecme-ikev2-pqc-auth](https://datatracker.ietf.org/doc/draft-ietf-ipsecme-ikev2-pqc-auth/)) | Clears a major “PQC auth” blocker for IKEv2 deployments. |
| SSH ML‑KEM hybrid KEX | RFC Editor processing | **Published as RFC 10042** ([RFC 10042](https://datatracker.ietf.org/doc/rfc10042/)) | SSH now has a published ML‑KEM hybrid KEX spec. |
| PKI composite KEM | IESG telechat pending | **IESG Evaluation → Revised I‑D Needed** ([draft-ietf-lamps-pq-composite-kem](https://datatracker.ietf.org/doc/draft-ietf-lamps-pq-composite-kem/)) | Composite KEM is still a key PKI transition dependency; DISCUSS resolution is the near-term gating item. |

---

## New / Expanded Protocol Coverage

These were added to the tracker because they are PQC-relevant protocol ecosystems with unique constraints:

1) **Routing security (RPKI / BGPsec)**
   - PQC is challenging due to **bulk validation** and repository distribution scaling.
   - Active inputs:
     - [draft-yoshikawa-sidrops-pqc-rpki](https://datatracker.ietf.org/doc/draft-yoshikawa-sidrops-pqc-rpki/) (experiments + migration considerations)
     - [draft-li-sidrops-pqrpki](https://datatracker.ietf.org/doc/draft-li-sidrops-pqrpki/) (pqRPKI ladder object; additive PQ authentication)

2) **Time synchronization (NTP / NTS)**
   - NTS inherits PQ KEX via TLS 1.3, but **pooling/deployment** and **PTP key management** are active work:
     - [RFC 8915](https://www.rfc-editor.org/rfc/rfc8915) (NTS for NTP)
     - [draft-ietf-ntp-nts-keyexchange-pool](https://datatracker.ietf.org/doc/draft-ietf-ntp-nts-keyexchange-pool/) (NTS pools)
     - [draft-ietf-ntp-nts-for-ptp](https://datatracker.ietf.org/doc/draft-ietf-ntp-nts-for-ptp/) (NTS4PTP)

3) **Email authentication (DKIM2)**
   - Email’s operational scale and DNS transport constraints make PQ signatures especially painful; algorithm agility / protocol evolution is worth tracking:
     - [draft-ietf-dkim-dkim2-spec](https://datatracker.ietf.org/doc/draft-ietf-dkim-dkim2-spec/)
     - [draft-ietf-dkim-dkim2-motivation](https://datatracker.ietf.org/doc/draft-ietf-dkim-dkim2-motivation/) (expired but informative)

4) **Web authentication (WebAuthn / FIDO2)**
   - Not IETF standards, but major deployment surface for PQ signatures.
   - See SAAG minutes noting a talk on PQ web authentication transition strategies: [minutes-126-saag](https://datatracker.ietf.org/doc/minutes-126-saag/).

---

## What’s Next (late 2026)

| Date / window | Item | Notes |
|---|---|---|
| Late Oct 2026 | OMB M‑26‑15 migration plans due | Key driver for enterprise “what must interop” requirements. |
| Nov 14–20, 2026 | IETF 127 | Likely venue for resolving stuck items (e.g., composite KEM DISCUSS) and reviving expired MLS combiner. |
| Dec 2026 | MLS combiner milestone | Tracker shows combiner draft expired; expect a re-spin to meet milestone. |
| Late 2026 → 2027 | MTC / PQ certificates ecosystem (PLANTS) | Let’s Encrypt staging environment expected late 2026; broader rollout in 2027 (MTC is a PLANTS WG deliverable and the most plausible path to browser-scale PQ authentication). |
