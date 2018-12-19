docker history  
==============================================

docker history   显示一个镜像的历史

用法

.. code-block:: bash 

    docker history [OPTIONS] IMAGE


样例

.. code-block:: bash 

    docker history --format "{{.ID}}: {{.CreatedSince}}" busybox