docker cp 
==============================

docker cp 提供容器和本地文件系统之间复制文件或者文件夹功能。

用法
-------------------------------

.. code-block:: bash 

    docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
    docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

cp的使用和unix的cp基本类似，-a保留权限。 

