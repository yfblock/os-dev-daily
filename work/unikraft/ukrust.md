## ukrust
目前这个库主要是为了支持可能存在的 `rust` 模块和应用程序，因此在这个模块中实现了 `rust` 的 `alloc`，以便在后面的 `rust` 模块中直接使用。目前这个模块利用 `bindgen` 来包装 `uk_malloc` 和 `uk_free` 等接口。`bindgen` 将 C 语言代码生成 `Rust` 辅助库。

```rust
#[allow(
	non_snake_case,
	improper_ctypes,
	non_camel_case_types,
	non_upper_case_globals,
	unsafe_op_in_unsafe_fn
)]

mod bindings_raw {
	include!(env!("LIBUKRUST_BINDINGS_FILE"));
}
pub use bindings_raw::*;
```

## Install Rust Bindgen
> [Click here to see bindgen info](https://crates.io/crates/bindgen)
``` shell
$ cargo install bindgen-cli
```
