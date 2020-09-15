



## debug_backtrace()

> 官方文档:http://php.p2hp.com/manual/zh/function.debug-backtrace.php

### 作用

>  产生一条回溯跟踪   
>
> 白话:打印出一个请求的所有调用过程
>
> 返回请求过程中的执行过的所有 类名  函数名  行号  等等信息
>
> 解决bug,调试的利器!

### 用法

```
debug_backtrace(DEBUG_BACKTRACE_IGNORE_ARGS,2)
第一个参数
第二个参数  表示追踪的层级
```

---

