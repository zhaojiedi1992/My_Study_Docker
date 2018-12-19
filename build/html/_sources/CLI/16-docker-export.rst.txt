docker export  
==============================================

docker export   将容器的文件系统导出为tar归档。

用法

.. code-block:: bash 

    docker export [OPTIONS] CONTAINER

选项只有一个`-o`来指定数据的位置。 

样例

.. code-block:: bash 

    docker export red_panda > latest.tar
    docker export -o latest.tar red_panda