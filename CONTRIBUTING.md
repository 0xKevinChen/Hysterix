# Contributing to Hystrix

Contributions across `@hystrix/sdk`, `@hystrix/relay`, `@hystrix/cli`, and `programs/hystrix-veil` are welcome.

## Development Prerequisites

- **Node.js `>= 22.6`** (native TypeScript execution — zero build step required for SDK & tests)
- **Rust `>= 1.79` & Solana CLI `>= 2.1` / Anchor `0.30.1`** (only required when compiling `programs/hystrix-veil`)

## Running the Test Suite

```bash
# Run all Curve25519 ECDH, Blind Veil Pass, Quill Drop & Split Sweep tests
npm test

# Run interactive examples
npm run example:stealth
npm run example:pass
npm run example:sweep

# Inspect CLI commands
npm run cli -- tiers
```

## Coding Guidelines

1. **Zero external runtime dependencies in `@hystrix/sdk`:** Core stealth derivation and blind-signature verification must stay auditable in pure TypeScript + WebCrypto/Node `crypto` primitives without pulling transitive npm supply-chain trees.
2. **Domain separation:** Every hash invocation must include an explicit versioned ASCII tag from `DOMAIN_TAGS` (`packages/sdk/src/constants.ts`).
3. **Strict atomic units:** Never use IEEE-754 floats when splitting lamports or SPL micro-units; all fee and leg calculations in `sweep.ts` and `lib.rs` must operate on `bigint` / `u64` / `u128`.
