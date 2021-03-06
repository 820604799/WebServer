

TinyWebServer
===============
Linux下基于C++的轻量级Web服务器注释、修改版本.
原项目来自:https://github.com/qinguoyi/TinyWebServer

**有什么特点？**
* 使用 **线程池 + 非阻塞socket + epoll(ET和LT均实现) + 事件处理(Reactor和模拟Proactor均实现)** 的并发模型
* 使用**状态机**解析HTTP请求报文，支持解析**GET和POST**请求
* 访问服务器数据库实现web端用户**注册、登录**功能
* 经Webbench压力测试可以实现**上万的并发连接**数据交换
-----
快速运行
------------
* 服务器测试环境
	* Ubuntu版本20.04
	* MySQL版本5.7.29以上
* 浏览器测试环境
	* Linux
	* Chrome
	* FireFox
	* 其他浏览器暂无测试

* 测试前确认已安装MySQL数据库

    ```C++
    // 建立yourdb库
    create database yourdb;

    // 创建user表
    USE yourdb;
    CREATE TABLE user(
        username char(50) NULL,
        passwd char(50) NULL
    )ENGINE=InnoDB;

    // 添加数据
    INSERT INTO user(username, passwd) VALUES('name', 'passwd');
    ```

* 修改main.cpp中的数据库初始化信息

    ```C++
    //数据库登录名,密码,库名
    string user = "root";
    string passwd = "root";
    string databasename = "yourdb";
    ```

* build 编译项目

    ```
    make server
   
    ```

* 启动server服务器项目

    ```C++
    sudo ./server
    ```

* 浏览器端 打开指定网页

    ```C++
    127.0.0.1:9006
    ```

个性化运行
------

```C++
./server [-p port] [-l LOGWrite] [-m TRIGMode] [-o OPT_LINGER] [-s sql_num] [-t thread_num]  [-a actor_model]
```

温馨提示:以上参数不是非必须，不用全部使用，根据个人情况搭配选用即可.

* -p，自定义端口号
	* 默认9006
* -l，选择日志写入方式，默认同步写入
	* 0，同步写入
	* 1，异步写入
* -m，listenfd和connfd的模式组合，默认使用LT + LT
	* 0，表示使用LT + LT
	* 1，表示使用LT + ET
    * 2，表示使用ET + LT
    * 3，表示使用ET + ET

* -s，数据库连接数量
	* 默认为8
* -t，线程数量
	* 默认为8

* -a，选择反应堆模型，默认Proactor
	* 0，Proactor模型
	* 1，Reactor模型

测试示例命令与含义

```C++
./server -p 9007  -m 0  -s 10 -t 10  -a 1
```

- [x] 端口9007

- [x] 使用LT + LT组合
- [x] 数据库连接池内有10条连接
- [x] 线程池内有10条线程

- [x] Reactor反应堆模型

