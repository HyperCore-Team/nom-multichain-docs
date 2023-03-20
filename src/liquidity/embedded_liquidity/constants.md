# Liquidity Embedded Constants

Liquidity constants are hard coded into the `embedded.go` file as network constants.

- `LiquidityZnnTotalPercentages` represent the maximum sum for the `ZNN` percentage rewards of each allowed `ZTS`

- `LiquidityQsrTotalPercentages` represent the maximum sum for the `QSR` percentage rewards of each allowed `ZTS`

- `LiquidityStakeWeights` represent the liquidity staking weights depending on the number of months a user locks the liquidity

```go
    LiquidityZnnTotalPercentages         uint32 = 10000
    LiquidityQsrTotalPercentages         uint32 = 10000
    LiquidityStakeWeights                       = []int64{0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12,
    }
```
