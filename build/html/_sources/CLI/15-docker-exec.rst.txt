docker exec  
==============================================

docker exec  在运行的容器中运行命令。 

用法

.. code-block:: bash 

    docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

主要选项

-t  分离模式
-e  设置环境变量
-i  交互
-t  分配tty 

.. note::  command应该是可执行文件，不能是带引号的命令。

