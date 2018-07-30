dockerfile参考
=====================================

使用
---------------------------------------

.. code-block:: bash 

    docker build
    docker build -t zhaojiedi1992/helloworld:v1.0 -t zhaojiedi1992/helloworld:latest  -f  -f /path/to/a/Dockerfile .

注释
---------------------------------------
dockefile中注释使用#开头。

.. code-block:: dockerfile

    # 这是一个注释行

转义
---------------------------------------
dockerfile中使用\来转义空格、换行符等。

环境变量替换
---------------------------------------
环境变量通过ENV指定，支持bash操作的。比如${foo},${foo-abc}等

可以使用环境变量的片段为:

- ADD 
- COPY 
- ENV 
- EXPOSE 
- FROM 
- LABEL 
- STOPSIGNAL 
- USER
- VOLUME 
- WORKDIR

.. note:: 在1.4版本之后，ONBUILD也支持使用环境变量了。

.dockerignore文件
---------------------------------------
dockerfile 主要配合add,copy使用，包含在dockerignore的配置被过滤掉的。具体的语法类似gitignore文件。

FROM
---------------------------------------
FROM用于指定基础镜像的名字。

用法： 

.. code-block:: dockerfile 

    FROM <image> [AS <name>]
    FROM <image>[:<tag>] [AS <name>]
    FROM <image>[@<digest>] [AS <name>]

使用样例

.. code-block:: dockerfile

    ARG  CODE_VERSION=latest
    FROM base:${CODE_VERSION}
    CMD  /code/run-app

ARG不同于ENV的，ARG作用于docker命令构建的时候，ENV是在镜像构建后，实例化的时候运行的。

RUN
---------------------------------------
RUN有2种格式用法

.. code-block:: dockerfile 

    RUN <command>
    RUN ["executable", "param1", "param2"]

CMD
-----------------------------------------
CMD有三种格式用法。

.. code-block:: dockerfile

    CMD ["executable","param1","param2"] 
    CMD ["param1","param2"] 
    CMD command param1 param2 

LABEL
--------------------------------------------
LABEL用于定义描述信息。是一个key,value对

用法:

.. code-block:: dockerfile 

    LABEL <key>=<value> <key>=<value> <key>=<value> ...

定义的LABEL信息，可以通过docker inspect命令获取镜像的LABEL信息。

MAINTAINER (已经弃用了)
--------------------------------------------

使用如下设置维护者。

.. code-block:: dockerfile

    LABEL maintainer="SvenDowideit@home.org.au"

EXPOSE
--------------------------------------------
用于指定需要暴露的的端口，用法： 

.. code-block:: dockerfile

    EXPOSE <port> [<port>/<protocol>...]

样例： 

.. code-block:: dockerfile 

    EXPOSE 80/tcp
    EXPOSE 80/udp

我们可以在docker运行时重设这些设置信息。

.. code-block:: dockerfile

    docker run -p 80:80/tcp -p 80:80/udp ...

ENV
--------------------------------------------
用于定义运行时的变量

.. code-block:: dockerfile 

    ENV <key> <value>
    ENV <key>=<value> ...

样例： 

.. code-block:: dockerfile

    ENV myName="John Doe" myDog=Rex\ The\ Dog \
        myCat=fluffy

ADD
--------------------------------------------
用于从当前的目录复制文件到镜像中去

.. code-block:: dockerifle 

    ADD [--chown=<user>:<group>] <src>... <dest>
    ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]

COPY
--------------------------------------------
用于从当前的目录复制文件到镜像中去

.. code-block:: dockerfile 

    COPY [--chown=<user>:<group>] <src>... <dest>
    COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]


ENTRYPOINT
--------------------------------------------
ENTRYPOINT用于定义进入点，用法2种。

.. code-block:: dockerfile 

    ENTRYPOINT ["executable", "param1", "param2"] 
    ENTRYPOINT command param1 param2 

样例： 

.. code-block:: dockerfile 

    FROM ubuntu
    ENTRYPOINT ["top", "-b"]
    CMD ["-c"]
    
如果docker运行的时候想重写ENTRYPOINT,可以指定--entrypoint 选项。

