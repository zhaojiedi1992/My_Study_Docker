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

ENTRYPOINT 和CMD 

.. csv-table::
   :header: "","ENTRYPOINT"	"ENTRYPOINT exec_entry p1_entry",	"ENTRYPOINT [exec_entry, p1_entry]"

   "No CMD",	                            "error, not allowed",	        "/bin/sh -c exec_entry p1_entry",	"exec_entry p1_entry"
   "CMD [exec_cmd, p1_cmd]",	        "exec_cmd p1_cmd",	            "/bin/sh -c exec_entry p1_entry",   "exec_entry p1_entry exec_cmd p1_cmd"
   "CMD [p1_cmd, p2_cmd]",	            "p1_cmd p2_cmd",             	"/bin/sh -c exec_entry p1_entry",	"exec_entry p1_entry p1_cmd p2_cmd"
   "CMD exec_cmd p1_cmd",                	"/bin/sh -c exec_cmd p1_cmd",   "/bin/sh -c exec_entry p1_entry",	"exec_entry p1_entry /bin/sh -c exec_cmd p1_cmd"


VOLUME
--------------------------------------------
用于设置卷，用法： 

.. code-block:: dockerfile

    VOLUME ["/data"]

USER
--------------------------------------------
设置运行的用户身份，用法： 

.. code-block:: dockerfile 

    USER <user>[:<group>] or
    USER <UID>[:<GID>]

WORKDIR
--------------------------------------------
设置工作目录，对RUN,CMD,ENTRYPOINT,COPY,ADD都是有影响的。

样例： 

.. code-block:: dockerfile

    WORKDIR /a
    WORKDIR b
    WORKDIR c
    RUN pwd

上面的打印结果为/a/b/c的。

ARG
--------------------------------------------------
ARG用于指定参数，用法： 

.. code-block:: dockerfile

    ARG <name>[=<default value>]

样例： 

.. code-block:: bash 

    docker build --build-arg <varname>=<value>

.. code-block:: dockerfile 

    FROM ubuntu
    ARG CONT_IMG_VER
    ENV CONT_IMG_VER ${CONT_IMG_VER:-v1.0.0}
    RUN echo $CONT_IMG_VER


ONBUILD
--------------------------------------------------
ONBUILD会在此镜像作为其他镜像的基础镜像的时候被执行。

构建步骤：

1. 当遇到ONBUILD指令时，构建器会向正在构建的镜像的元数据添加触发器。 该指令不会影响当前构建。
2. 在构建结束时，所有触发器的列表都存储在映像清单中的OnBuild键下。 可以使用docker inspect命令检查它们。
3. 稍后，可以使用FROM指令将镜像用作新构建的基础。 作为处理FROM指令的一部分，下游构建器查找ONBUILD触发器，并按照它们注册的顺序执行它们。 
   如果任何触发器失败，则中止FROM指令，这反过来导致构建失败。 如果所有触发器都成功，则FROM指令完成，并且构建继续照常进行。
4. 执行后，触发器将从最终镜像中清除。

样例： 

.. code-block:: dockerfile 

    [...]
    ONBUILD ADD . /app/src
    ONBUILD RUN /usr/local/bin/python-build --dir /app/src
    [...]

STOPSIGNAL
--------------------------------------------------
设置发送的信号。

.. code-block:: dockerfile 

    STOPSIGNAL SIGKILL


HEALTHCHECK
--------------------------------------------------
提供健康检查功能，用法： 

.. code-block:: dockerfile 

    HEALTHCHECK [OPTIONS] CMD command
    HEALTHCHECK NONE

        # 选项有
        --interval=DURATION (default: 30s)
        --timeout=DURATION (default: 30s)
        --start-period=DURATION (default: 0s)
        --retries=N (default: 3)

SHELL
--------------------------------------------------------
指定shell的，用法。

.. code-block:: dockerfile 

    SHELL ["executable", "parameters"]
    # 在linux下为  ["/bin/sh", "-c"]
    # 在window下为 ["cmd", "/S", "/C"]

