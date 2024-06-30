#### **php7新特征**

##### **底层优化**

######                 ● **变量引用**

zval 结构体去除了 refcount 和 is_ref，不再单独保存引用计数，引入了一个新的结构体 zend_refcounted 来记录变量的引用和被赋值次数，提升变量赋值和拷贝的性能；

######                 ● **占用字节**

zval占用字节减少，由php5的48字节减少到16字节，减小内存占用；

######                 ● **存储结构**

php5的数组存储使用**双向链表**指针，非连续存储；php7使用**连续存储空间存数组**，提高命中率；

##### **强类型**

##### **匿名类**

无类名，只用一次，如Laravel的migration类

```php
new class(){
    public function __construct($prop)
    {
    $this->prop3 = $prop;
    }
}
```

##### **??及?:**

##### **太空船比较符**

##### **php箭头函数(7.4新增)**

简易匿名函数

```php
$y = 1;

$fn1 = fn($x) => $x + $y;
// 相当于通过 value 使用 $y：
$fn2 = function ($x) use ($y) {
    return $x + $y;
};

var_export($fn1(3)); // echo 4

fn2 = fn($x) => fn($z) => $x + $z;
var_export($fn2(3)(1)) //echo 4
?>
```

##### **字符串内部实现**

...

#### **php8新特征**

##### **引入JIT编译器**

传统PHP是使用解释器将源代码逐行解析执行；开启后，将源代码编译为机器代码后再执行，提高执行速度

##### **底层数据结构及算法**

######                 ● **新的Hash表内部实现**

######                 ● **Zval2.0减少内存占用**

##### **match**

不同于switch，match使用严格比较，区分数字和字符串

```php
$return_value = match($food) {
    'a','b' => 'This food is an apple',
    'c' => 'This food is a bar',
    'd' => 'This food is a cake',
};
```

##### **字符串与数字0比较**

```php
0 == 'foobar' // php7 true php8 false
```

##### **联合类型**

```php
public function foo(Foo|Bar $input): int|float;
```

##### **字符串函数**

只看字符串是否包含另一字符串时，可不使用`strpos`，用新函数`str_contains`，检测字符串出现位置

```php
str_contains();
str_starts_with();
str_ends_with();
```

##### **原生注解**

```php
#[Attribute]
// 通过反射类获取注解
```

##### **命名参数**

```php
// PHP 8引入了命名参数作为现有位置参数的扩展。命名参数允许根据参数名而不是参数位置向函数传参，参数有默认值时，可跳过
function func($a,$b){}

func(a:1,b:2);
```

#### **php8.1新特征**
1.enum 枚举类
2.允许并发执行代码
3.readonly 只读类，里面的属性只初始化一次，属性不可再改变
4.final 类常量，不可被子类覆盖或更改
5.array_is_list($arr) 判断数组是否是索引从0开始的连续索引
6.... 支持非字符串索引数组

#### **php8.2新特征**
1.trait内可放常量
2.不再允许外部设置对象中不存在的属性

#### php8.3新特征

json验证

