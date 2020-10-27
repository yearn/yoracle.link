# UniswapV2Oracle

UniswapV2Oracle is an on-chain oracle for UniswapV2 pairs. It allows any pair to be added in a permissionless fashion.

## UniswapV2Oracle

UniswapV2Oracles are sliding window oracles that uses observations collected over a window to provide moving price averages in the past `windowSize` with a precision of `windowSize / granularity`.

The `windowSize` was set to `4 hours`. Funding rates on major perpetual platforms are updated roughly every `8 hours`, for this reason `4 hours` was selected. The granularity is set to `4`, translating to roughly `1` reading every `hour`.

## Integrating

```
// returns the amount out corresponding to the amount in for a given token using the moving average over the time
function consult(address tokenIn, uint amountIn, address tokenOut) external view returns (uint amountOut)
```

Example;

```text
interface IUniswapV2Oracle {
  function consult(address tokenIn, uint amountIn, address tokenOut) external view returns (uint amountOut);
}

...

IUniswapV2Oracle public constant UniswapV2Oracle = IUniswapV2Oracle(0xa661F3644A59A5A5D768ee2765794F1cD084403d);

...

uint _ethOut = UniswapV2Oracle.consult(YFI, 1e18, ETH);

```

## Beta Deployment

UniswapV2Oracle [0x0e494045D79E2e2E9B30c303925142C60430B19A](https://etherscan.io/address/0x0e494045D79E2e2E9B30c303925142C60430B19A)  


## Pairs

Pair | Address
-- | --
ETH-WBTC | [0xBb2b8038a1640196FbE3e38816F3e67Cba72D940](https://etherscan.io/address/0xBb2b8038a1640196FbE3e38816F3e67Cba72D940)
ETH-USDC | [0xB4e16d0168e52d35CaCD2c6185b44281Ec28C9Dc](https://etherscan.io/address/0xB4e16d0168e52d35CaCD2c6185b44281Ec28C9Dc)
ETH-USDT | [0x0d4a11d5EEaaC28EC3F61d100daF4d40471f1852](https://etherscan.io/address/0x0d4a11d5EEaaC28EC3F61d100daF4d40471f1852)
ETH-DAI | [0xA478c2975Ab1Ea89e8196811F51A7B7Ade33eB11](https://etherscan.io/address/0xA478c2975Ab1Ea89e8196811F51A7B7Ade33eB11)
ETH-UNI | [0xd3d2E2692501A5c9Ca623199D38826e513033a17](https://etherscan.io/address/0xd3d2E2692501A5c9Ca623199D38826e513033a17)
YFI-ETH | [0x2fDbAdf3C4D5A8666Bc06645B8358ab803996E28](https://etherscan.io/address/0x2fDbAdf3C4D5A8666Bc06645B8358ab803996E28)
MKR-ETH | [0xC2aDdA861F89bBB333c90c492cB837741916A225](https://etherscan.io/address/0xC2aDdA861F89bBB333c90c492cB837741916A225)
AAVE-ETH | [0xDFC14d2Af169B0D36C4EFF567Ada9b2E0CAE044f](https://etherscan.io/address/0xDFC14d2Af169B0D36C4EFF567Ada9b2E0CAE044f)
COMP-ETH | [0xCFfDdeD873554F362Ac02f8Fb1f02E5ada10516f](https://etherscan.io/address/0xCFfDdeD873554F362Ac02f8Fb1f02E5ada10516f)

## Tokens

Symbol | Address
-- | --
WBTC |  [0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599](https://etherscan.io/address/0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599)
USDC | 	[0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48](https://etherscan.io/address/0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48)
USDT | 	[0xdAC17F958D2ee523a2206206994597C13D831ec7](https://etherscan.io/address/0xdAC17F958D2ee523a2206206994597C13D831ec7)
DAI |  [0x6B175474E89094C44Da98b954EedeAC495271d0F](https://etherscan.io/address/0x6B175474E89094C44Da98b954EedeAC495271d0F)
UNI |  [0x1f9840a85d5aF5bf1D1762F925BDADdC4201F984](https://etherscan.io/address/0x1f9840a85d5aF5bf1D1762F925BDADdC4201F984)
YFI |  [0x0bc529c00C6401aEF6D220BE8C6Ea1667F6Ad93e](https://etherscan.io/address/0x0bc529c00C6401aEF6D220BE8C6Ea1667F6Ad93e)
MKR |  [0x9f8F72aA9304c8B593d555F12eF6589cC3A579A2](https://etherscan.io/address/0x9f8F72aA9304c8B593d555F12eF6589cC3A579A2)
AAVE |  [0x7Fc66500c84A76Ad7e9c93437bFc5Ac33E2DDaE9](https://etherscan.io/address/0x7Fc66500c84A76Ad7e9c93437bFc5Ac33E2DDaE9)
COMP |  [0xc00e94Cb662C3520282E6f5717214004A7f26888](https://etherscan.io/address/0xc00e94Cb662C3520282E6f5717214004A7f26888)
