用于方便的表示和储存键值对，
属于`集合类型`的一种，提供于标准库中，但需手动引用：
```rust
use std::collections::HashMap;
```
类型为`HashMap<K:&str, V：i32>`  

-----
## 创建  
方法近似与其他的复合类型等：
```rust
let hashmap = HashMap::new();
```
## 插入
使用`insert()`来在末尾插入一个新的键值对
```rust
hashmap.inset(String::from("Blue"), 32);
```
>如果提前知道`HashMap`的存储数量，可以使用`HashMap::with_capacity(capacity)`来指定大小，这可提高性能。
### 批量插入
可以通过遍历循环的方式，但在rust中更好的方式是使用迭代器：
```rust
let team_list = vec![
("中国".to_string, 100),
("美国".to_string, 50),
("法国".to_string, 20),];
let mut team_map:HashMap<_,_> = team_list.into_inter().collect();
```

