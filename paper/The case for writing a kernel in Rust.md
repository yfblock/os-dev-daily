本论文中技巧应用在已有的 `tock os` 中。
## 较为经典的应用
### TakeCell
```rust
struct App {
	count: u32,
	tx_callback: Callback,
	rx_callback: Callback,
	app_read: Option<AppSlice<Shared, u8>>,
	app_write: Option<AppSlice<Shared, u8>>,
}

pub struct Driver {
	app: TakeCell<App>,
}

driver.app.map(|app| {
	app.count = app.count + 1
});
```
这个 `map` 很喜欢😄。

## 技巧
1. 利用 `Cell` 和 `TakeCell`  来实现资源的 `Multiple References`.
2. 提前约定好操作层，然后再进行挂载和使用。