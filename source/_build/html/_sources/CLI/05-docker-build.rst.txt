docker build 
=======================

docker build 从dockerfile构建镜像。 

用法
-------------------

.. code-block:: bash 

    docker build [OPTIONS] PATH | URL | -

其中url可以是git仓库和tar和纯文本文件。


样例
-----------------------------

.. code-block:: bash 

    docker build https://github.com/docker/rootfs.git#container:docker
    docker build . 
    docker build - < Dockerfile
    docker build -f ctx/Dockerfile http://server/ctx.tar.gz

