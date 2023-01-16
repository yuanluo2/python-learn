###### 当你安装python后，在python的安装目录下有一个scripts目录，该目录中就有 pyinstaller.exe 文件。对于你在电脑中安装的每一个版本的python，它们各自的目录下都有这样的 pyinstaller.exe 文件。特意说明这一点是由于不同版本的 python 对于操作系统的支持并不完全一样。当你使用 python 3.10.6 的 pyinstaller 生成 exe 文件后，它实际上是不能在 win7 系统上运行的，你必须采取更低版本的 python 携带的 pyinstaller 来生成。

###### 生成命令如下：

```shell
pyinstaller -F -w test.py
```

