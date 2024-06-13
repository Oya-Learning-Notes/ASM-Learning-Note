How to encoding instructions?

Textbook mentioned 3 ways.

- Fixed Length Encoding
- Huffman Encoding
- Extend Encoding

> Textbook P168

First two could be checked out on textbook, we don't discuss them here.

# Extend Encoding

Extend Encoding means select some certains different bits length to use.

## Code Length & Code Points

_Code length_ represents __the bits count of this code__.

_Code Points_ represents __how many different situation this code could distinguish__.

For example, code `xxxx`.

- Code Length = `4`.
- Code Points = `2^4` = `16`.

## Representation of Extend Encoding

There are two representation method of this encoding.

- `X/Y/Z/... Encoding`

This means the ___Code Points_ of different length encoding are `X`, `Y`, `Z`, ...__

- `P-Q-R-... Encoding`

This means the ___Code Length_ choosen are `P`, `Q`, `R`, ...__

Example:

```
00
01
10

11001
11010
11011
11100
11110

1111101
1111110
1111111
```

Then this _Extend Encoding_ method could be represents by:

- `3/5/3`, `2-5-7`

## Rules When Encoding

- Unless it's already the longest _Code Length_, __All-One Code is reserved__.
- Instruction with less _Address_ param should be longer, __reserved the bits for instructions that have more bits to represent _Address___.

Example:

![IMG_3198](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/35a07e49-37f7-4d50-917a-942203ff7835)

> For more info, check out Textbook P173

## Code Points Calculations

Here we are going to learn how to calculate the count of code points in different code part, the total possible instruction count, and the relations between prefix and code points.

We use an example question:

> Q: One system using `3-6-9` encoding. Has `M` most-used instruction, `N` second-most-used instruction.
>
> 1. What's the maximum instruction count of this system?
> 2. What's the max `N`? In this case what's the maximum instruction count?
> 3. Similar to 2 but when `M` is max.

-----

- Maximum Instruction Count:

In previous example, we only reserve all-one (`1111...`) prefix for longer instruction. So we don't need to consider prefix issue when calculating the total code point. But here we need.

__If there are more then one prefix the shorter length code reserved, we need to multiply the prefix count__ when calculating the total code points of longer code part. 

Here the prefix reserved in:

- `3 bits`: $R_3 = 2^3 - N$
- `6 bits`: $R_6 = R_3 \cdot 2^{6-3} - M$

> Notice that the prefix are added on layer by layer.

So the total code points of `9 bits` is: 

$$
\begin{align}
C_9 
&= 2^{9-6} \cdot R_6\\
&= 2^{3} \cdot [(2^3 - N) \cdot 2^{6-3} - M]
\end{align}
$$

Here, $C_9$ means the total useable code points of `9 bits` code part.

-----

- When $N$ is it's max

From (1) we know:

Here the prefix reserved in:

- `3 bits`: $R_3 = 2^3 - N$
- `6 bits`: $R_6 = R_3 \cdot 2^{6-3} - M$

Prefix should be positive ($R_i \ge 1$), so $M_{max} = 7$.

If in this case, we want a maximum total code points number, we should shrink $M$ to it's min $M_{min} = 1$. Then we could use the formula in (1) to calculate.

Final answer is:

$$
\begin{align}
C_3 + C_6 + C_9
&= N_{max} + M_{min} + C_9 \\
&= 7 + 1 + 2^{3} \cdot [(2^3 - N) \cdot 2^{6-3} - M] \\
&= 8 + 8 \cdot ((8 - 7) \cdot 8 - M) \\
&= 8 + 8 \cdot (8-1) \\
&= 8 + 56 \\
&= 64
\end{align}
$$

### Max Total Instruction

Based on Textbook P175, if we want maximize the total count of instruction, we need to use as least short code points as possible.