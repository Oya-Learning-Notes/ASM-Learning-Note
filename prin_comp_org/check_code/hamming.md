In this note we are going to talking about hamming code. This post will NOT fully introduce the hamming code, but only record some important point.

# Data Bit and Check Bit

Assume that:

- `k`: The bits of data.
- `r`: The number of check bits.

To positioning an error during `k + r` bits, we need `log2(k + r)` bits. In another word, __`r` check bits could positioning errors from `2^r` bits__.

However in hamming code we __need an extra _Check Bit_ to verfiy the whole data and check bits sequence__.

So ultimately, __if valid check bits is `r`, the total length `k + r <= 2^(r - 1)`__

Here is an example table showing the relationship of `k` and `r`.

| r | total = 2^(r-1) | k (k &lt; total - r) |
|---|-----------------|----------------------|
| 4 | 8               | [1, 4]               |
| 5 | 16              | [5, 11]              |
| 6 | 32              | [12, 26]             |
| 7 | 64              | [27, 57]             |

# Refs

[3Blue1Brown - But What Are Hamming Code](https://youtu.be/X8jsijhllIA)

> Check textbook P150 for more info.