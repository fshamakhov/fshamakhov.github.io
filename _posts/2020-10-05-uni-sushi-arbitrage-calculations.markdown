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

Uniswap algorithm adds 1 wei to right side of the eq (2).

Now we assume of an operation of purchase of token for ETH at exchange #1
and selling it at exchange #2 for ETH and introduce this notations

```
x1 - amount in at exchange #1
x2 - amount out at exchange #1 and amount in at exchange #2
x3 - amount out at exchange #2
y1 - reservesIn at exchange #1
y2 - reservesIn at exchange #2                                   (3)
z1 - reservesOut at exchange #1
z2 - reservesOut at exchange #2
k  - fee coefficient (k = 0.997)
```

Then output amount of token at exchange #1 would be equal to input amount of
token at exchange #2

```
        y2 * x3      k * x1 * z1
x2 = ------------- = -----------  (4)
     k * (z2 - x3)   y1 + k * x1
```

For output amount `x3` at exchange #2

```
     k * x2 * z2
x3 = ----------- (5)
     y2 + k * x2
```

If we rewrite (2) in terms of (3)

```
        y1 * x2
x1 = -------------  (6)
     k * (z1 - x2)
```

We need to maximize the difference between amount spent `x1` and amount
received `x3` by `x2`

```
                     / | k * x2 * z2      y1 * x2    | \
max(|x3 - x1|) = max|  | ----------- - ------------- |  |  (7)
                     \ | y2 + k * x2   k * (z1 - x2) | /
```
If we take a derivative of (7) by `x2` and look for extremum we will get
quadratic equation

```
k^3 * (z2 * y2 - z1 * y1) * x2^2 - 2 * z1 * k^2 * y2 * (y1 + k * z2) + k^3 * z2 * y2 * z1^2 - k * y1 * z1 * y2^2 = 0  (8)
```

The roots are

```
                                                 __________________
     z1 * y2 * (y1 + k * z2) ± (y2 + k * z1) * \/ z2 * y2 * z1 * y1
x2 = --------------------------------------------------------------  (9)
                       k * (z2 * y2 - z1 * y1)
```
