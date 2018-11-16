macvlan网络
===================================

在某些应用程序，尤其是遗留应用程序或监事网络流量的应用程序，希望直接连接到物理网络，在这种情况下，您可以使用macvlan网络驱动程序为每个容器的虚拟网络接口分配
mac地址。

创建macvlan网络
--------------------------------------

.. code-block:: bash 

    docker network create -d macvlan \
    --subnet=172.16.86.0/24 \
    --gateway=172.16.86.1  \
    -- parent=eth0 pub_net 

