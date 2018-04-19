

> 基于Ambari安装大数据环境，你是不是已经厌倦一步步、按部就班的安装、等待？
> 
> 你是否憧憬于，一键傻瓜式地安装操作？
> 
> 现在：
>
> 你可以通过Ambari Blueprint功能，点击安装，然后慢悠悠地喝着你的咖啡...
>
> 等待...

## 初级篇

### 步骤1： 安装ambari-server和ambari-agent

安装ambari-server:
> dpkg -i ambari-server_2.5.1.0-0-dist.deb
> ambari-server setup --jdbc-db=mysql --jdbc-driver=/usr/share/java/mysql-connector-java.jar
> ambari-server setup
> ambari-server restart

安装ambari-agent:
>
> dpkg -i ambari-agent_2.5.1.0-0.deb
> ambari-agent reset hzadg-mammut-platform1.server.163.org
> ambari-agent restart


### 步骤2： 选择你需要安装的集群种类
选择需要安装的blueprint，在./blueprint路径下，提供了可以选择的安装模板。需要注意的是：
* cluster_blueprint.json: 定义安装服务的配置信息、机器同安装服务之间的映射信息； 
* cluster_mapping.json: 定义真是机器域名同机器映射之间的关系，默认提供的是3台机器安装所有服务组件；

> 并不是所有的安装模板都是通用的，需要根据实际情况修改特定的配置，此处需要注意；
> 同时，如果需要改变安装机器映射关系，可以修改cluster_mapping.json文件。
> 

### 步骤3： 执行开启部署

> curl -H "X-Requested-By: ambari" -X POST -u admin:admin http://${Ambari-Server}:8080/api/v1/blueprints/cluster_bluprint -d @cluster_blueprint.json
> 
> curl -H "X-Requested-By: ambari" -X POST -u admin:admin http://${Ambari-Server}:8080/api/v1/clusters/${new cluster name} -d @cluster2_mapping.json

示例：
> cd ./blueprints/cluster_with_9_services
>
> curl -H "X-Requested-By: ambari" -X POST -u admin:admin http://hzadg-mammut-platform1.server.163.org:8080/api/v1/blueprints/cluster_bluprint -d @cluster_blueprint.json
> 
> # 创建名为cluster1的新集群
> curl -H "X-Requested-By: ambari" -X POST -u admin:admin http://hzadg-mammut-platform1.server.163.org:8080/api/v1/clusters/cluster1 -d @cluster_mapping.json


## 进阶篇
### 自定义部署方案
* 给出的cluster_mapping.json部署机器默认只有3台，你可以添加或者缩减；
* 但要注意修改cluster_blueprint.json中的机器->服务的映射关系；


### 自定义Blueprint，后续部署使用
场景： 如果当前集群你修改了许多配置信息，你可以通过以下接口，将该集群打包成blueprint，保存以供后续自动化部署使用：

> curl -H "X-Requested-By: ambari" -X GET -u admin:admin http://hzadg-mammut-platform1.server.163.org:8080/api/v1/clusters/cluster1?format=blueprint
>
> 如果感兴趣，可以丰富本库，可以提MR。

注意事项：
* 注意保存的blueprint中切记避免固定host或者ip，尽量通过host_group_1这种变量代替；

### 其他blueprint相关接口
*  获取当前集群中所有的blueprints: http://hzadg-mammut-platform1.server.163.org:8080/api/v1/blueprints
