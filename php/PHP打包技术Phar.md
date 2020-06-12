## 作用

把多个PHP文件打包成一个phar文件  类似Java语言的jar包.

PHP5.3版本 开始添加的特性

不需要安装其他扩展  属于PHP自带功能

PHAR文件默认状态是只读的，因为我们现在需要创建一个自己的Phar文件，所以需要允许写入Phar文件，这需要修改一下 `php.ini`

```
phar.readonly = 0
```