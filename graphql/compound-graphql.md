# Compound Finance GraphQL API

## Description

Compound Finance is a decentralized, blockchain-based DeFi lending protocol built on Ethereum. It allows users to supply assets to earn interest or borrow assets against supplied collateral. The protocol operates via cToken smart contracts — each supported asset has a corresponding cToken (e.g., cDAI, cETH, cUSDC) that represents a user's position in the market.

This GraphQL API is exposed via The Graph protocol's hosted subgraph for Compound v2. It indexes on-chain events and state, making it queryable without running a full Ethereum node.

## Endpoint

```
https://api.thegraph.com/subgraphs/name/graphprotocol/compound-v2
```

## Documentation

- Compound Finance Docs: https://compound.finance/docs
- The Graph Hosted Service: https://thegraph.com/hosted-service/subgraph/graphprotocol/compound-v2
- Subgraph Source (GitHub): https://github.com/graphprotocol/compound-v2-subgraph

## Key Types

| Type | Kind | Description |
|------|------|-------------|
| Comptroller | Entity | Protocol-level configuration (price oracle, liquidation incentive, close factor) |
| Market | Entity | Each cToken market with supply/borrow rates, reserves, exchange rate, and pricing |
| Account | Entity | An Ethereum address that has interacted with Compound markets |
| AccountCToken | Entity | A single account's position within a single cToken market |
| AccountCTokenTransaction | Entity | Auxiliary transaction records for AccountCToken |
| TransferEvent | Entity | Any cToken transfer between addresses |
| MintEvent | Entity | cToken mint (supply underlying to receive cTokens) |
| RedeemEvent | Entity | cToken redeem (burn cTokens to receive underlying) |
| LiquidationEvent | Entity | Liquidation of an undercollateralized position |
| BorrowEvent | Entity | Borrow of underlying asset from a market |
| RepayEvent | Entity | Repayment of a borrow (payer may differ from borrower) |
| CTokenTransfer | Interface | Shared interface for TransferEvent, MintEvent, RedeemEvent, LiquidationEvent |
| UnderlyingTransfer | Interface | Shared interface for BorrowEvent and RepayEvent |

## Example Query

```graphql
{
  markets(first: 5) {
    id
    name
    symbol
    borrowRate
    supplyRate
    totalBorrows
    totalSupply
    underlyingName
    underlyingSymbol
    underlyingPriceUSD
  }
}
```

## Notes

- The subgraph is maintained by the Graph Protocol team and indexes Compound v2 (not v3/Comet).
- Market exchange rates and borrow indexes are snapshotted at the last accrual block; real-time balances require recalculation using the block delta.
- BigDecimal fields are returned as strings in JSON responses.
- The Graph hosted service may be deprecated; migrate to The Graph's decentralized network for production use.
- For Compound v3 (Comet architecture), see the official Compound v3 subgraph or Goldsky mirror.
