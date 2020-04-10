# HBase Demo

1. 输入start-hbase.sh 命令并输入yes 启动Hbase
![Start Hbase](../images/start_hbase.png "启动Hbase")

2. 输入hbase shell 命令进入Hbase命令行界面
![Hbase Shell](../images/hbase_shell.png "进入Hbase Shell")

3. 输入如下命令创建表

    ```mysql
    create 'students','ID','Description','1 Course','Home'
    ```
![Hbase Create Table](../images/hbase_create.png "创建表")

4. 输入如下命令插入数据

    ```mysql
    put 'students','001','Description:Name','Li Lei'
    put 'students','001','Description:Height','176'
    put 'students','001','Course:Chinese','80'
    ```
![Hbase Put Data](../images/hbase_put.png "插入数据")

5. 输入如下命令查看描述表信息

    ```mysql
    describe 'students'
    ```
![Hbase Describe Table](../images/hbase_describe.png "查看表信息")
