# Programming

#### 编写和运行MapReduce/Spark程序

### 在Vscode中编写和运行程序

1. 点击图标访问Master节点的Vscode容器。
![Homepage](../images/open-vscode.png "打开Vscode")


2. 在workspace中可以看到共享区中的数据集和从Git仓库中clone的代码。
![Homepage](../images/vscode-ws.png "Vscode Workspace")

3. 在Vscode Terminal中将数据集上传至HDFS并执行程序。
![Homepage](../images/vscode-terminal.png "Hadoop操作")

4. 程序执行成功后将结果文件从HDFS下载至workspace。
![Homepage](../images/vscode-result.png "查看执行结果")

5. 在“工作区—监控”页面可以监控集群状态，查看任务日志等。
![Homepage](../images/yarn-rm.png "查看任务执行情况")


### 在终端编写和运行程序

1. 点击图标访问集群Master节点的终端。
![Homepage](../images/open-ssh.png "打开SSH终端")

2. 用vi编写程序
![Homepage](../images/ssh-vi.png "vi编辑代码")

3. 运行程序
![Homepage](../images/ssh-execution.png "终端执行程序")

4. 查看结果
![Homepage](../images/ssh-cat.png "查看运行结果")


### 在Jupyter Notebook中编写和运行程序

1. 访问Jupyter Notebook的端口，默认密码123456
![Homepage](../images/open-jupyter.png "打开Jupyter Notebook")
![Homepage](../images/jupyter1.png "Jupyter Notebook首页")

2. 编写程序
![Homepage](../images/jupyter2.png "在Jupyter Notebook中编写Spark程序")

3. 新建Terminal
![Homepage](../images/jupyter3.png "新建终端")

4. 执行Spark程序
![Homepage](../images/jupyter5.png "执行Spark程序")

5. 查看程序执行结果
![Homepage](../images/jupyter6.png "Spark程序执行结束")

6. 关闭Terminal
![Homepage](../images/jupyter4.png "关闭终端")
