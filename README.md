# Keep3rV1Oracle

Keep3rV1Oracle is an on-chain oracle for UniswapV2 pairs.

## Keep3rV1Oracle

Keep3rV1Oracles are sliding window oracles that use observations collected over a window to provide moving price averages in the past `windowSize` with a precision of `windowSize / granularity`.

The `windowSize` is based on the `granularity` supplied by the user. There is a reading every `periodSize` minutes.

## Price Feeds

### Data Freshness

Contract: Keep3rV1Oracle

```
// returns the amount out corresponding to the amount in for a given token using the moving average over the time
function current(address tokenIn, uint amountIn, address tokenOut) external view returns (uint amountOut)
```

Example;

```
interface IUniswapV2Oracle {
  function current(address tokenIn, uint amountIn, address tokenOut) external view returns (uint amountOut);
}

```

### Security

Contract: Keep3rV1Oracle

A quote allows the caller to specify the `granularity` or amount of `points` to take. Each `point` is `periodSize`, so 24 `points` would be `24 * periodSize` `windowSize`

Data freshness is decreased for increased security

```
// returns the amount out corresponding to the amount in for a given token using the moving average over the time taking granularity samples
function quote(address tokenIn, uint amountIn, address tokenOut, uint granularity) external view returns (uint amountOut)
```

### Price points

Returns a set of price `points` equal to `points * periodSize`, so for the points in the last 24 hours use `points = 48`

```
// returns an amount of price points equal to periodSize * points
prices(address tokenIn, uint amountIn, address tokenOut, uint points) external view returns (uint[] memory)
```

### Hourly

Returns a set of price `points` equal to `points * hours`, so for the points in the last 24 hours use `points = 24`

```
// returns an amount of price points equal to hour * points
function hourly(address tokenIn, uint amountIn, address tokenOut, uint points) external view returns (uint[] memory)
```

### Daily

Returns a set of price `points` equal to `points * days`, so for the points in the last 24 hours use `points = 1`

```
// returns an amount of price points equal to days * points
function daily(address tokenIn, uint amountIn, address tokenOut, uint points) external view returns (uint[] memory)
```

### Weekly

Returns a set of price `points` equal to `points * weeks`, so for the points in the last week use `points = 1`

```
// returns an amount of price points equal to days * points
function weekly(address tokenIn, uint amountIn, address tokenOut, uint points) external view returns (uint[] memory)
```

## Volatility

Contract: Keep3rV1Volatility

### Realized Volatility

Returns the realized volatility over the last amount of `points` per `window * periodSize`, so volatility over last hour would be `points = 1 & window = 2`

```
// returns realized volatility over points * window
function rVol(address tokenIn, address tokenOut, uint points, uint window) external view returns (uint)
```

### Hourly Realized Volatility

Returns the realized volatility over the last amount of `points` per `hour`, so volatility over last hour would be `points = 1`

```
// returns realized volatility over points * window
function rVolHourly(address tokenIn, address tokenOut, uint points) external view returns (uint);
```

### Daily Realized Volatility

Returns the realized volatility over the last amount of `points` per `days`, so volatility over last day would be `points = 1`

```
// returns realized volatility over points * window
function rVolDaily(address tokenIn, address tokenOut, uint points) external view returns (uint);
```

### Weekly Realized Volatility

Returns the realized volatility over the last amount of `points` per `weeks`, so volatility over last day week be `points = 1`

```
// returns realized volatility over points * window
function rVolWeekly(address tokenIn, address tokenOut, uint points) external view returns (uint);
```

## Pricing

### Black Scholes Estimate

Contract: Keep3rV1Oracle

```
/**
 * @dev blackScholesEstimate calculates a rough price estimate for an ATM option
 * @dev input parameters should be transformed prior to being passed to the function
 * @dev so as to remove decimal places otherwise results will be far less accurate
 * @param _vol uint256 volatility of the underlying converted to remove decimals
 * @param _underlying uint256 price of the underlying asset
 * @param _time uint256 days to expiration in years multiplied to remove decimals
 */
function blackScholesEstimate(
    uint256 _vol,
    uint256 _underlying,
    uint256 _time
) external pure returns (uint256 estimate);
```

### Options Pricing

Contract: Keep3rV1Volatility

```
function C(uint t, uint v, uint sp, uint st) external pure returns (uint);
function quoteAll(uint t, uint v, uint sp, uint st) external pure returns (uint call, uint put);
function quotePrice(address tokenIn, address tokenOut, uint t, uint sp, uint st) external view returns (uint call, uint put);
function price(address tokenIn, address tokenOut) external view returns (uint);
function quote(address tokenIn, address tokenOut, uint t) external view returns (uint call, uint put);
```

## Math libs

```
function optimalExp(uint256 x) external pure returns (uint256);
function ln(uint256 x) external pure returns (uint);
function ncdf(uint x) external pure returns (uint);
function cdf(int x) external pure returns (uint);
function generalLog(uint256 x) external pure returns (uint);

```

## Beta Deployment

UniswapKeep3rV1Oracle [0x73353801921417F465377c8d898c6f4C0270282C](https://etherscan.io/address/0x73353801921417F465377c8d898c6f4C0270282C)  
UniswapKeep3rV1Volatility [0xCCdfCB72753CfD55C5afF5d98eA5f9C43be9659d](https://etherscan.io/address/0xCCdfCB72753CfD55C5afF5d98eA5f9C43be9659d)  
SushiswapKeep3rV1Oracle [0xf67Ab1c914deE06Ba0F264031885Ea7B276a7cDa](https://etherscan.io/address/0xf67Ab1c914deE06Ba0F264031885Ea7B276a7cDa)  
SushiswapKeep3rV1Volatility [0x173ed6531818456f29Fc74011a3B1FB4B6132Dc9](https://etherscan.io/address/0x173ed6531818456f29Fc74011a3B1FB4B6132Dc9)  

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
KP3R |  [0x1ceb5cb57c4d4e2b2433641b95dd330a33185a44](https://etherscan.io/address/0x1ceb5cb57c4d4e2b2433641b95dd330a33185a44)
SNX |  [0xc011a73ee8576fb46f5e1c5751ca3b9fe0af2a6f](https://etherscan.io/address/0xc011a73ee8576fb46f5e1c5751ca3b9fe0af2a6f)
LINK |  [0x514910771af9ca656af840dff83e8264ecf986ca](https://etherscan.io/address/0x514910771af9ca656af840dff83e8264ecf986ca)
CRV |  [0xd533a949740bb3306d119cc777fa900ba034cd52](https://etherscan.io/address/0xd533a949740bb3306d119cc777fa900ba034cd52)
UMA |  [0x04fa0d235c4abf4bcf4787af4cf447de572ef828](https://etherscan.io/address/0x04fa0d235c4abf4bcf4787af4cf447de572ef828)
renBTC |  [0xeb4c2781e4eba804ce9a9803c67d0893436bb27d](https://etherscan.io/address/0xeb4c2781e4eba804ce9a9803c67d0893436bb27d)
HEGIC |  [0x584bc13c7d411c00c01a62e8019472de68768430](https://etherscan.io/address/0x584bc13c7d411c00c01a62e8019472de68768430)
COL |  [0xc76fb75950536d98fa62ea968e1d6b45ffea2a55](https://etherscan.io/address/0xc76fb75950536d98fa62ea968e1d6b45ffea2a55)
STAKE |  [0x0ae055097c6d159879521c384f1d2123d1f195e6](https://etherscan.io/address/0x0ae055097c6d159879521c384f1d2123d1f195e6)
