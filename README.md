# PoC — Ploutos Money Oracle Misconfiguration (Feb 2026)

> **Educational / research use only.** This repository reproduces the oracle
> misconfiguration exploit that drained ~$388K from Ploutos Money lending pools.

## Quick Start

```bash
git clone https://github.com/NomosLabs-Security/poc-ploutos-money-2026
cd poc-ploutos-money-2026
forge install foundry-rs/forge-std --no-git
forge test -vvvv
```

## Details

- **Date:** 2026-02-26
- **Chain:** Ethereum (primary), Hemi, Arbitrum, Hyperliquid, Avalanche
- **Config TX:** [0xa17dc37e...705f8474](https://etherscan.io/tx/0xa17dc37e1b65c65d20042212fb834974f7faaa961442e3fc05393778705f8474)
- **Loss:** 187.36 ETH (~$388K)
- **Expected Output:** Oracle misconfiguration allows borrowing 187 ETH with 8 USDC collateral

## Vulnerability

Ploutos Money's lending pool used Chainlink price feeds for collateral valuation.
The USDC price feed was misconfigured to point at the **BTC/USD** aggregator instead
of USDC/USD. This valued 1 USDC at ~$97,000 (BTC price), allowing the attacker to:

1. Deposit 8 USDC (valued at ~$776K by the broken oracle)
2. Borrow 187.36 ETH (~$388K) — within the LTV ratio
3. Walk away with 187 ETH; collateral worth $8 in reality

The exploit occurred one block after the misconfiguration was confirmed on-chain.

- **Full Analysis:** [NomosLabs Security Intelligence Archive](https://nomoslabs.io/archive/ploutos-money-2026)

MIT — NomosLabs Security Research
