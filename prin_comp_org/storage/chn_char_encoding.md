This note will discuss how to convert the encoding between 3 different _Chinese Character Encoding_ method. 

# Chinese Character Encoding

There are 3 encodings:

- 区位码 Area code.
- 国标码 Intl code.
- 机内码 Internal Code.

# Convertion Rules

Consider we already know area code: `4636(dec)`. (This is the code of 文).

## Area Code -> Intl Code

### Conclusion

- Divide into two part (dec).
- Change to Hex.
- Add `20h` to each part.

<details>
<summary>Example</summary>

First **divide Area code into two part**: `46` and `36`, then **use Hexidemical to reinterpret** these two part:

```
46(dec) -> 2e(hex)
36(dec) -> 24(hex)
```

Use one byte to represent each part, **then add `20(hex)` to each byte**:

```
2e + 20 = 4e
24 + 20 = 44
```

Then we get the _Intl Code_: `4e44(hex)`

</details>

## Intl Code -> Internal Code

Just **add _Intl Code_ by `8080(hex)`**

```
4e44 + 8080 = cec4
```

So the _Internal Code_ is `cec4`.

# Refs

> Textbook P100