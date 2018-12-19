docker info 
==============================================

docker info  显示docker系统范围的信息。

用法

.. code-block:: bash 

    docker info [OPTIONS]

样例

.. code-block:: bash 

   docker info --format '{{json .}}'