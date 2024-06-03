Cache need to be refresh. There are 3 different refresh strategies metions in textbook.

# Unified Refresh (集中刷新)

Use a consecutive _R/W Cycles_ to refresh cache.

Consider we have a `1K * 1Bit` cache. Since `1K = 32 * 32`. So means __there are `32` rows.__

So, we need `32` consecutive `R/W Cycle` to refresh.

If the _Refresh Cycle_ is $t_r$. _R/W Cycle_ is $t$:

Then the total refresh time:

$$
t_{refresh} = t \cdot c_{rows}
$$

- $t$ is _R/W Cycle_ time.
- $c_{rows}$ is the number of rows in the cache. Here is `32`.

![IMG_2158](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/d3a2f19b-9665-4981-8590-3da90469658f)

The __`dead area` means R/W request could not be dealt__ in this time period.

> Here notice _Refresh Cycle_ and _R/W Cycle_ are completely different.
>
> Refresh Cycle means the time consumed to make the whole storage to refresh, and the R/W cycle is the basic cycle to read or write.

# Distributed Refresh

> Textbook P41

# Async Refresh

> Textbook P42