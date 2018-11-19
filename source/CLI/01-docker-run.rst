docker run 
===================================

docker run 提供直接运行一个容器。

一般形式
--------------------------------

.. code-block:: bash 

    $ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]

主要选项
---------------------------------------

-d 后台运行
-a 附件到标准输出流
-t 分配一个pseudo-tty
-i 保持STDIN打开的
--restart  重启策略
--rm  停止后删除容器
--network 设置网络
--cidfile 写入容器id到特定文件中
--pid  设置pid命令空间模式
--dns  指定dns
--network-alias 网络别名
--add-hosts   添加一行到/etc/hosts 
--mac-address 指定mac地址
--ip 设置ip
-m 内存限制
--memory-swap 总呢次限制
-c cpu份额
--cpuset-cpus 允许指定的cpu
--privileged  特权模式
--log-driver 日志驱动类型
--entrypoint 指定运行时的默认值
-p 暴露特定端口
-e 指定环境变量
--tmpfs 挂载tmpfs文件系统
-v 挂载卷
--volume-from 和特定容器挂载一样的卷
-u 指定用户
-w 工作目录设置

