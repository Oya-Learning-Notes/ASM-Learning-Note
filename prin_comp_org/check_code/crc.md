# Cyclic Redundancy Check

CRC is a widely used method to check the integrity of the data. Check [Wikipedia Page](https://en.wikipedia.org/wiki/Cyclic_redundancy_check).

# Basic Mechamism

The core calculation in `CRC` is `XOR Divide`. __In `XOR Mul` and `XOR Div`, we don't consider _Carry_ and _Borrow_.__

> Check textbook P154 if you don't familiar with _Binary XOR Divide_.

First, __we convert _Data Bits_ into a _Polynomial_, we called it _M(x)_.__

Second, we need to reserve place for check bits. Assumte we have `r` check bits, then we left shift _M(x)_ by `r`, or we could say: __Multiply M(x) by x^r__.

Then, we need to calculate `XOR DIV`, __Calculate M(x) XOR DIV G(x) == R(x) ... Q(x)__. Here G(x) is the key polynomial that has been determined in advance.

Finally, __add M(x) by Q(x)__, then we got final result. (Here the M(x) is the one has been left shifted)

Image below is an example of calculating `CRC`.

![IMG_2156](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/3f25fd7c-ff47-4a8f-b750-2e8b9ffe8efd)

# Check And Correction

The most __important part that makes a great CRC algorithm is to find a great G(x) polynomial__, it should satisfy the condition below:

- Any error will cause non-zero Q(x)
- Different _Error Position_ lead to _Different Non-zero Q(x)_
- Q(x) / G(x) should loop.

# Correction Step

Notice since the $Q(x) / G(x)$ loops, we __use this characristics and _Shift Left_ operation to finished the correction__.

![IMG_3202](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/84003703-80e8-4b71-a378-c6689dc80a75)

# Refs

[Wikipedia - CRC](https://en.wikipedia.org/wiki/Cyclic_redundancy_check)