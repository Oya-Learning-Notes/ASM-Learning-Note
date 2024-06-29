# Cache Mapping Address Structure Question

![image](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/ba5a7473-d034-42f4-9e2b-03d2e44e9314)

![image](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/a3c58957-9a4b-4b44-adb8-4b04836f949f)

> Wangdao 2025 PCO (Principle of Computer Organization) P148(PDF Page)

# About Markup Matrix (标记阵列)

**Every block has an Markup Row**, all this rows consist of the whole Markup Matrix. Generally, **Markup Matrix consists of two part**:

- **Tag part**. Contains the info that this block in cache mapped to which block in Mem.
- **Markup part**. Related to the algorithm used in cache management. For example:
    - `Mutated` bit when using WritingBack(回写法)
    - `Count` bits used in `LRU` algorithm (it usually uses 2 bits).

## Fully-Associated Mapping

![image](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/098250de-3f0a-4824-b97b-3a051bbbefe3)

In image above, the `m=16` part will become `Tag` part.

## Direct Mapping

![image](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/0ecd7921-f1c6-4c7b-8120-b3ac0d0f5aa4)

In image above, the `t=9` part will become the `Tag` part.

## Group Mapping

![image](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/6af70b73-2d8c-4d3a-a682-e232de21b9a9)

In image above, the `t=10` part will become the `Tag` part.

