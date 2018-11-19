使用docker 命令行
====================

docker中的优先级
命令行指定 > 环境变量 > config.json 

获取帮助信息
---------------------

.. code-block:: bash 

    docker run --help 
    docker container ls --help 

.. csv-table::
   :header: "命令","描述"
   :widths: 15, 40

   "docker attach","将本地标准的输入输出和错误流附加到正在运行的容器上面"
   "docker bulid","从dockerfile构建镜像"
   "docker checkpoint","管理检查点"
   "docker commit","根据容器的更改创建新的镜像"
   "docker config","管理docker的配置"
   "docker container","管理容器"
   "docker cp","在容器和本地文件系统之间复制文件或者文件夹"
   "docker create ","创建一个新的容器"
   "dokcer deploy","部署或者更新"
   "docker diff","检查容器文件系统的文件或者目录的更改"
   "docker envents","获取服务器的实时事件"
   "docker exec","在正在运行的容器中运行命令"
   "docker export","将容器的文件系统导出为tar归档"
   "docker history","显示镜像的提交历史"
   "docker image","管理镜像"
   "docker images","列出镜像"
   "docker import ","从tar包导入内容创建新的镜像"
   "dokcer info","获取docker基本信息"
   "docker inspect","获取docker特定对象的详细信息"
   "docker kill","杀掉特定容器"
   "docker load ","从tar包加载镜像或者stdin加载"
   "docker login","登陆，默认是dockerhub"
   "docker logout","注销"
   "docker logs","获取容器的日志信息"
   "docker manifest","管理docker镜像清单"
   "docker network","管理docker网络"
   "docker node","管理swam节点"
   "docker pause","暂停容器"
   "docker port","管理端口"
   "docker ps ","列出容器"
   "docker pull","拉取镜像"
   "docker push","提交镜像"
   "docker rename","重命名"
   "docker restart ","重启容器"
   "docker rm","删除容器"
   "docker rmi","删除镜像"
   "docker run","运行容器"
   "dokcer save","保持容器"
   "docker search","从dockerhub搜索镜像"
   "docker service","管理服务"
   "docker stack","管理堆栈"
   "docker stats","获取统计信息"
   "docker stop","停止容器"
   "docke swarm","管理swarm"
   "docker system","管理docker"
   "docker tag","打tag信息"
   "docker top","获取top信息"
   "docker trust","管理对docker镜像的信任"
   "docker update","更新"
   "docker version","获取版本信息"
   "docker volume","管理卷"
   "docker wait","等待容器停止，然后打印退出码"
