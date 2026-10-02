# FundMe

[![CI](https://github.com/agent-mino/foundry-fund-me/actions/workflows/test.yml/badge.svg)](https://github.com/agent-mino/foundry-fund-me/actions/workflows/test.yml)

A crowdfunding smart contract that enforces a minimum USD contribution using **Chainlink price feeds**. Contributions are tracked per address; only the owner can withdraw. Demonstrates Chainlink oracle integration, Solidity library patterns, gas optimization, and multi-network deployment with Foundry.

## How it works

- `fund()` — accepts ETH if the contribution is worth ≥ $5 USD at the current ETH/USD rate from Chainlink; otherwise reverts
- `withdraw()` — owner-only; clears all balances and transfers the contract's ETH to the owner
- `cheaperWithdraw()` — same result, but caches `s_funders.length` in a local variable to avoid repeated storage reads on large funder lists
- `fallback()` / `receive()` — route direct ETH transfers through `fund()` automatically

The USD conversion is implemented as a `PriceConverter` library attached to `uint256`, keeping the funding logic readable: `msg.value.getConversionRate(priceFeed) >= MINIMUM_USD`.

## Gas optimization

`withdraw` reads `s_funders.length` from storage on every loop iteration. `cheaperWithdraw` reads it once into memory, which reduces gas cost proportionally with the number of funders.

## Tech stack

Solidity 0.8.18 · Chainlink price feeds · Foundry (Forge + Anvil)

## Run locally

Requires [Foundry](https://book.getfoundry.sh/getting-started/installation).

```bash
git clone --recurse-submodules https://github.com/agent-mino/foundry-fund-me.git
cd foundry-fund-me
forge build
forge test
```

## Deploy

The deploy script uses `HelperConfig` to select the correct Chainlink price feed address for the target network, and deploys a mock aggregator locally so tests run without a live RPC.

```bash
# Local Anvil node
anvil &
forge script script/DeployFundMe.s.sol --broadcast --rpc-url http://127.0.0.1:8545 --private-key <ANVIL_KEY>

# Sepolia testnet (set PRIVATE_KEY and SEPOLIA_RPC_URL in .env)
forge script script/DeployFundMe.s.sol --broadcast --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY
```

## CI

Every push runs `forge fmt --check`, `forge build --sizes`, and `forge test -vvv`.
