## 作用



## 安装

```shell
yum install jq
```





## 命令示例

### 以json格式查看日志

```shell
tail -f debug.log | jq
```

### 取Json中的某个字段

```shell
# 取json中第一个分片 中的name
cat ./json.txt | jq '.[0].name'
"zhangsan"
# 取courses 中第一个下标的课程
cat ./json.txt | jq '.[].courses[0]'
"语文"
"物理"
```

### 对`json`重新进行整理

```shell
# 只输出name 和 age
cat ./json.txt | jq '[.[] | {name:.name, age:.age}]'
[
  {
    "name": "zhangsan",
    "age": 21
  },
  {
    "name": "lisi",
    "age": 22
  }
]
# 输出name和第一个course
cat ./json.txt | jq '[.[] | {name:.name, course:.courses[0]}]'
[
  {
    "name": "zhangsan",
    "course": "语文"
  },
  {
    "name": "lisi",
    "course": "物理"
  }
]
```

