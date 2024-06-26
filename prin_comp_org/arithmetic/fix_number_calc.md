This post will discuss the multiplication of _fixed point number_ . Please make sure you have the basic knowledge about _number encoding_ , for more info, check out [Number Encoding Notes](./number_encoding.md)

# ORG Encoding Multiply

When two number is represented by _original encoding_ , the multiplication operation will be really simple.

Just like we have done with `dec` number, we just multiply all the significand part, and **the sign of ans is acquired by the `XOR` of the two sign of two oprand**.

Notice that we __use two bits to represents sign in tmp sum and do NOT use bit to represent sign in mul__.

![IMG_2127](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/bd816a24-a7da-4f1c-b636-1d669050c104)

Notice that: If the bit count of `oprand 2 ` is `n`, then we do `n` times `(+, ->)`

> Checkout textbook 107

# TWC Encoding Multiply

Therer are two method to calculate `TWC` mul.

## Calibrate (校准法)

### Steps

- First do multiply like `ORG` encoding.
- Then if _oprand 2_ is negative, add `-X(TWC)` to the result.

### Notice

- Even we **only use significant part of `Y`, we still need to convert it to `TWC` first**.

For example, if `Y = -0.1011`. Then:

```
Y = 1.0101 (TWC)
Using: 0101
```

## Comparison

Checkout textbook P115

![IMG_2128](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/f3c86057-f9e2-4c0c-9947-5ca220178012)

As the img says: Everytime after `SHR`, we compare `extra - low_bit_of_mul`:

- res < 0: tmpAns += (-X)(TWC)
- res = 0: tmpAns += 0
- res > 0: tmpAns += (X)(TWC)

> `X` means oprand 1 here.

### Notice

- `tmp_ans` has *2-bit* sign. `mul` has *1-bit* sign.
- Use `extra`, and the initial value is `0`. After initialize the `extra`, do a `ADD` operation, considered as preprocess.
- Use `(->, +)` as basic unit, do `n` times.
  - Use _TWC bit shift rules_ when do `SHR`.
  - _(n is the bit count of the `oprand 2`)_
  - *(Preprocess has been ignored)*
- Deprecate one more bit in the `mul` part when finished. (Anyway the bits count of the final result should be `m+n`. _(m is the bit count of `tmp ans` and n is the bit count of oprand 2)_.)

# ORG 2-Bits Multiply

One bit a time is too time wasting, so we want to __calculate 2 bits__ at a time. The basic thought is:

- Using state of 2 bits to update tmp sum.
- Perform `SHR 2` *(Arithmetic Shift, preserve sign)*

![ORG Two Bits Multiply](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/00ac530b-d16b-42c5-8699-3fea951b5555)


## State Corresponding Operation

![IMG_3132](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/8a650903-708b-4bc1-acaf-65da19d7a6c2)

As the images implys:

Based on the number of the two bits _(could be `0, 1, 2, 3`)_ , we correspondingly add `0, X, 2X, 3X` to the tmp sum.

But this is only the theoretical rules, we also need some tricks on it. Check out the passage below.

## Tricks when doing operation

### Divide +3X to -X+4X

Add `0`, `X`, and `2X` is easy. (we just do `SHL` on `X` to get `2X`)

But how we can do `ADD 3X`?

The answer is we divide `+3X` into `-X + 4X`. That is in this round, we only do `-X`, and we set up a flag `F` to represent if we need to do `+4X` in next round.

Notice because __we do a `SHR 2` before into next round__, so actually __we only need to do `+4X / 4 = +X` in next round__.

The final rules is like the image below:

![IMG_3133](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/21e1c6f2-fc39-4a3f-ad87-a9673b9e5020)

> For more info, checkout Textbook P117.

### Use +(-|X|(TWC)) to do -X

Since we use -|X|(TWC), so when we do `SHR`, __we need to use the bit shift rules of Two's complement__. 

Notice: the `mul` part is still using `|X|`, and the final sign is still be acquired by XOR the sign of two oprand. The TWC here only be used to achieve `-X`.

## Tricks when MUL is odd

There is two ways to solve this.

__Add `0` to right most bit__

For example if the `mul` is `0.01011`, then make it to be: `0.010110`. Then when retrive the result, __abandon the right most bit__. For example if the result is `000.10111 110100`, then we only want `000.1011111010` part.

Anyway the point is the __bit count of the result significand is always the significand bit count sum of two oprand__. This rules always applys (we use this rules in TWC one bit multiply too)

__Add `0` to the left most bit__

This time The __last `SHR` opreation only shift one bit__.

## Conclusion

- Always convert `X` to `|X|` first. Notice we may use `-|X|(TWC)`, not `-X(TWC)`
- Divide +3X operation to -X and +4X. Set up flag.
- Use `3` bit to represent sign of tmp sum.
- Use _TWC bit shift rules_ when do `SHR`.

# TWC Two Bit Multiply


## Notices

- `tmp_sum` has *3-bits* sign. `mul` has *1-bit* sign.
  - Because we use one bit to repr sign in `mul` part, the **significand part now should be odd and not even**.
- The last shift only `shift` one bit.

![TWC Two Bits Multiply](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/b9d350c3-eaa7-4768-93f5-f175a702869d)

## Perform Rules

![IMG_3134](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/d47541c0-33e4-47e1-8434-1425e6645e91)

The image above shows the rules.

## When MUL is even

__Add one more sign bit to mul__

Then we don't do `SHR` at all.

For example, `1.01001` to `11.01001`.

__Add `0` to rightmost bit of `mul`__

Don't change any operation. The last time we still need to do `SHR 1`.

We found that both in `ORG` and `TWC` two bit multiplication, when __adding bit to right, then it will not affect the operation__. But when __adding bit to left, then we will decrease `SHR` by one time__.

# ORG One Bit Division

## Notices

- `tmp_sum` has *2-bits* sign. `ans` part don't need sign.
- *Final answer* has `1-bit` sign. For example, answer part is `010111`, then *Final Answer* is `0.10111`
- Use `(test, <-)` as a unit. But at last we could **test one more time but not perform shift** to get more accurate resule *(or don't test and put `1` to the final bit of answer)*.

![IMG_3147](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/b8e3ba10-08c3-43a8-8f62-bb9dbad2f42e)

As the image above.

![One Bit Division](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/c7412201-c2a8-4514-9802-5a20a5e09abc)

> Notice: Answer is wrong, which don't take the sign into consideration. Correct answer should be `1.10110`

## Leftmost Bit In Result

Notice that this method required _opr1 < opr2_, so the _leftmost_ bit of the result will be zero. And __this bit will be covered by _sign_ bit__.

```
Result: 0010011
Sign: 1
Answer: 1.010011
```

# ORG One Bit Division (Optimized)

Consider what we need to do in previous method:

Consider the rest is `R`, `opr2` is `Y`.

- `R_i < 0`: `R_i += Y` `<-` `R_(i+1) - Y`
- `R_i > 0`: `<-` `R_(i+1) - Y`

> <- means _left shift_

So how can we simplify it? Notice the first situation. We prefer to not do `+=` part and directly left shift.

```
SHL (R_i - Y) = R_(i+1) - 2Y
R_(i+1) - 2Y + Y = R_(i+1) - Y
```

So we found that if `R_i < 0`, then we can __directly shift, and replace the `-Y` opreation to `+Y` operation.__

> Textbook P125

# Refs

This note page has a [Legacy Version](./fix_number_calc_legacy.md)