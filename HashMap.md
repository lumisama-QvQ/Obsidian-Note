用于方便的表示和储存键值对，
属于`集合类型`的一种，提供于标准库中，但并没有包含于`prelude`[^1]中,需手动引用：
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
>**注意**:此处的team_map 必须标注类型，否则编译不通过，这是因为`collect()`方法支持多种集合类型的生成。

## 所有权
与其他的集合类型类似，所以要特别注意包含`clone`和`borrow`特征的类型
- `insert(K, V)`方法会将`K`,`V`的所有权移入`HashMap`中
- 类型若实现了`copy`特征， 则会复制入`HashMap`中， 所以无所谓所有权
## 查询
我们可以通过`get()`方法方便的获取元素
```rust
let hashmap = HashMap::new();
hashmap.insert("Blue".to_string, 23);
let num = hashmap.get(&"blue".to_string);
assert_eq!(num, Option(
	Some(&23)
	)
)
```
- 注意到， `get()`方法会返回一个`Option<&i32>`类型，此处的借用是必要的，不然可能会引起所有权的转移，这会造成一些不必要的问题
- 同样，`get()`方法的参数

[^1]: Rust方便用户使用所编写所搞的自动预加载库，是不是很贴心😉？
