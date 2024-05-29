In this notes we are going to talking about calculation of _Floating Point Number_.

In this notes, all `E` (Exponent) and `S` (Significand) part is stored in `TWC`(Two's complement) encoding.

Below is terminology table in this post.



# Exponent Alignment

Some calculation require us to align the _Exponent_ part of two number.

![IMG_2151](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/552cfbf6-11bf-4c10-9e7b-139aae4a9726)

The image above shows how we align the _Exponent_ of two floating point number.

Notice that with __consideration of precision, we usually keep the larger exponent__ and align the small one to the greater one.

# Exponent Addition

There are total 5 steps when adding FLP.

1. Exponent Alignment
2. Significand Calculation
3. Formatting
4. Rounding (Possible Formatting Again)
5. Overflow Check

## Step 1: Alignment

This step do exactly the things we talk in _Exponent Alignment_ part.

### Temporary Bit

Notice: When doing _Right Shift_, we should __set up an _temporary bit_ to store the bit that has been moved out__.

This bit has two usage.

First usage is in _Step 3: Formatting_. there may have some _Shift Left_ operation, then we could put the bit in _temp bit_ back instead of using 0 to fill.

Second usage is in _Step 4: Rounding_.

## Step 2: Calculation

We just add up the _Significand_ part of two numbers. We are free to choose several different calculation method we learnt before _(E.g.: `ORG add`, `TWC add` etc.)_

Notice we need to __use at least 2 bits sign bits in calculation__. This will be used to do formating work in step 3.

## Step 3: Formatting

In this step, we check if two sign bits are identical.

__Sign bits identical, but significand not satisfy the format__

This means the __leftmost bit of significand is NOT 1__. This time we need to __do _Shift Left_ until the leftmost bit becomes 1__. Notice we should use the _Temporary Bit_.

For example, if significand is `001011`, temporary bit is `1`, then we shift it to `101110`, and decrease the _Exponent_ part by `1`.

__Sign bits not identical__

This means carry happened on the leftmost bit when calculating with significand. __Do _Shift Right_ one time__.

## Step 4: Rounding

Check the temporary bit.

- If `0`, do nothing.
- If `1`, add _Significand_ part by `1`. 

Notice if this add __deformat the significand part, we need to do _Formatting_ step again__.

## Step 5: Overflow Check

Check if the `exponent` part is out of range.

- Exponent All Zero: Lower Overflow, Machine Zero.
- Exponent All One: Upper Overflow, Error.
