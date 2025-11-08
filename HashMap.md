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
assert_eq!( num, Some(&23) )
```
- 注意到， `get()`方法会返回一个`Option<&i32>`类型，此处的借用是必要的，不然可能会引起所有权的转移，这会造成一些不必要的问题
- 同样，`get()`方法的参数是一个借用，这主要是`get()`方法的签名原因：
```rust
impl<K, V> HashMap<K, V>
where
    K: Eq + Hash,
{
    pub fn get<Q>(&self, k: &Q) -> Option<&V>
    where
        K: Borrow<Q>,
        Q: Hash + Eq + ?Sized,
    { ... }
}
```
>哇，我只是想查询一下`V`的值而已，怎么还搞了个`Option`类型？
>毕竟我们`Rust`可是安全语言，万一没查到呢？这时`Option`类型的好处就来了，有就返回`Some(&i32)`, 没有就返回`None
>但解包取值还要多写一行，程序猿都是懒的，有没有一次取到值的写法？
>有的，有的
>`let num:i32 = hashmap.get("blue".to_string).copied().unwrap_or(0);`
>这里， 先用`copied()`方法复制，将`&V`变为`V`，后用`unwrap_or()`方法进行错误处理，在`cpied()`方法失败时默认返回`0`
## 更新
有没有想过，若`insert()`方法的`K`值之前已经插入过了，会发生什么？
真如你所想，会更新原有键对

[^1]: Rust方便用户使用所编写所搞的自动预加载库，是不是很贴心😉？
