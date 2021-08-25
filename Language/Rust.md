# Rust
Rust 是好活，自从我校2016年以 Rust 语言开设 CS100（程序语言设计）开始，上科大就成为了宣传Rust的堡垒，中国 Rust 之父张汉东先生及宣发 Rust 的各界人士选择推广 Rust 的最佳地点就会选择上科大，这雨与 riscv 类似。写小的bash工具（ Zero Cost Abstraction 的 cffi ）、写大型（10w+ line）的系统方向程序必备。由于语言特性有很多的静态检查，会指导大家对于内存管理，异步编程有更深刻的理解。

## 学习参考资料
1. rCore - 清华维护的教学操作系统
2. Libra - 脸书维护的区块链数据库
3. 飞书 - Tokio 代码池


## 异步中的`Sync`和 `Send`
https://kaisery.github.io/trpl-zh-cn/ch16-04-extensible-concurrency-sync-and-send.html
### `Send`
```rust
use std::rc::Rc;
use std::sync::Mutex;
use std::thread;

fn main() {
    let num = Rc::new(Mutex::new(0));
    let mut handlers = vec![];
    for i in 1..10 {
        let num_copy = num.clone();
        let handle = thread::spawn(move || {
            *num_copy.lock().unwrap() += 1;
        });
        handlers.push(handle);
    }
    for handler in handlers {
        handler.join();
    }
    println!("{}", num.lock().unwrap());
}
```
这段代目不能通过编译, 原因是`num_copy`在`move`到线程中时，可能会多线程同时修改引用计数。所以在Rust中，`Rc`没有`Send` trait,因为它不允许在线程间转移所有权。
```
error[E0277]: `Rc<Mutex<i32>>` cannot be sent between threads safely
   --> src/main.rs:10:22
    |
10  |           let handle = thread::spawn(move || {
    |  ______________________^^^^^^^^^^^^^_-
    | |                      |
    | |                      `Rc<Mutex<i32>>` cannot be sent between threads safely
11  | |             *num_copy.lock().unwrap() += 1;
12  | |         });
    | |_________- within this `[closure@src/main.rs:10:36: 12:10]`
    | 
   ::: /home/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:624:8
    |
624 |       F: Send + 'static,
    |          ---- required by this bound in `spawn`
    |
    = help: within `[closure@src/main.rs:10:36: 12:10]`, the trait `Send` is not implemented for `Rc<Mutex<i32>>`
    = note: required because it appears within the type `[closure@src/main.rs:10:36: 12:10]`
```
### `Sync`
```rust
use std::cell::Cell;
use std::thread;

fn main() {
    let num = Cell::new(0);
    let mut handlers = vec![];
    for i in 1..10 {
        let handle = thread::spawn(|| {
            num.set(i);
        });
        handlers.push(handle);
    }
    for h in handlers {
        h.join();
    }
    println!("{}", num.get());
}
```
这段代目不能通过编译, 原因是`Cell<T>` 我们在多个线程中共享`&Cell<T>`时, 多可线程可以并发地修改其内部的值，这并不安全。所以`Cell<T>` 实现了`!Sync` trait。
```
error[E0277]: `Cell<i32>` cannot be shared between threads safely
   --> src/main.rs:8:22
    |
8   |         let handle = thread::spawn(|| {
    |                      ^^^^^^^^^^^^^ `Cell<i32>` cannot be shared between threads safely
    | 
   ::: /home/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:624:8
    |
624 |     F: Send + 'static,
    |        ---- required by this bound in `spawn`
    |
    = help: the trait `Sync` is not implemented for `Cell<i32>`
    = note: required because of the requirements on the impl of `Send` for `&Cell<i32>`
    = note: required because it appears within the type `[closure@src/main.rs:8:36: 10:10]`

```
