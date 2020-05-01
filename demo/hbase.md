# HBase Demo

- 执行start-hbase.sh，并输入yes，启动Hbase
![Start Hbase](../images/hbase-start.png "启动Hbase")

- 输入hbase shell 命令进入Hbase命令行界面
![Hbase Shell](../images/hbase-shell.png "进入Hbase Shell")

- 输入如下命令创建表

    ```mysql
    create 'students','ID','Description','Course','Home'
    ```
![Hbase Create Table](../images/hbase-create.png "创建表")

- 输入如下命令插入数据

    ```mysql
    put 'students','001','Description:Name','Li Lei'
    put 'students','001','Description:Height','176'
    put 'students','001','Course:Chinese','80'
    ```
![Hbase Put Data](../images/hbase-put.png "插入数据")

- 输入如下命令查看描述表信息

    ```mysql
    describe 'students'
    ```
![Hbase Describe Table](../images/hbase-describe.png "查看表信息")

- 执行stop-hbase.sh，退出Hbase
![Stop Hbase](../images/hbase-stop.png "退出Hbase")
