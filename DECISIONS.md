# Decisions

## D1. Name

**Tare.** The weights-and-measures term for the weight of the container, subtracted from the gross reading to get the net weight.

Collision search (2026-10-10): a web search of "Tare" against crypto, DeFi and tokenized-stock products returned no product of that name. Backup name if a collision appears: **Cupel**.

## D2. Share denomination on-chain, not off-chain

Limits are signed in shares and the contract reads the unit in the same transaction. An off-chain guard protects only while an app is open and an owner is awake to sign.

## D3. Allowance model, no custody

The owner approves the book for the exact amount of one order. The book pulls at fill, executes an allowlisted route, verifies in shares, pays out, and resets the approval. Nothing is held between transactions.

## D4. Unit mapping for xStocks

xStocks balances are rebasing and denominated in share units, so the registry reports a unit of one for them and records the issuer multiplier only as information. Reason: the issuer documents `balanceOf` as the equity-adjusted balance. Open item in `LIMITATIONS.md`.

## D5. Aggregator router allowlisted although unverified

The Trading API returns executable calldata with the book as taker through one router. Allowlisting it lets Ondo and bStocks fill through deeper liquidity. Risk accepted because the approval is exact and reset after the call and the share floor reverts any fill that delivers too little. Reversible by the registry owner.

## D6. Wrapper catalogue

Core tickers keep full evidence (addresses, selectors, pools, transfer simulation). Other tickers are included only when the issuer unit source answers on-chain and a direct USDT pool holds real depth, so every ticker on the Shelf has a wrapper a contract can fill.

## D7. EVM target

The book is compiled for Cancun. BNB Smart Chain supports it, and the issuer token implementations use `PUSH0`.

## D8. Model role

A model, when configured, writes wording only. Removing the key changes sentences, never a decision. The decision path has no network call to a model.

## D9. Evidence rule

Numbers in README, landing and proof pages are read from `data/**/summary.json` at build time. `scripts/check_claims.ts` fails CI on a number that is in no summary file.


## D10. Multi-chain expansion and brand: Parallax

**Parallax.** The apparent shift of an object's position when viewed along two different lines of sight. In tokenized stock markets, one share looks different depending on the wrapper lens you view it through. Parallax calculates the true position.

## D11. Stellar Soroban target

The protocol is ported to Stellar's Soroban WebAssembly (WASM) smart contract platform (Protocol 20+). Soroban provides Turing-complete execution, atomic cross-contract calls, and instantaneous panics/reverts when the `ShareFloor` is violated. Open orders are stored in Persistent Storage with automatic TTL extensions.

## D12. Stellar USDC decimal scaling

On Stellar, native Circle USDC uses 7 decimals (stroops). The protocol normalizes USDC values to 18-decimal fixed point (WAD) internally when computing per-share pricing to maintain high fractional equity precision.
