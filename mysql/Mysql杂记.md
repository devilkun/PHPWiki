

## datetime 和 timestamp的效率对比

**MyISAM 引擎，不建立索引的情况下（推荐)**

> 效率从高到低：
int > UNIXTIMESTAMP(timestamp) > datetime（直接和时间比较）> timestamp的效率对比（直接和时间比较）> UNIXTIMESTAMP(datetime) 。

**对于 MyISAM 引擎，建立索引的情况下**
> 效率从高到低：
> UNIXTIMESTAMP(timestamp) > int > datetime（直接和时间比较）>timestamp（直接和时间比较）>UNIXTIMESTAMP(datetime) 。

**对于 InnoDB 引擎，没有索引的情况下(不建议)**

> 效率从高到低：
> int > UNIXTIMESTAMP(timestamp) > datetime（直接和时间比较） > timestamp（直接和时间比较）> UNIXTIMESTAMP(datetime)。

**对于 InnoDB 引擎，建立索引的情况下**
> 效率从高到低：
> int > datetime（直接和时间比较） > timestamp（直接和时间比较）> UNIXTIMESTAMP(timestamp) > UNIXTIMESTAMP(datetime)。

**一句话，对于 MyISAM 引擎，采用 UNIX_TIMESTAMP(timestamp) 比较；**
 **对于InnoDB 引擎，建立索引，采用 int 或 datetime直接时间比较**

------









