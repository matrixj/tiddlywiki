# 背景

# 处理方式
以下通过**打开本地文件**为例子，展示不同风格的错误处理方式。

## c风格错误处理
错误码

## 异常风格错误处理


## Rust 错误处理
这里的错误处理主要集中于Rust 的 可恢复错误（recoverable）`Result<T, E>`, 它是由`std`提供的一个错误实现。
T为返回成功时的结果类型，E为错误的类型,以下不熟悉rust语法直接看注释:),这里的例子使用了std::io::Result的实现来详述细节

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt");
    let f = match f {
        Ok(file) => file,
        Err(error) => match error.kind() { // 展开error.kind()再进行匹配
            ErrorKind::NotFound => match File::create("hello.txt") { 
                Ok(fc) => fc,
                Err(e) => panic!("Problem creating the file: {:?}", e),
            },
            other_error => panic!("Problem opening the file: {:?}", other_error),
        },
    };
}
```
File::Open返回值的定义：
```rust
// 辅助定义
pub enum Result<T, E> {
    Ok(T), //Ok为T类型
    Err(E), // Err为E类型
}
pub struct Error {
    repr: Repr,
}
enum Repr {
    Os(i32), //错误码
    Simple(ErrorKind), //错误类型
    Custom(Box<Custom>), // 用户定义类型
}

type Result<T> = Result<T, Error>;
pub fn open<P: AsRef<Path>> (path: P) - > Result<File>
```

可以看到 Result可以被通过模式匹配到不同的处理逻辑，同时Error可以被不用层次地展开