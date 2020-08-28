## updateData更新函数

> 适用场景:一次更新一行数据    比如以主键id为条件进行更新

```
 /**
     * 调用示例
     * 更新单个字段
     * updateData(['count' => 2],['user_id' => 120017]);
     * 更新多个字段
     * updateData(['count' => 2,'money' => 3],['user_id' => 120017]);
     * //多条件更新
     * updateData(['count' => 2,'money' => 3],['user_id' => 120017,'type' => 1]);
     * @param array $fields
     * @param array $wheres
     */
    public function updateData(array $fields, array $wheres)
    {
        if (empty($wheres) || empty($fields) ) {
            throw new \RuntimeException("缺少更新条件");
        }
        $columns = [];
        foreach ($fields as $field => $number){
            $columns[] =  "`{$field}` =  {$number}";
        }
        $where = [];
        foreach ($wheres as $key => $value){
            $where[] =  "`{$key}` = '{$value}'";
        }
        $db = DiHelper::getDB();
        $sql = sprintf("UPDATE `%s` SET %s WHERE %s", $this->getSource(), join(',', $columns), join('AND', $where));
        LogHelper::debug("updateDataSQL",$sql);
        // todo 未使用参数绑定 待优化
        if (!$db->execute($sql, []) || $db->affectedRows() < 1) {
            //更新失败
            LogHelper::error("updateDataSQL",$sql);
            ErrorHandle::throwErr(Err::create(CoreLogic::OPERATE_ERROR, []));
        }
    }
```





## increment 更新指定的字段    +1

```
 /**
     * 调用示例
     * 更新单个字段
     * increment(['count' => 2],['user_id' => 120017]);
     * 更新多个字段
     * increment(['count' => 2,'money' => 3],['user_id' => 120017]);
     * //多条件更新
     * increment(['count' => 2,'money' => 3],['user_id' => 120017,'type' => 1]);
     * @param array $fields
     * @param array $wheres
     */
    public function increment(array $fields, array $wheres)
    {
        if (empty($wheres) || empty($fields) ) {
            throw new \RuntimeException("缺少更新条件");
        }
        $columns = [];
        foreach ($fields as $field => $number){
            $columns[] =  "`{$field}` = `{$field}` + {$number}";
        }
        $where = [];
        foreach ($wheres as $key => $value){
            $where[] =  "`{$key}` = '{$value}'";
        }
        $db = DiHelper::getDB();
        $sql = sprintf("UPDATE `%s` SET %s WHERE %s", $this->getSource(), join(',', $columns), join('AND', $where));
        LogHelper::debug("increment",$sql);
        // todo 未使用参数绑定 待优化
        if (!$db->execute($sql, []) || $db->affectedRows() < 1) {
            //更新失败
            ErrorHandle::throwErr(Err::create(CoreLogic::OPERATE_ERROR,  ["tableName:{$this->getSource()}".'increment+N']));
        }
    }

   
```

## decrement 更新指定的字段  -N  默认-1

```
 /**
     * 更新指定的字段  -N  默认-1
     * @param array $fields
     * @param array $wheres
     */
    public  function decrement(array $fields, array $wheres)
    {
        if (empty($wheres) || empty($fields) ) {
            throw new \RuntimeException("缺少更新条件");
        }
        $columns = [];
        foreach ($fields as $field => $number){
            $columns[] =  "`{$field}` = `{$field}` - {$number}";
        }
        $where = [];
        foreach ($wheres as $key => $value){
            $where[] =  "`{$key}` = '{$value}'";
        }
        $db = DiHelper::getDB();
        $sql = sprintf("UPDATE `%s` SET %s WHERE %s", $this->getSource(), join(',', $columns), join('AND', $where));
        LogHelper::debug('decrement',$sql);
        // todo 未使用参数绑定 待优化
        if (!$db->execute($sql, []) || $db->affectedRows() < 1) {
            //更新失败
            ErrorHandle::throwErr(Err::create(CoreLogic::OPERATE_ERROR, []));
        }
    }
```

## findFirstArray

```
public static function findFirstArray($parameters = null)
{
    $result  = parent::findFirst($parameters);
    return !empty($result) ? $result->toArray() : [];
}
```

## findArray

```
public static function findArray($parameters = null)
{
    $result  = parent::find($parameters);
    return !empty($result) ? $result->toArray() : [];
}
```