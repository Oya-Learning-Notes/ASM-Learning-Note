This post will discuss the multiplication of _fixed point number_ . Please make sure you have the basic knowledge about _number encoding_ , for more info, check out [Number Encoding Notes](./number_encoding.md)

# ORG Encoding Multiply

When two number is represented by _original encoding_ , the multiplication operation will be really simple.

Just like we have done with `dec` number, we just multiply all the significand part, and the sign part is acquired by the `XOR` of the two sign of the different number.

```
0.1011
0.1001
= 1011 + 0 + 0 + 1011000
= (0*0).1100011
= 0.01100011
```

![IMG_2127](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/bd816a24-a7da-4f1c-b636-1d669050c104)

If the bit count of `oprand 2 ` is `n`, then we do `n` times `(+, ->)`

> Checkout textbook 107

# TWC Encoding Multiply

Therer are two method to calculate `TWC` mul.

## Calibrate (校准法)

First do multiply like `ORG` encoding.

Then if _oprand 2_ is negative, add `-X(TWC)` to the result.

## Comparison

Checkout textbook P115

![IMG_2128](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/f3c86057-f9e2-4c0c-9947-5ca220178012)

As the img says: Everytime after `SHR`, we compare `extra - low_bit_of_mul`:

- res < 0: tmpAns += (-X)(TWC)
- res = 0: tmpAns += 0
- res > 0: tmpAns += (X)(TWC)

> `X` means oprand 1 here.

Notice we __use two sign to represent bits__ in calculation.

When using this method, we need to first add an _extra_ bit `0` to the `mul` part and do an `add`. **This could be considered a preprocess**.

Different from `ORG`, here the operation group becomes `(->, +)` and we do `n` times. _(n is the bit count of the `oprand 2`)_

Also notice that when finished calculation, **we need to deprecate one more bit in the `mul` part.** Anyway the bits count of the final result should be `m+n`. _(m is the bit count of `tmp ans` and n is the bit count of oprand 2)_