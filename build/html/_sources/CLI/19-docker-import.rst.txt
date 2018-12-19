docker import 
==============================================

docker import   从tar包导入内容以创建文件系统镜像。

用法

.. code-block:: bash 

   docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]

选项

-c  将dockerfile指令应用到创建的镜像
-m  为导入的图形设置提交信息


样例

.. code-block:: bash 

    docker import http://example.com/exampleimage.tgz
    cat exampleimage.tgz | docker import - exampleimagelocal:new
    docker import -m "init"  /path/to/exampleimage.tgz
    sudo tar -c . | docker import - exampleimagedir

    