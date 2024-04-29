There are three mapping method:

- Fully Associated Mapping (全相联)
- Direct Mapping (直接映射)
- Group Associated Mapping (组相联)

# Fully Associated Mapping

![image](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/098250de-3f0a-4824-b97b-3a051bbbefe3)

- `m`: bits used to represents memory block id.
- `c`: bits used to represents cache block id.
- `b`: bits used to represents in-block storage unit id.

## Accesss

When received `24` bits address:

- Lower `b` bits directly passed to cache as `b`.
- Higher `m` bits goes to lookup table.
  - Lookup table do **full scan** to find if there's a valid `table[m] = c`
  - If no, then not hit.
- Use the founded `c` as the higher bits `c` to access cache.

# Direct Mapping

![image](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/0ecd7921-f1c6-4c7b-8120-b3ac0d0f5aa4)

From the image we know, consider the block number $C = 2^c$ :

$$
i = MemBlockId \mod C
$$

Those block with same calculation result $i$ with share the same cache block. For example in pic above $C = 2^c = 2^7 = 128$ , then 0, 128, 256, ... blocks in memory will both share the cache block with index `0` (That means the first block in cache).

## Accesss

When received `24` bits address:

- Lower `b` bits directly passed to cache as `b`.
- Middle `c` bits: Goes to higher cache address as `c`. Goes to table and select **one of the** $2^c$ th row in the table.
  - The `table[c]` will give us a `t` bit data, compare it to the `t` in the memory address. If not equal, miss.
- If equal, access successful.

Notice in this case we don't need a full lookup to the table, because the middle `c` part directly select a row in the table, we just need to compare them.

# Group Associated Mapping

![image](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/6af70b73-2d8c-4d3a-a682-e232de21b9a9)

This method combine the thought of the previous two methods.

- `q` bits used to represents group id.
- `r` bits used to represents in-group block id.


## Conversion to previous methods

- When `q = c, r = 0` => Direct Mapping
- When `q = 0, r = c` => Full Associated Mapping

## Accesss

- Lower `b` from memory directly goes to cache as `b`.
- Middle `q`: Directly goes to cache as `q` (group id). Goes into table to select a row.
  - Check if this row contains higher `t` of the memory. (If not exist, miss)
- If hit, we will got `r` part from the table, it goes to cache as the `r` part.