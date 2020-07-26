
## 问题简述:
```
每天一次出现 2006 MySQL server has gone away的错误日志


```

## 发生时间:
2020-07-21

## 相关项目
LiveServer 

## 问题现象:

详细日志

```
LiveServer通知
"\nError Code: HY000\nSQLSTATE[HY000]: General error: 2006 MySQL server has gone away\n#0 [internal function]: 
PDOStatement->execute()\n#1 [internal function]: 
Phalcon\\Db\\Adapter\\Pdo->executePrepared(Object(PDOStatement), Array, Array)\n#2 
[internal function]: Phalcon\\Db\\Adapter\\Pdo->query('SELECT COUNT(*)...', Array, Array)\n#3 
[internal function]: Phalcon\\Db\\Adapter->fetchOne('SELECT COUNT(*)...', NULL, Array, Array)\n#4 [internal function]: 
Phalcon\\Mvc\\Model->_exists(Object(Phalcon\\Mvc\\Model\\MetaData\\Memory), 
Object(Phalcon\\Db\\Adapter\\Pdo\\Mysql), 'log_queue_task')\n#5 
\/data\/web\/liveserver\/app\/sdks\/Services\/Base\/QueueService.php(84): 
Phalcon\\Mvc\\Model->save(Array)\n#6 \/data\/web\/liveserver\/app\/tasks\/Base\/QueueTaskBase.php(69): App\\Sdks\\Services\\Base\\QueueService::saveFailedTask(Array)\n#7 
\/data\/web\/liveserver\/app\/tasks\/Queue\/JobTask.php(17): 
App\\Tasks\\Base\\QueueTaskBase->run('LiveServerTask')\n#8 [internal function]: 
App\\Tasks\\Queue\\JobTask->execAction(Array, Array)\n#9 [internal function]: 
Phalcon\\Cli\\Dispatcher->callActionMethod(Object(App\\Tasks\\Queue\\JobTask), 
'execAction', Array)\'\/usr\/local\/php\/bin:\/usr\/local\/openresty\/nginx\/sbin:
\/usr\/local\/mysql\/bin:\/usr\/local\/sbin:\/usr\/local\/bin:\/usr\/sbin:
\/usr\/bin:\/usr\/local\/openresty\/bin:\/usr\/local\/go\/bin:\/root\/bin',\n 
'HOME' => '\/root',\n  
'LANG' => 'en_US.UTF-8',\n  
'TERM' => 'xterm',\n  
'SHELL' => '\/bin\/bash',\n  
'SHLVL' => '1',\n  
'SUPERVISOR_ENABLED' => '1',\n  
'HISTSIZE' => '10000',\n  
'XDG_RUNTIME_DIR' => '\/run\/user\/0',\n  
'SUPERVISOR_PROCESS_NAME' => '00',\n  
'SUPERVISOR_SERVER_URL' => 'unix:\/\/\/var\/run\/supervisor.sock',\n  
'XDG_SESSION_ID' => '7435',\n 
'_' => '\/usr\/bin\/python',\n  'LS_COLORS' => \n 
'MAIL' => '\/var\/spool\/mail\/root',\n  
'SSH_CONNECTION' => '1.202.240.226 9690 172.26.228.9 22',\n  
'SUPERVISOR_GROUP_NAME' => 'LiveServerJob',\n  
'PHP_SELF' => '\/data\/web\/liveserver\/app\/tasks\/cli.php',\n  
'SCRIPT_NAME' => '\/data\/web\/liveserver\/app\/tasks\/cli.php',\n  
'SCRIPT_FILENAME' => '\/data\/web\/liveserver\/app\/tasks\/cli.php',\n  
'PATH_TRANSLATED' => '\/data\/web\/liveserver\/app\/tasks\/cli.php',\n 
'DOCUMENT_ROOT' => '',\n  
'REQUEST_TIME_FLOAT' => 1595160154.7932,\n  
'REQUEST_TIME' => 1595160154,\n  
'argv' => \n  array (\n    
0 => '\/data\/web\/liveserver\/app\/tasks\/cli.php',\n    
1 => 'queue.job',\n    
2 => 'exec',\n  ),\n  
'argc' => 3,\n)\n"
```



## 问题原因:

phalcon的离线脚本`/data/web/liveserver/app/tasks/cli.php`与mysql使用的是长连接.     离线脚本默认是永不退出的

长连接很久没有新的请求发起，达到了server端的timeout，被server强行关闭。

此后再通过这个connection发起查询的时候，就会报错server has gone away

## 

## 解决方案:
> 最终的解决方案  方案可能不止一个,沿途的风景才是前进路上的乐趣. 

### 方案1:修改数据库配置的超时时间

> "show global variables like '%timeout';"

### 方案2:检查 MySQL的链接状态，使其重新链接

有mysql_ping这么一个函数，在很多资料中都说这个mysql_ping的 API会检查数据库是否链接，如果是断开的话会尝试重新连接，但在我的测试过程中发现事实并不是这样子的，是有条件的，必须要通过 mysql_options这个C API传递相关参数，让MYSQL有断开自动链接的选项（MySQL默认为不自动连接），但我测试中发现PHP的MySQL的API中并不带这个函数，你重新编辑MySQL吧，呵呵。但mysql_ping这个函数还是终于能用得上的，只是要在其中有一个小小的操作技巧： 

```
function ping(){ 
	if(!mysql_ping($this->link)){ 
		mysql_close($this->link); //注意：一定要先执行数据库关闭，这是关键 
		$this->connect(); 
    } 
} 
```

ping()这个函数先检测数据连接是否正常，如果被关闭，整个把当前脚本的MYSQL实例关闭，再重新连接。 

经 过这样处理后，可以非常有效的解决MySQL server has gone away这样的问题，而且不会对系统造成额外的开销。