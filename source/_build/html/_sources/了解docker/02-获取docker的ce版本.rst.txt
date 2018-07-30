获取docker的ce版本
========================================

OS需求
------------------------------------------

- 确保你启用的标准centos-extra仓库。
- overlay2存储驱动推荐使用

卸载老版本的docker
------------------------------------------

.. code-block:: bash

    $ sudo yum remove docker \
                    docker-client \
                    docker-client-latest \
                    docker-common \
                    docker-latest \
                    docker-latest-logrotate \
                    docker-logrotate \
                    docker-selinux \
                    docker-engine-selinux \
                    docker-engine

.. note:: docker 社区版本的包名字为docker-ce,使用yum remove docker-ce完成卸载。



设置按照需要的仓库
------------------------------------------

.. code-block:: bash 

    $ sudo yum install -y yum-utils \
    device-mapper-persistent-data \
    lvm2
    $ sudo yum-config-manager \
        --add-repo \
        https://download.docker.com/linux/centos/docker-ce.repo

    $ sudo yum-config-manager --enable docker-ce-edge
    $ sudo yum-config-manager --enable docker-ce-test

安装docker
------------------------------------------

.. code-block:: bash

    # 按照默认版本
    $ sudo yum install docker-ce

    # 按照指定版本
    $ yum list docker-ce --showduplicates | sort -r
    $ sudo yum install docker-ce-<VERSION STRING>

    # 启动docker服务
    $ sudo systemctl start docker

测试docker
----------------------------------------------

.. code-block:: bash 

    # 运行一个hello world
    $ sudo docker run hello-world
