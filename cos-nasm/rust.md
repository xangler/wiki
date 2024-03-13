# Rust 使用

## rust 版本管理
```bash
rustup update
```

## rust 项目管理
```bash
cargo init/new
cargo build
cargo run 
```

## 修改默认源
配置文件(${HOME}/.cargo/config)
```
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
replace-with = 'tuna'
[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"
```