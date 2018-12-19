docker  inspect
==============================================

docker inspect   获取docker对象的基础信息。

用法

.. code-block:: bash 

    docker inspect [OPTIONS] NAME|ID [NAME|ID...]

样例

.. code-block:: bash 

    docker inspect --format='{{json .Config}}' $INSTANCE_ID
    docker inspect --format='{{.Config.Image}}' $INSTANCE_I
