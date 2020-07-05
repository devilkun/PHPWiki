# 如何减少代码中的if/else

> if/else 只适合最多3层

## 1.三元运算符

```php
a ? b : c;

或者php7种的
a ?? b;
```

## 2.switch

```php
switch (expression){
case label1:
  expression = label1 时执行的代码 ;
  break;  
case label2:
  expression = label2 时执行的代码 ;
  break;
default:
  表达式的值不等于 label1 及 label2 时执行的代码;
}
```



## 4.映射表map

```php
旧代码
$typeName;
if($type == 1){
	$typeName = '类型1';
}else if($type == 2){
	$typeName = '类型2';
}else{
	$typeName = '类型3';
}


新代码
定义一个Map
$typeNameMap = [
	1 => '类型1',
	2 => '类型1',
	3 => '类型1',
]
$typeName = $typeNameMap[$type];
```



## 4.接口与实现类interface/implement

如果你的case可能会增加，那就这么做。

4.1 提前写一个map，key就是switch的 case值，value就是对应的实体类

4.2 抽象出通用方法，变成一个接口，统一入参和返回值

4.3主实现类（controller）就是把type穿进去，

**用反射获取到对应的实现类，然后调用抽象出来的方法**，

这样无论增加多少种case，都不会改主逻辑代码

###  4.4 每个类单独实现接口，互不影响

特别适合短信，邮件，支付， 存储等可能支持多个第三方服务，可能会切服务的场景

这其实是“**可变和不可变**”分离的原则。

简单说就是当switch中的条件发生变化时，能否不改变controller里的主逻辑代码，而具备扩展能力。

还有就是当case种的代码变化的情况下，能否将影响到的部分隔离，也就是我们说的“**解藕**”。

通常做到把初始化type和实现类放到配置文件里就够了。



## 5.策略模式



## 6.状态模式



## 7.责任链模式