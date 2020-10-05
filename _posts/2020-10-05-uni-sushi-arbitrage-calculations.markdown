---
layout: post
title:  "Uniswap & Sushiswap arbitrage calculations"
date:   2020-10-05 20:46:25 +0800
categories: crypto
---
For input and output tokens

```
oa - output amount
ia - input amount
or - output reserves
ir - input reserves
k = 0.997
```

According to Uniswap formula for output amount [getAmountOut](https://github.com/Uniswap/uniswap-v2-periphery/blob/master/contracts/libraries/UniswapV2Library.sol#L43)

```
        997 * ia * or       k * ia * or
oa = -------------------- = ------------  (1)
     1000 * ir + 997 * ia   ir + k * ia
```

If we solve eq. (1) with respect to `ia`

```
        ir * oa
ia = -------------  (2)
     k * (or - oa)
```

Uniswap algorithm adds 1 wei to right side of the (2) eq.

Now if we assume of an operation of purchase of token for ETH at exchange #1
and selling it at exchange #2 for ETH and introduce this notations

```
x1 - amount out at exchange #1
x2 - amount out at exchange #2
y1 - reservesIn at exchange #1
y2 - reservesIn at exchange #2
z1 - reservesOut at exchange #1
z2 - reservesOut at exchange #2
k  - fee coefficient (k = 0.997)
```

Then output amount of token at exchange #1 would be equal to input amount of
token at exchange #2

```
        y2 * x2
x1 = -------------  (3)
     k * (z2 - x2)
```

If input amount at exchange #1 <= output amount at exchange #2 =>

```
         y1 * x1
x2 >= -------------  (4)
      k * (z1 - x1)
```

Substituting `x2` from (3)

```
     k * x1 * z2
x2 = -----------  (5)
     k * x1 + y2
```

```
k * x1 * z2       y1 * x1
----------- >= -------------  (6)
k * x1 + y2    k * (z1 - x1)
```
