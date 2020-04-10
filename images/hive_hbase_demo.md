# Demo

## Hbase

1. 输入`start-hbase.sh`命令并输入`yes`启动Hbase

![start_hbase](start_hbase.png)

2. 输入`hbase shell`命令进入Hbase命令行界面

![hbase_shell](hbase_shell.png)

3. 输入如下命令创建表

```mysql
create 'students','ID','Description','Course','Home'
```

![hbase_create](hbase_create.png)

4. 输入如下命令插入数据

```mysql 
put 'students','001','Description:Name','Li Lei'
put 'students','001','Description:Height','176'
put 'students','001','Course:Chinese','80'
```

![hbase_put](hbase_put.png)

5. 输入如下命令查看描述表信息

```mysql
describe 'students'
```

![hbase_describe](hbase_describe.png)



## hive

1. 选择带有hive的镜像创建集群后进入terminal，输入`hive`进入Hive的命令行界面，输入`show tables;`显示OK表明Hive正常启动

![hive_shell](hive_shell.png)

2. 输入如下命令创建表

   ```mysql
   create table logs(
       user_id int,item_id int,cat_id int,mer_id int,brand_id int,month int,day int,action int,age_range int,gender int,province string)
       row format serde 'org.apache.hadoop.hive.serde2.OpenCSVSerde' 
       WITH SERDEPROPERTIES( "separatorChar"="," )  STORED AS TEXTFILE; 
   ```

![hive_create](hive_create.png)

3. 输入如下命令从csv文件导入到创建的表中

```mysql
load data local inpath '/workspace/dataset1/million_user_log.csv' overwrite into table logs; 
```

![hive_load_data](hive_load_data.png)

4. 输入如下命令测试导入成功：

```mysql
select count(*) from logs;
```

![hive_select_1](hive_select_1.png)

执行结果为1000000条

5. 查询记录：action为2的user数量，输入命令如下：

```mysql 
select count(distinct user_id) from logs where action=2;
```

![hive_select_2](hive_select_2.png)