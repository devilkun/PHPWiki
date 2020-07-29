# Redis内存锁

## 使用场景

1.抢红包 [并发]

2.商品/优惠券秒杀[并发]

3.热点Key过期[并发]

4.某个行为 在指定时间内 只允许操作一次



## 可选命令

### INCR

> key 不存在，那么 key 的值会先被初始化为 0 ，然后再执行 INCR 操作进行加一。
> 然后其它用户在执行 INCR 操作进行加一时，如果返回的数大于 1 ，说明这个锁正在被使用当中。

### SETNX

> 这种加锁的思路是，如果 key 不存在，将 key 设置为 value
> 如果 key 已存在，则 `SETNX` 不做任何动作

### SET

> 上面两种方法都有一个问题，会发现，都需要设置 key 过期。
> 那么为什么要设置key过期呢？如果请求执行因为某些原因意外退出了，
> 导致创建了锁但是没有删除锁，那么这个锁将一直存在，以至于以后缓存再也得不到更新。
> 于是乎我们需要给锁加一个过期时间以防不测。
> 但是借助 Expire 来设置就不是原子性操作了。
> 官方就引用了另外一个，使用 `SET` 命令本身已经从版本 2.6.12 开始包含了设置过期时间的功能。
>
> 具体请看https://redis.io/commands/set
>
> The [SET](https://redis.io/commands/set) command supports a set of options that modify its behavior:
>
> - `EX` *seconds* -- Set the specified expire time, in seconds.
> - `PX` *milliseconds* -- Set the specified expire time, in milliseconds.
> - `NX` -- Only set the key if it does not already exist.
> - `XX` -- Only set the key if it already exist.
> - `KEEPTTL` -- Retain the time to live associated with the key.

## **其它问题**

虽然上面一步已经满足了我们的需求，但是还是要考虑其它问题？
1、 第一次锁失败了要怎么办？中断请求还是再次循环请求？
2、 循环请求的话，如果有一个获取了锁，其它的在去获取锁的时候，是不是容易发生抢锁的可能？
3、 锁提前过期后，客户端A还没执行完，然后客户端B获取到了锁，这时候客户端A执行完了，会不会在删锁的时候把B的锁给删掉？

## **解决办法**

针对问题1：增加重试机制 ,使用循环请求去获取锁   
针对问题2：针对第二个问题，在循环请求获取锁的时候，加入睡眠功能，等待几毫秒在执行循环
针对问题3：在加锁的时候存入的key是随机的。这样的话，每次在删除key的时候判断下存入的key里的value和自己存的是否一样



## 实现细节

```
<?php

namespace App\Sdks\Library\Helpers\MemoryLock;


use App\Sdks\Library\Helpers\DiHelper;
use App\Sdks\Library\Helpers\LogHelper;

/**
 * redis内存锁适配器
 */
class LockManager
{

    /**
     * 获取内存锁key
     *
     * @param string $lockKey
     * @return string
     */
    protected static function getLockKey(string $lockKey)
    {
        return LockConfig::getConfig()['lockPrefix'] . $lockKey;
    }

    /**
     * 获取内存锁
     *
     * @param string $lockKey
     * @param $value
     * @return mixed
     */
    public static function lock(string $lockKey, $value)
    {
        $lockConfig = LockConfig::getConfig();
        $lockKey = self::getLockKey($lockKey);
        LogHelper::debug('redis_lock-key:',$lockKey);
        $redis = DiHelper::getRedis();
        $i = 0;
        do {
            $isLock = $redis->set($lockKey, $value, ['nx', 'ex' => $lockConfig['lockTimeout']]);

            //如果第一次没有获取到锁,等待指定时间[毫秒]后重试
            if($isLock == false ){
                usleep($lockConfig['lockTimeWait']);
                $i++;
                //超过重试次数,依然拿不到锁,退出
                if ($i > $lockConfig['lockRetryCount']) {
                    return false;
                }
            }
        } while ($isLock == false);
        return $isLock;
    }

    /**
     * 释放内存锁
     *
     * @param string $lockKey
     * @param $value
     * @return bool
     */
    public static function unlock(string $lockKey, $value)
    {
        $lockKey = self::getLockKey($lockKey);
        $redis = DiHelper::getRedis();
        //判断value是否是自己的,防止误删除其他进程的锁
        if ($redis->get($lockKey) == $value) {
            LogHelper::debug('redis_lock-delKey:',$lockKey);
            $redis->del($lockKey);
        }
        return true;
    }
}

```

## 使用示例

```
 		//用内存锁解决并发上麦问题
        //获取锁
        $key   = $roomId.'microId'.$microId;
        $value = time()+rand(1,1000);
        $res = LockManager::lock($key,$value);

        LogHelper::debug('redis_lock',$res);
        if($res == false){
            LogHelper::debug('redis_lock','redis_lock_fail');
            ErrorHandle::throwErr(Err::create(CoreLogic::MICRO_SOMEBODY_ELSE_ERROR));
        }
        
        //业务逻辑代码
        
         //释放锁
        LockManager::unlock($key,$value);
```

