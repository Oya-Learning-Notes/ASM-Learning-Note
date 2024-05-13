There are different ways to represent a number in binary.

- Orginal (ORG)
- One's Complement (ONC)
- Two's Complement (TWC)
- Offset (OFF)

# Original

This code just use `symbol` bit and the `significand` bits to represent numbers. `0` means positive and `1` means negative.

# One's Complement

When the number is positive, same to orginal code. When negative, just reversed all bits except the sign bit of orginal code.

```
3(dec) = 0011(bin) = 00011(org)(onc)
-3(dec) = -0011(bin) = 10011(org) = 11100(onc)
```

## Calculation

To perform add on two `ONC` encoding number.

1. Add the binary of them. (Including sign bit)
2. If there is carry from the sign bit, then add it back to the rightmost bit. (Cycling carry)

```
-4 (DEC) + (-6) (DEC)
1011 (ONC) + 1001(ONC)
= [1]0100
= 0100 + 1
= 0101 (ONC) = 5 (DEC)
// Here negative overflow occured and 5 is the complement of -10 mod 15
```

# Two's Complement

Number is positive: Same to ORG.
Number is negative: Euqals ONC + 1.

## Overflow Detection

Here we are going to discuss how to detect _overflow_ when performing add operation with `TWC`.

Notice that we recognize _Positive Overflow_ and _Negative Overflow_ .

1. Symbol Comparison

We could easily found **when two operand have different sign, then it's impossible for overflow to occured**.

So there are two overflow occasion:

- Both positive, got negative number. (Positive Overflow)
- Both negative, got positive number. (Negative Overflow)

2. Dual Carry Method

For any numbers add operation, observe two carry behaviour.

- Significand Highest Bit Carry Behaviour `C`
- Symbol Bit Carry Behaviour `C_f`

When:

- `C == C_f`, the result is valid.
- `C != C_f, C = 1` Positive Overflow
- `C != C_f, C = 0` Negative Overflow

![IMG_2124](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/a2054c98-a3ec-45e6-85e1-f15a8525969e)

The image above shows how this method works. Notice **`C == C_f` will always true when two numbers have different sign**.

3. Sign Expand

This method is two expand sign bit from 1 bit to 2 bit. Or you can say is to temporarily expand the whole number size by one bit.

- `0` -> `00`
- `1` -> `11`

Then we just just need to perform add operation to these two expanded numbers, and check if two sign bit is identical.

- Two sign bit identical: Result Correct.
- Two sign bit not identical： `10` Negative Overflow. `01` Positive Overflow

![image](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/1597f357-5c8d-47e0-8a04-e2cf470e9a3d)

Check out image above to learn more detail about this method.

# Offset

For n-bit storage, `OFF` = `ORG` + `2^n-1`.

This encode make _addition_ of two numbers very simple. In _IEEE 754_ , the exponent part is stored using the _Offset Encode_ .