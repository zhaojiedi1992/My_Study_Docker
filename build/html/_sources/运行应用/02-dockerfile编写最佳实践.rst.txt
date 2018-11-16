dockerfile编写最佳实践
============================================================

一般准则和建议
----------------------------------------

- 创建尽可能精简的容器
- 了解构建上下文
- 通过stdin管道Dockerfile
- 包含dockerignore文件
- 使用多阶段构建
- 不要安装不必要的包
- 解构应用程序，分离容器功能，每个容器限制为单个进程，保存容器的清洁和模块化，容器直接可以通过网络通信。
- 最小化层级
- 一个命令可以使用转义为多行，美化dockerfile文件。让别人看起来舒服点。
- 利用构建缓存


通过stdin管道Dockerfile的样例
----------------------------------------

.. tabs::

   .. tab:: docker<17.04

        .. code-block:: bash

            docker build -t foo -<<EOF
            FROM busybox
            RUN echo "hello world"
            EOF

   .. tab:: docker>=17.05 local

        .. code-block:: bash 

            docker build -t foo . -f-<<EOF
            FROM busybox
            RUN echo "hello world"
            COPY . /my-copied-files
            EOF

   .. tab:: docker>=17.05 remote

        .. code-block:: bash 

            docker build -t foo https://github.com/thajeztah/pgadmin4-docker.git -f-<<EOF
            FROM busybox
            COPY LICENSE config_local.py /usr/local/lib/python2.7/site-packages/pgadmin4/
            EOF

docker指令
----------------------------------------

FROM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
尽量选择alpine镜像作为源镜像，这样构建的镜像才最小

LABEL
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
LABEL用来说明我们的镜像信息的，尽可能的详细点。

比较详细的dockerfile label 

.. code-block:: dockerfile

    # Set one or more individual labels
    LABEL com.example.version="0.0.1-beta"
    LABEL vendor1="ACME Incorporated"
    LABEL vendor2=ZENITH\ Incorporated
    LABEL com.example.release-date="2015-02-12"
    LABEL com.example.version.is-production=""

但是上面的label会构建多层的。需要改进为如下风格的。

.. code-block:: dockerfile 

    LABEL vendor=ACME\ Incorporated \
        com.example.is-beta= \
        com.example.is-production="" \
        com.example.version="0.0.1-beta" \
        com.example.release-date="2015-02-12"

RUN
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
使用\来分割命令为多行，来增加可读性和可维护性。

USING PIPES
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

docker使用/bin/sh -c来解释run命令，该解释器仅仅评估管道中最后一个的操作码来确定成功，

.. code-block:: dockerfile

    # 即使wget失败，后续的wc -l成功，整句的就是成功的。
    RUN wget -O - https://some.site | wc -l > /number

CMD
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
CMD 尽可能采用数组形式的，且只提供参数，具体的命令使用ENDPOINT指定。

EXPOSE
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ENV
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
为了确保你的应用程序以比较简单的方式去运行，可以使用env去添加环境变量。

.. code-block:: dockerfile 

    ENV PATH /usr/local/nginx/bin:$PATH
    CMD ["nginx"]

每个ENV会添加一层，可以一次添加多个行间变量。可以使用run来替代ENV

.. code-block:: dockerfile

    FROM alpine
    RUN export ADMIN_USER="mark" \
        && echo $ADMIN_USER > ./mark \
        && unset ADMIN_USER
    CMD sh

ADD or COPY 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
ADD和COPY都可以完成文件的copy工作。 但是ADD可以下载网络资源，并可以自动完成解压到特定目录去。
强烈建议不要使用ADD从远程URL中获取包。 你应该使用curl或wget代替,样例如下。

错误做法： 

.. code-block:: dockerfile

ADD http://example.com/big.tar.xz /usr/src/things/
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all

替代做法： 

.. code-block:: dockerfile

    RUN mkdir -p /usr/src/things \
        && curl -SL http://example.com/big.tar.xz \
        | tar -xJC /usr/src/things \
        && make -C /usr/src/things all


.. note:: 如果不需要add的自动解压功能，你应该始终使用COPY。

ENTRYPOINT
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
ENTRYPOINT用于定义程序的主命令，CMD用来定义给ENTRYPOINT的参数，且2中都采用数组形式。

exec用于执行一个新命令去替换现有进程，可以保证我们的进程启动在pid为1 。

VOLUME
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
使用卷去存储数据库数据，或者配置文件等。

USER
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

可以指定一个非root的用户作为容器的运行身份。

WORKDIR
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
写WORKDIR请采用绝对路径，采用相对路径会增加维护和故障排查问题。


ONBUILD
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
ONBUILD命令是执行在当前dockefile构建完毕的镜像被别人作为基础镜像后的执行脚本。

