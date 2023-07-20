---
title: Hello World
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

# C++后台开发需要掌握什么？

这个话题有点大，而且像我这种小菜，只能抛砖引玉。语言只是基础，不能一味地去研究语法糖。记得我在学校的时候，特别喜欢去研究语法糖，现在想想，浪费了很多时间。当然，作为C++后端的研发工程师，你首先需要掌握C++的基础语法，需要掌握STL里面常用的库和算法，如果你觉得这还不够，你可以去系统地学习下boost库，里面多STL里面所不具有很备的，看看C++11就知道了，里面很多新增的东西都是来自boost库。当然，仅仅掌握语言还远远不够，C++做后台开发时，模块跟模块直接除了通过lib库或so库的方式相互调用外，还有更多的是采用网络交互，这个时候，你就需要掌握多线程编程和网络编程的基础知识，当然，由于开发效率的需要，现在你不需要从零搭建一个网络服务框架，比如：ACE、boost的asio和libevent。当然现在已经有各种开源的RPC框架了，比如google-rpc，你可以通过调用本地函数来完成网络包的发送与接收，so easy！那么网络通信包的格式如何定义呢？客户端和服务端需要提前约定？数据交互格式，常用的包括：json、xml和protobuffer，通常前端后后端交互会采用json，而后端各个模块的交互，你可以随便选择；对于HTTP协议的交互，我用的比较多的是json，而 tcp协议，我用的比较多的是protobuffer。当然，服务端的平台有很重要，国内后台开发，基本都是运行在Linux系统上，所以你需要掌握Linux系统的常用的命令，这样你才可以在Linux系统上运用自如，所以，如果你想从事或者即将从事C++后台开发，请暂时抛下VS下的C++学习，从现在开始，转向Linux平台下的C++开发，那里有你要编译器GCC/G++，调试时用到的gdb，如果你想依次性一个命令编译所有的文件，请学习下如何编写makefile。好了，有了编程语言，有了编译和调试方法，你就可以将你的应用程序放在你的Linux系统上监听客户端的请求了。如果某一天，你的程序出core了怎么办？你必须要学会如果找出bug，除了前面提到的gdb，在大型的应用里面，你必须要学会掌握如何追bug，这个时候，你就要学会打日志，并且分等级打印日志，这样一出问题了你就能够快速定位问题的所在。日志有了，程序也能正常跑了，那你怎么算你程序的性能或者收益呢？所以，你需要学会编写脚本语言，我个人推荐你去掌握shell脚本和python脚本，脚本语言能够一边执行一边编译，具有比较高的开发效率，不用你每次执行前编译，掌握了脚本，你不用再那么忙了，哈哈。
