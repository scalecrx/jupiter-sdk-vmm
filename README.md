# jupiter-sdk-vmm

Scale VMM adapter crate for Jupiter's `jupiter-amm-interface`.

## What This Implements

- `ScaleVmm` implementing `jupiter_amm_interface::Amm`.
- Offline quote logic matching Scale VMM on-chain math for:
  - Constant product
  - Exponential
  - Buy path (`mint_a -> mint_b`) with fee on input `mint_a`
  - Sell path (`mint_b -> mint_a`) with fee on output `mint_a`
- `ExactIn` support and explicit `ExactOut` rejection.
- Dynamic account metas including beneficiary token accounts.
- Full VMM swap account contract including mandatory AMM graduation accounts:
  - VMM config PDA: `["config"]`
  - VMM vault PDAs: `[pair, mint]`
  - AMM pool PDA: `["pool", pair, mint_a, mint_b]`
  - AMM vault PDAs: `[amm_pool, mint]`
  - AMM config PDA: `["config"]`
  - Fee token ATA for platform + beneficiaries on `mint_a`

## Swap Leg Selection

By default, this adapter emits `Swap::TokenSwap`.

You can override the Jupiter swap leg per market using `KeyedAccount.params`:

```json
{
  "swap": "gamma"
}
```

Graduation remains fixed to the Scale AMM program
`SCALEwAvEK5gtkdHiFzXfPgtk2YwJxPDzaV3aDmR7tA`. The adapter rejects any
non-Scale `amm_program_id` override.

Supported `swap` overrides:

- `token_swap`
- `gamma`
- `meteora_damm_v2`
- `obsidian`
- `raydium_v2`

## Local Validation

```bash
cargo test
```
