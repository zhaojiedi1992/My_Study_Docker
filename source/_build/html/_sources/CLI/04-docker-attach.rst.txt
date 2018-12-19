docker attach 
========================

docker attach 用于将本地标准的输入、输出和错误流附加到正在运行的容器上。 

使用attach到特定容器后，使用`ctrl+c` 发送kill信号的， 会导致容器被杀掉。
如果指定了--sig-proxy为true的时候，则发送int信号， 你可以使用ctrl+p+q来保持继续运行。 

