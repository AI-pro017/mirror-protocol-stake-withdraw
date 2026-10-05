# Mirror Protocol Stake and Withdraw

A Node.js script for providing liquidity to Mirror Protocol pools on Terra and staking the LP tokens, written for the Maui pool.

Mirror Protocol had "mAssets", tokens that tracked the price of real stocks and ETFs like mAAPL, mTSLA and mSPY, each with a liquidity pool against UST. This script automates the steps a liquidity provider would otherwise click through in the Mirror app.

## What it does

`autoStake(mnemonic, network, qty, mAsset, slippage)`

1. Reads the pool to get the current mAsset/UST price.
2. Approves the Mirror staking contract to spend `qty` of the mAsset.
3. Calls `auto_stake`, which adds the mAsset plus the matching amount of UST to the pool and stakes the LP tokens in one go, within the given slippage percentage.

`withdraw(mnemonic, network, qty, mAsset)`

1. Unbonds `qty` LP tokens from the staking contract.
2. Sends them back to the pool with `withdraw_liquidity` to get the mAsset and UST out.

`queryPool(network, mAsset, address)` prints how many LP tokens an address has staked for that mAsset.

All contract addresses for mainnet (`columbus-5`) and the Bombay testnet are in `config.js`, covering MIR and every mAsset pool.

## Running it

```bash
git clone https://github.com/AI-pro017/mirror-protocol-stake-withdraw.git
cd mirror-protocol-stake-withdraw
npm install
```

Pick the call you want at the bottom of `index.js` (function, network, amount and mAsset), then pass your wallet mnemonic through the `MNEMONIC` environment variable:

```bash
MNEMONIC="word1 word2 ... word24" node index.js
```

The mnemonic is never written into the code, and `.env` files are ignored by git.

## Status

This was written for the original Terra chain (`columbus-5`), now known as Terra Classic. Since the Terra collapse in May 2022, the Bombay testnet has been shut down and the `terra.dev` LCD endpoints are offline, so running it today would need new endpoints and contract addresses. It's mostly useful as a reference for working with Terra smart contracts using terra.js.

## Built with

- `@terra-money/terra.js` 2.0 for signing and broadcasting
- `base-64` for encoding the CW20 `send` message
