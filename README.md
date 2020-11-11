# UniswapV2SlidingOracle

UniswapV2SlidingOracle is an on-chain oracle for UniswapV2 pairs.

## UniswapV2SlidingOracle

UniswapV2SlidingOracles are sliding window oracles that uses observations collected over a window to provide moving price averages in the past `windowSize` with a precision of `windowSize / granularity`.

The `windowSize` is based on the `granularity` supplied by the user. There is a reading every `periodSize` minutes.

## Price Feeds

### Data Freshness

```
// returns the amount out corresponding to the amount in for a given token using the moving average over the time
function current(address tokenIn, uint amountIn, address tokenOut) external view returns (uint amountOut)
```

Example;

```text
interface IUniswapV2Oracle {
  function current(address tokenIn, uint amountIn, address tokenOut) external view returns (uint amountOut);
}

...

IUniswapV2Oracle public constant UniswapV2Oracle = IUniswapV2Oracle(0xCA2E2df6A7a7Cf5bd19D112E8568910a6C2D3885);

...

uint _ethOut = UniswapV2Oracle.current(WETH, 1e18, YFI);

```

### Security

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


### Realized Volatility

Returns the realized volatility over the last amount of `points` per `window * periodSize`, so volatility over last hour would be `points = 1 & window = 2`

```
// returns realized volatility over points * window
function realizedVolatility(address tokenIn, uint amountIn, address tokenOut, uint points, uint window) external view returns (uint)
```

### Hourly Realized Volatility

Returns the realized volatility over the last amount of `points` per `hour`, so volatility over last hour would be `points = 1`

```
// returns realized volatility over points * window
function realizedVolatilityHourly(address tokenIn, uint amountIn, address tokenOut, uint points) external view returns (uint[] memory)
```

### Daily Realized Volatility

Returns the realized volatility over the last amount of `points` per `days`, so volatility over last day would be `points = 1`

```
// returns realized volatility over points * window
function realizedVolatilityDaily(address tokenIn, uint amountIn, address tokenOut, uint points) external view returns (uint[] memory)
```

### Weekly Realized Volatility

Returns the realized volatility over the last amount of `points` per `weeks`, so volatility over last day week be `points = 1`

```
// returns realized volatility over points * window
function realizedVolatilityWeekly(address tokenIn, uint amountIn, address tokenOut, uint points) external view returns (uint[] memory)
```

## Pricing

### Black Scholes Estimate

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
) public pure returns (uint256 estimate) {
    estimate = 40 * _vol * _underlying * sqrt(_time);
    return estimate;
}
```

## Beta Deployment

UniswapV2Oracle [0xCA2E2df6A7a7Cf5bd19D112E8568910a6C2D3885](https://etherscan.io/address/0xCA2E2df6A7a7Cf5bd19D112E8568910a6C2D3885)  
Keep3rV1Oracle [0x73353801921417F465377c8d898c6f4C0270282C](https://etherscan.io/address/0x73353801921417F465377c8d898c6f4C0270282C)  

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
KP3R |  [0x1ceb5cb57c4d4e2b2433641b95dd330a33185a44](https://etherscan.io/address/0x1ceb5cb57c4d4e2b2433641b95dd330a33185a44)
SNX |  [0xc011a73ee8576fb46f5e1c5751ca3b9fe0af2a6f](https://etherscan.io/address/0xc011a73ee8576fb46f5e1c5751ca3b9fe0af2a6f)
LINK |  [0x514910771af9ca656af840dff83e8264ecf986ca](https://etherscan.io/address/0x514910771af9ca656af840dff83e8264ecf986ca)
CRV |  [0xd533a949740bb3306d119cc777fa900ba034cd52](https://etherscan.io/address/0xd533a949740bb3306d119cc777fa900ba034cd52)
