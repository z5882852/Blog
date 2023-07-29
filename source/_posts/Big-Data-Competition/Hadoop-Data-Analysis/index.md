---
title: Hadoop数据分析
date: 2023-06-06 20:39:00
tags: [Hadoop,数据分析]
---

### MySQL
```bash
systemctl start mysqld.service # 启动mysql服务
systemctl stop mysqld.service # 停止mysql服务
systemctl restart mysqld.service # 重启mysql服务
systemctl status mysqld.service # 查看mysql服务状态
```

### Hadoop
```bash
start-all.sh # 启动Hadoop集群
stop-all.sh # 关闭Hadoop集群

# 以下为单模块启动
start-dfs.sh # 启动HDFS模块
stop-dfs.sh # 关闭HDFS模块

start-yarn.sh # 启动yarn集群
stop-yarn.sh # 关闭yarn集群
```
以下命令的区别
```bash
hadoop fs [options]
hadoop dfs [options]
hdfs dfs [options]
```
- `hadoop fs`：通用的文件系统命令，针对任何系统，比如本地文件、HDFS文件、HFTP文件、S3文件系统等。
- `hadoop dfs`：特定针对HDFS的文件系统的相关操作，但是已经不推荐使用。
- `hdfs dfs`：与hadoop dfs类似，同样是针对HDFS文件系统的操作，替代hadoop dfs

### HDFS
```bash
hdfs namenode -format # 格式化HDFS文件系统
```


### Hive
注: 伪集群的Hive通过Reduce操作时是真的慢，基本都是1分钟以上，使用能不用Hive就不用Hive。
```bash
schematool -dbType mysql -initSchema  # 初始化Hive元数据库

hive # 进入Hive

hive> create database <库名>;  # 创建Hive数据库

hive> use <库名>;  #进入数据库

hive> create table <表名>(id int,name string);  #创建数据表

hive> insert into <表名> values (1,"zhangsan");  #插入数据
```
以下例子为创建字段：id,title,price,views,sales,stock的表，并导入csv数据。
```bash
hive> create table goods(
    >     id int,
    >     title string,
    >     price double,
    >     views int,
    >     sales int,
    >     stock int
    > ) 
    > row format delimited fields terminated by ','; #创建数据表,指定了逗号作为列分隔符; 

hive> load data local inpath '/data/csv2.csv' into table goods;  # 导入csv文件数据

hive> load data local inpath '/data/goods.txt' into table goods;  # 导入csv文件数据
``` 
以下例子是通过Reduce查询价格(price)为空的记录并写入到某一路径。
```bash
hive> insert overwrite local directory '/root/'
    > row format delimited fields terminated by '\t'  # 数据分隔符'\t'
    > select * from goods where price is null;  # 查询语句
```
当然有更简单的方法。
```bash
hive -e "use shopxo;select * from goods where price is null;" > /root/shopxo/out.txt  # 在hive外执行
```
以下是将查询结果保存到表的语句。
```bash
hive> insert overwrite table goods1  # goods1 为表名称
    > select * 
    > from goods 
    > where title not like "%连衣裙%" 
    >   and title not like "%女士%" 
    >   and title is not null;

# 以下是直接创建新表的语句
hive> create table goods1 as # goods1 为创建的新表名称
    > select * 
    > from goods 
    > where title not like "%连衣裙%" 
    >   and title not like "%女士%"
    >   and title is not null;
```

### 一些问题
![图片无法加载](/Blog/src/01.png "图片")
其中第4题的空值null数据是值上一题的价格(price)为空值(null)而不是标题(title)为null