### 查看php-fpm进程数量

```
ps -ef |grep php-fpm |wc -l

ps -ef |grep "php-fpm"|grep "pool"|wc -l
```



### 查看PHP-FPM在你的机器上的平均内存占用

```
ps --no-headers -o "rss,cmd" -C php-fpm | awk '{ sum+=$1 } END { printf ("%d%s\n", sum/NR/1024,"M") }'
```

### 查看每个php-fpm占用的内存大小

```
ps -ylC php-fpm --sort:rss
```



### 查看单个php-fpm进程消耗内存的明细

```
pmap $(pgrep php-fpm) | less
```

