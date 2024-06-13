There are two ways to encoding the address of _Cache_.

# 高位交叉编址

This method also called _Sequence Addressing_. Check out the image below:

![IMG_3234](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/5aa764f6-b404-4b95-b9d1-334babb6245a)

## Access Time

Assuming that the cache module unit size is one word _(or you can say the size of cache module is identical to data bus)._ The time consumed to access consecutive $m$ words will be:

$$
ETA = mT
$$

In which:

- $m$: Count of consecutive words you want to read.
- $T$: R/W cycle time of a module.

## Pros and Cons

Pros:

- Error will only have local affect.

Cons:

- Slow compared to low-bit inter-encoding.

# 低位交叉编址

![IMG_3235](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/06fcd249-ca4f-44e5-abef-5604ea01b209)

![IMG_3236](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/5112bf35-8e99-4a8a-8c75-a2fc2e238398)

## Access Time

With same condition as above, the  time will be:

$$
ETA = 1 \cdot T + (m - 1) \cdot \tau 
$$

In which:

- $m$ and $T$: Same meaning as above.
- $\tau$: The working cycle of CPU data bus.

Limitation:

If using this addr encoding, we should ensure:

$$
T \ge m \cdot \tau
$$

The only things we could change is $m$, that is the count of cache module. So we usually called: $m = \frac{T}{\tau}$ the inter-rate _(交叉存取度)_.

## Pros and Cons

Pros:

- Fast

Cons:

- Once a module has error, it would have __global impact__.
