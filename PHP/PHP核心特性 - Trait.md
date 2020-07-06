原文链接https://learnku.com/articles/35908

## trait的作用

解决**PHP不支持多级继承**的问题,

## trait的好处

组合优于继承   

以组合的方式 结合代码    降低对象之间的耦合度 



为什么 PHP 会引入 Trait ? 我们先来看看软件开发中的两种常用代码复用模式，继承和组合。

**继承：强调 父类与子类 的关系，即子类是父类的一个特殊类型；**
**组合：强调 整体与局部 的关系，侧重的一种需要的关系；**
软件开发中有一条原则，叫做组合优于继承。

继承关系中，子类与父类保持着高度的依赖关系，加上 PHP 不支持多继承，为了避免重写编写代码，很多功能都被统一封装到父类中。

这样做有两个坏处：

一是随着继承的层数和子类的增加，代码复杂度不断增加，大量的方法都将面临着重写；

二是这些功能对于一些子类来说可能是不必要的，破坏了代码的封装性。

Trait 的提出弥补了 PHP 对组合支持的不足，一个 Trait 就相当于一个模块，不同的 Trait 以组合的方式注入到类中。

以插拔式的方式, 组合在一起.

 

`trait` 中可包含属性、方法 与 抽象方法，这三者的结合既可以复用代码，也可以对代码的使用作出一些约定

Trait 中也可以包括静态属性和静态方法，以下是一个单例模式的简单封装。

trait Singleton
{
    private static $instance;

    public static function getInstance() {
        if (!(self::$instance instanceof self)) {
            self::$instance = new self;
        }
        return self::$instance;
    }
}
每个 Trait 中可以包含其他 Trait，进一步提高了代码的灵活性

<?php
trait Hello
{
    function sayHello() {
        echo "Hello";
    }
}

trait World
{
    function sayWorld() {
        echo "World";
    }
}

class MyWorld
{
    use Hello;
    use World;
}

————————————————
原文作者：心智极客
转自链接：https://learnku.com/articles/35908
版权声明：著作权归作者所有。商业转载请联系作者获得授权，非商业转载请保留以上作者信息和原文链接。