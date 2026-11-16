# Security Policy & Threat Model Disclosure

## Scope

Hystrix is non-custodial client-side stealth payment infrastructure on Solana. Security-critical surfaces include:

1. **Dual-Key Stealth Derivation (`packages/sdk/src/ed25519.ts`, `stealth.ts`)** — subgroup validation (`8 * P != O`), scalar reduction modulo `L`, and view-tag domain separation.
2. **Chaumian Blind Veil Passes (`packages/sdk/src/veilPass.ts`)** — full-domain hash (`FDH`) bounds `[2, N - 2]`, coprime blinding factor generation, and epoch-scoped replay caps.
3. **Quill Drop Claim Vaults (`packages/sdk/src/drop.ts`, `programs/hystrix-veil`)** — URI `#fragment` secrecy, `MAX_RELAY_FEE_BPS = 35` on-chain ceiling, and post-expiry depositor reclaim checks.

## Key Invariants

- **Non-custodial spend authority:** Veil Relay nodes only co-sign as Solana `feePayer`. A relayer can never redirect a stealth sweep destination signed by `p_spend` or `claim_authority`, nor raise `relay_fee_bps` above the user-signed instruction data (`<= 35 bps`).
- **Torsion rejection:** `decodePoint` explicitly rejects small-order Edwards25519 torsion points (`8 * P == O`) before computing ECDH shared secrets `r * V` or `v * R`.
- **Zero-leak URI fragments:** Quill Drop bearer entropy resides exclusively after `#` in `https://hystrix.online/drop/...#<secret>`, ensuring browsers never transmit the secret in HTTP request lines or `Referer` headers.

## Reporting a Vulnerability

Do **not** open a public GitHub issue for exploitable cryptographic or Solana program bugs. Reach out via DM on [@hystrix_sol](https://x.com/hystrix_sol) to request an encrypted PGP/Signal disclosure channel.
