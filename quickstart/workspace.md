# Workspace

### 工作区约定
- 每个用户只能创建一个以用户名为前缀的集群。当用户名下无集群时，可在“工作区—概况”页面创建集群。当用户已有集群时，在“工作区—概况”页面显示集群基本信息。

### 新建集群

1. 系统提供了多个不同配置的镜像供用户选择。
![Homepage](../images/homepage.png "主页面")

2. 用户选择其中一个镜像。
![Select Image](../images/image-sel.png "选择镜像")

3. 在对话框中对集群进行配置。slave节点数、Git仓库地址、数据集均为必填项。数据集可从共享区的数据集中选择。
![Image Configuration](../images/cluster-config.png "配置集群")

4. 创建后页面显示集群基本情况。集群Status为“等待中”表示正在创建，创建成功后Status为“就绪”（<font color="red">需要主动刷新</font>）。
![Cluster Waiting](../images/cluster-waiting.png "集群正在创建")
![Cluster Ready](../images/cluster-ready.png "集群创建成功")

### 集群概况

1. 集群创建成功后，概况页面由两个卡片组成。上面为集群的基本信息和对集群的基本操作。卡片下部的图标代表的三个操作分别为：访问Vscode、访问SSH终端、删除集群。
![Cluster Overview](../images/cluster-overview.png "集群概况和基本操作")

2. 下面的卡片显示集群的端口映射信息，点击<font color="blue">“访问”</font>可以在新的窗口中访问各个端口对应的Web UI，包括Hadoop/Spark Web UI、Vscode、Jupyter Notebook等。
![Cluster Operation](../images/cluster-ports.png "可访问端口列表")

### 集群操作

1. 打开Vscode
![Open Vscode](../images/vscode.png "Vscode Server")

2. 打开SSH终端
![Open Terminal](../images/terminal.png "Web SSH终端")

3. 删除集群
![Delete Cluster](../images/cluster-del.png "删除集群")

4. 删除成功后回到主界面，可以再次选择镜像，新建集群
![Homepage](../images/homepage.png "主页面")

