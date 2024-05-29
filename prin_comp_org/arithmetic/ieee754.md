In this post we are going to discuss the how the floating numbers stored in computer.

Most of current computer system comply with _IEEE 754_ , which is an international standard to store such floating point number into computer.

# How to use this post?

This post is a complement and extra explanation, it's recommend that you check out the [IEEE 754 Wikipedia Page](https://en.wikipedia.org/wiki/IEEE_754) first.

And when you find some hard-to-understand conceptions and mechanisms, back to check if this post has some extra content can help you.

# Structure

As the name imply, floating point means the decimal point could _floating_ , and this floating is produced by the mutate the _exponent_ part.

```
2.30e2
2.30e3 = 23.0e2
```

In above example, you can imagine the decimal point shift right.

> In case some readers don't know, e[x] here means 10^x, for example, 2.3e3 means 2.3*10^3. _e_ here represents _Exponent_ .
>
> Also we need to know the base of the exponent could vary. For example _IEEE 754_ standard allows use `2` as base of a floating point number.

In _IEEE 754_ , a floating point comprise 3 parts:

- Sign
- Exponent
- Fraction

A simplified sturcture diagram could like the image below:

![IMG_2038.PNG](https://s2.loli.net/2024/03/08/tIQbWAyGVJMzsUe.jpg)

There are both standard to store binary floating number and decimal floating number, and their mechanism is the similar. For convenience, the following post will take binary floating number as example.

## Patterns

- _sign_ Always 1 bit. 0 represents positive, 1 represents negative.
- _exponent_ & _significand_ Varys from different type of floating point number.

# Bias for exponent

![IMG_2039.PNG](https://s2.loli.net/2024/03/08/DuaO5qPRXyEoM7m.jpg)

As you see from the picture above. For _n-bits_ exponent, its bias is $2^{n-1} -1$ .

More directly, if there is an _7-bits_ exponent `???????`, then its bias is `0111111`.

The value in memory, bias, and the actually represented value has following relationship:

$$
ActualValue = InMemory - Bias
$$

Check the example picture below: (In which Org means the value in storage/memory, the middle column means bias, and the right column means the actual represented value)

![IMG_2041.PNG](https://s2.loli.net/2024/03/08/twVpqzGQD8UubOM.jpg)

Notice here the encode could not be directly considered as the `Offset Encoding`. Because in _Offset Encoding_, the shift number will be $2^(n-1)$, but in _IEEE 754`, the shift number is $2^(n-1) -1$.
## Special Cases

- All Zeros. This represents _subnormal_ numbers in the standard. And which means hidden one convention will be disabled.
- All Ones. This indicates _infinity_ or _NaN_ . Check [Wikipedia](https://en.wikipedia.org/wiki/IEEE_754#Interchange_formats) for more info.

# Hidden One Convention for significand

![IMG_2040.PNG](https://s2.loli.net/2024/03/08/OU9umAl2aNnvsCG.jpg)

As you see, when we representing the normal numbers (in the normal range of the standard), then we could **always expected that the first bit in the significand of the number is `1`**. So we could just ignore this `1`, which could help us save one bit. (Notice this convention doesn't work when we are representing numbers in the subnormal ranges).