## 产生跨库事务的原因

> 数据库在同一个物理机器   同一个3306端口下
> 虽然创建了N个业务db,
> 在一个进程里 同时更新N个库,是没有跨库事务的问题

> 跨 不同物理DB时,使用事务  出现跨库事务问题.

## 解决方案原理

TCC 两阶段提交 来保证数据的最终一致性

TCC 是 Try Confirm Cancel的简写

Try: 尝试执行业务

```
完成所有业务检查（一致性）

预留必须业务资源（准隔离性）
```

Confirm: 确认执行业务

```
真正执行业务

不作任何业务检查

只使用Try阶段预留的业务资源

Confirm操作满足幂等性
```

Cancel: 取消执行业务

```
释放Try阶段预留的业务资源

Cancel操作满足幂等性
```

## 开源框架

### Java

https://github.com/changmingxie/tcc-transaction

> 真羡慕Java的生态,PHP目前没有TCC开源框架可供使用 需要自己实现

## 实现方案1  基于事务日志表 [Mysql]

t_transactionlog 事务日志表

```
public class TransactionLog {

    private Long gtid;
    private Long tid;
    private Long pid;
    private String name;
    //状态
    private int state;


    private String className;
    private String commitMethod;
    private String rollbackMethod;
    private String actionType;
}
```

事务阶段状态常量类

```
public enum TransactionState {

    TRYING(1), CONFIRMING(2), CANCELLING(3);

    private int id;

    TransactionState(int id) {
        this.id = id;
    }

    public int getId() {
        return id;
    }

    public static TransactionState valueOf(int id) {

        switch (id) {
            case 1:
                return TRYING;
            case 2:
                return CONFIRMING;
            default:
                return CANCELLING;
        }
    }

}
```



