目前使用 `inventory` 出现了一些问题，`Inventory` 似乎不支持 `target = "none"` 的情况（由于 `ctor` 不支持，`Inventory` 是基于 `ctor`，`linkme` 工作也不正常。）

## Linkme
`Linkme` 在使用过程中出现链接错误，似乎是没有找到相应的段，后面可能需要看看再进行一下特殊处理？