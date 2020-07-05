# PHP函数式编程

**lambda表达式  更通俗一些的叫法是匿名函数。**

## 意义

> 1.通过函数的命名,就能知道代码实现了什么样的功能，而不用去琢磨代码具体细节来理解代码的作用
>
> 2.代码的可读性更高
>
> 3.用更少的代码  完成同样的功能  编码效率更高

## 作用

> 在处理列表数组方面
>
> 可以完全替换掉for、foreach、while这些循环控制语句，
>
>  这也是函数式编程方式在PHP的一部份体现。

## array_filter() 用回调函数过滤数组中的单元

> 官网地址 http://php.p2hp.com/manual/zh/function.array-filter.php

### 语法

```
array_filter ( array $array [, callable $callback [, int $flag = 0 ]] ) : array

依次将 array 数组中的每个值传递到 callback 函数。
如果 callback 函数返回 true，则 array 数组的当前值,会被包含在返回的结果数组中。
数组的键名保留不变。
```

### 参数

```
array 要循环的数组

callback 使用的回调函数
如果没有提供 callback 函数， 将删除 array 中所有等值为 FALSE 的条目。

flag 决定callback接收的参数形式:
ARRAY_FILTER_USE_KEY - callback接受键名作为的唯一参数
ARRAY_FILTER_USE_BOTH - callback同时接受键名和键值
```

### 示例

**说明: 过滤出数组中  所有性别为男的数据**

数据

```php
$data = [
	["id"=>1,"name"=>"张三","gender"=>男],
	["id"=>2,"name"=>"李四","gender"=>男],
	["id"=>3,"name"=>"王五","gender"=>女],
	["id"=>4,"name"=>"赵六","gender"=>女],
];
```



旧代码

```php
$manUserList = [];
foreach($data as $value){
	if($value['gender'] == '男'){
		$manUserList[] = $value;
	}
}
```

新代码

```php
$manUserList = array_filter($data,function($value){
    return $value['gender'] == '男';
})
```



## array_map() 为数组的每个元素应用回调函数

> map=映射
>
> http://php.p2hp.com/manual/zh/function.array-map.php

## 

```php

```



## array_reduce()用回调函数迭代地将数组简化为单一的值

> http://php.p2hp.com/manual/zh/function.array-reduce.php

```

```



## array_walk() 使用用户自定义函数,对数组中的每个元素做回调处理

> http://php.p2hp.com/manual/zh/function.array-walk.php

**array_walk是for或foreach语句的替代函数**

```

```

