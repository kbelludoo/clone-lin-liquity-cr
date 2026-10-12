# clone-lin-liquity-cr

Experimental i64-scale LIN clone of [liquity/dev](https://github.com/liquity/dev) `packages/contracts/contracts/Dependencies/LiquityMath.sol` (`_computeCR` / `decMul` / `_decPow`).

- **LIN clone:** [`src/lin_liquity_cr.lin`](src/lin_liquity_cr.lin)
- **Proofs (results only) live in lin-open:** https://github.com/kbelludoo/lin-open
- **Event receipt:** `docs/events/EVENT_LIQUITY_CR_CLONE_LIN.rulel` in lin-open

This repository is the standalone LIN copy. It is **not** Liquity, **not** a TroveManager, and does **not** claim Solidity uint256 / mainnet LUSD / PriceFeed parity.

Upstream LiquityMath.sol is MIT. This LIN kernel is an original uint64-scale implementation of the published ICR / half-up decimal mul / exponentiation-by-squaring, not a verbatim Solidity dump.

Canonical goldens (uint64-scale, 128-bit product):

- `_computeCR(10, 5, 3)` = **6**
- `_computeCR(1000, 0, 2000)` = **0** (Solidity `2**256-1` infinite CR; LIN fail-closes)
- same 10000/10000 coll/debt: Liquity CR **1e18** (price=1e18) vs Aave-style HF **0.85e18** (lt=85%)
- floor wad-mul `3*(5e17)/1e18` = **1** vs Liquity `decMul` half-up **2**
- `_decPow(2e18, 3)` = **8e18**
- `_decPow(MINUTE_DECAY_FACTOR, 2)` = **998076443575627963**
- `1e18 * 1e18 / 1` fail-closes to **0**
