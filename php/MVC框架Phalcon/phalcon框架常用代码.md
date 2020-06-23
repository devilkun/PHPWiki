
## 抛出异常

```
//参数错误
ErrorHandle::throwErr(Err::create(CoreLogic::INVALID_PARAM, ['phone']));

//操作失败
ErrorHandle::throwErr(Err::create(CoreLogic::OPERATE_ERROR, ['phone']));

//重复操作
ErrorHandle::throwErr(Err::create(CoreLogic::REPEAT_SEND_ERROR, ['']));

//没有权限
ErrorHandle::throwErr(Err::create(CoreLogic::PERMISSION_ERROR, ['phone']));
```

## create_time/update_time

```
`create_time` datetime DEFAULT CURRENT_TIMESTAMP,
`update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
```

## 查询    使用参数绑定  杜绝SQL注入的问题

官方文档

https://www.kancloud.cn/jaya1992/phalcon_doc_zh/753278

不带注释说明的版本

```
public static function findList()
    {
        return self::find(
        	[
	        	'columns' => 'id,name',
            	"conditions" => "id > :id: AND is_delete = 0",
            	'bind' => [
                	"id" => 0
            	],
            	'order'      => "id desc",
			   'offset' => 5
            	"limit"	=> 20,

         	]
         )->toArray();
    }
```

带注释说明的版本

```
public static function findList()
    {
        return self::find(
        	[
        		//返回指定的字段
	        	'columns' => 'id,name',
        		//查询条件
            	"conditions" => "id > :id: AND is_delete = 0",
            	//参数绑定
            	'bind' => [
                	"id" => 0
            	],
            	//指定排序条件
            	'order'      => "id desc",
            	//将查询结果偏移一定量  配合limit做分页查询
            	'offset' => 5
            	//限制返回数据的条数
            	"limit"	=> 20,
            	
         	]
         )->toArray();
    }
```

另一种写法,面向对象的方式

没有哪种更好, 我跟习惯上面的写法.

```
$robots = Robots::query()
    ->where('type = :type:')
    ->andWhere('year < 2000')
    ->bind(['type' => 'mechanical'])
    ->order('name')
    ->execute();
```



## in查询

```
/**
 * in查询
 * @param $ids 数组
 * @return mixed
 */
public static function findInList($ids)
{
    return self::find(
        [
            'conditions' => ' id IN ({ids:array})',
            'bind'       => ['ids'=>$ids],
        ]
    );
}
```

## 悲观锁 的使用

**又称为  独占锁,排他锁**

使用此选项，`Phalcon\Mvc\Model` 将读取最新的可用数据，并在其读取的每一行上设置独占锁。

使用场景: 建议在只查询一条数据的情况下使用  

```
'for_update' => true
```

**使用样例**

```
public static function findList()
    {
        return self::findFirst(
        	[
        		//返回指定的字段
	        	'columns' => 'id,name',
        		//查询条件
            	"conditions" => "id > :id: AND is_delete = 0",
            	//参数绑定
            	'bind' => [
                	"id" => 0
            	],
            	//指定排序条件
            	'order'      => "id desc",
            	//将查询结果偏移一定量  配合limit做分页查询
            	'offset' => 5
            	//限制返回数据的条数
            	"limit"	=> 20,
            	
         	]
         )->toArray();
    }
```



## 共享锁的使用

使用此选项， `Phalcon\Mvc\Model` 将读取最新的可用数据，并在其读取的每一行上设置共享锁。

```
'shared_lock' => true
```





## 插入新数据后,获取最新的主键id

```
$newTag = new \App\Sdks\Models\TagModel();
$res = $newTag->create([
      'tag_name' => 'test1',
      'is_delete'=>0
       ]);
 //打印添加是否成功
 var_dump($res);
 //打印最新的主键id
 var_dump($newTag->tag_id);
```

## 操作Redis

```
$redis = DiHelper::getRedis();
$key   = sprintf(RedisKey::USER_RONGYUN_TOKEN, $uuid);
$redis->set($key,$register->token,RedisKey::expire(RedisKey::USER_RONGYUN_TOKEN));
```

## Rpc客户端调用示例

> 必须用try{ }catch{ }包起来
>
> 定一个rpc返回异常的code  

```
	try {
            $client = DiHelper::getRpcClient();
            $rpcRes = $client->getUserInfo('120017');
            if($rpcRes['code'] !== 0){
                ErrorHandle::throwErr(Err::create(CoreLogic::RPC_ERROR, [$rpcRes['code'].$rpcRes['msg']]));
            }
        } catch (Exception $e) {
            throw new JsonFmtException($e->getMessage(), $e->getCode());
        }
