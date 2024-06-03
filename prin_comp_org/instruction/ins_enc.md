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

> For more info, check out Textbook P173