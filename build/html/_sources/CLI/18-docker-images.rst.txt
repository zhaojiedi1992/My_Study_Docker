docker images 
==============================================

docker images   列出镜像列表

用法

.. code-block:: bash 

    docker images [OPTIONS] [REPOSITORY[:TAG]]


样例

.. code-block:: bash 

    docker history --format "{{.ID}}: {{.CreatedSince}}" busybox
    docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
    