```

## 打印日志 支持数组格式

```
//打印字符串
LogHelper::debug("userServer-UserLogin","userId:12007");

//打印数组  后续会自动进行转换为json
LogHelper::debug("userServer-UserLogin",['userId'=>12007,'nickName'=> '天下独舞']);
```



## Redis内存锁的使用

```
$redis = DiHelper::getSharedRedis();
LockManager::init($redis);
//获取锁
LockManager::lock($key);

//释放锁
LockManager::unlock($key);
```

## MongoDB 操作

```
$roomMongoDao = new RoomMongoDao();

$roomWaitUserMongoDao = new RoomWaitUserMongoDao();

$userMongoDao = new UserMongoDao();
```



## 生成随机数 随机字符串

```
$random = new Random();

//生成一个0-N之间的随机数字
//可用于redis key加随机数,防止缓存雪崩问题
$number     = $random->number($n);

// ...
$bytes      = $random->bytes();

// 随机生成一个长度为$length的十六进制字符串
$hex        = $random->hex($length);

// 随机生成长度为$length的base64字符串
$base64     = $random->base64($length);

// 生产一个长度为$length的安全base64字符串.
$base64Safe = $random->base64Safe($length);

// 生成一个UUId
// See https://en.wikipedia.org/wiki/Universally_unique_identifier
$uuid       = $random->uuid();


```

















---



参考 https://segmentfault.com/a/1190000014166424#item-1-11

## 使用数据库事务

```
<?php

try {
	$db = DiHelper::getDB();
    // 开启一个事务 Start a transaction 
    $db->begin();

    // 执行一些SQL
    $connection->execute('DELETE `robots` WHERE `id` = 101');
    $connection->execute('DELETE `robots` WHERE `id` = 102');
    $connection->execute('DELETE `robots` WHERE `id` = 103');

    // 提交事务
    $db->commit();
} catch (Exception $e) {
    // 发生异常，回滚事务
    // 使用事务,必须捕获异常
    $db->rollback();
}
```



## 使用数据库 进行事务嵌套

**事务嵌套产生问题的原因:**

> 当执行一个START TRANSACTION指令时，会隐式的执行一个commit操作
>
> 翻译:开启一个新事务时,会自动提交上一个事务

**解决事务嵌套问题的关键在与  创建保存点**

> `InnoDB引擎`支持SQL语句
> `SAVEPOINT`  创建事务保存点  [也叫暂存点]
> `ROLLBACK TO SAVEPOINT` 回滚事务到指定的保存点
> `RELEASE SAVEPOINT` 删除事务的所有保存点
> `ROLLBACK`  回滚事务
>
> 一个事务支持设置多个保存点  每个保存点的名称不一样  如果名称一样则会新的覆盖旧的
>
> `ROLLBACK TO SAVEPOINT`语句将事务回滚到指定的保存点，而不会终止该事务
> 如果执行`COMMIT`或`ROLLBACK`事务,则将删除当前事务的所有保存点。

更高级的用法, phalcon提供了事务管理器Transaction Manager

```
try {
    //开启一个事务 Start a transaction
    $connection->begin();

    // 执行一些SQL
    $connection->execute('DELETE `robots` WHERE `id` = 101');

    try {
        // 开启一个嵌套事务
        $connection->begin();

        // 在嵌套事务中执行一些SQL
        $connection->execute('DELETE `robots` WHERE `id` = 102');
        $connection->execute('DELETE `robots` WHERE `id` = 103');

        // 创建一个保存点 Create a save point
        $connection->commit();
    } catch (Exception $e) {
        // 发生错误，回滚 嵌套事务
        // 使用事务,必须捕获异常
        $connection->rollback();
    }

    // 继续，执行更多SQL语句
    $connection->execute('DELETE `robots` WHERE `id` = 104');

    // 提交第一个事务
    $connection->commit();
} catch (Exception $e) {
    // 发生异常，回滚第一个事务
    // 使用事务,必须捕获异常
    $connection->rollback();
}
```

如果要在Phalcon中使用，记得override `Phalcon\Mvc\Model:: getSchema`

phalcon提供了事务管理器