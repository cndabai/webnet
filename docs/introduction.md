# 软件包介绍

WebNet 软件包是自主研发的，基于 HTTP 协议的 Server 服务器实现，它不仅提供设备与 HTTP Client 通讯的基本功能，而且支持多种模块功能扩展，满足开发者对嵌入式设备服务器的功能需求。

## 软件包目录结构

WebClient 软件包目录结构如下所示：

| 名称       | 说明                     |
| ---------- | ------------------------ |
| docs       | 文档目录                 |
| inc        | 头文件目录               |
| src        | 源文件目录               |
| module     | 功能模块文件目录         |
| samples    | 示例文件目录             |
| LICENSE    | 许可证文件               |
| README.md  | 软件包使用说明           |
| SConscript | RT-Thread 默认的构建脚本 |

## 软件包功能特点

WebNet 软件包提供丰富多样的功能支持，大部分功能以模块的形式给出，每个功能模块提供单独的控制方式且不同功能模块之间互不干扰，模块与主程序之间配合形成了一套功能齐全、高性能、高并发且高度可裁剪的网络服务器框架体系。

WebNet 软件包功能特点：

- **支持 HTTP 1.0/1.1**

WebNet 软件包用于在嵌入式设备中运行网络服务器功能，其功能实现基于 HTTP 协议，且同时支持 HTTP 1.0 和 HTTP 1.1 协议，提高网络服务器兼容性。

- **支持 CGI 功能**

CGI（Common Gateway Interface ）功能，是服务器运行时的外部程序规范，可以用于扩展服务器的功能。CGI 功能可以完成浏览器和服务器的交互过程。WebNet 软件包中的 CGI 功能，用户可以**自定义注册 CGI 执行函数**，服务器通过判断浏览器请求的 CGI 类型，执行相应的函数，并给予相应反馈信息。

- **支持 ASP 变量替换功能**

ASP（Active Server Pages）功能， 实现的网页中变量替换的功能，即当浏览器访问的页面代码中有 **<% %>** 标记出现时（**支持包含  .asp 后缀的页面文件**），将自动替换成**自定义注册的ASP 执行函数**，并且给与相应反馈信息。使用此功能可以很方便地独立修改每个变量执行函数的实现方式，完成整个页面不同部分的改动，方便前后台开发分工合作。

- **支持 AUTH 基本认证功能**

在使用 HTTP 协议进行通信的过程中，HTTP 协议定义了 Basic Authentication 基本认证过程以允许HTTP 服务器对 WEB 浏览器进行用户身份认证。基本认证功能为服务器提供简单的用户验证方式，其认证方式快速、准确，适用于对服务器安全性能有一定要求的系统和设备。WebNet 软件包中基本认证功能设计成与 URL 中目录相挂钩，在使用时可以**根据目录划分权限**。

- **支持 INDEX 目录文件显示功能**

WebNet 服务器开启之后，在浏览器输入对应的设备 IP 地址和目录名，就可以访问该目录，并列出该目录下所有文件的信息，类似于简单的文件服务器，可以用于服务器文件的查询和下载。

- **支持 ALIAS 别名访问功能**

ALIAS 别名访问功能，可以给文件夹设置多个别名，可以用于**长路径的简化操作**，方便用户对资源的访问。

- **支持 SSI 文件嵌入功能**

SSI（Server Side Include）功能，一般用于页面执行服务器程序或者插入文本内容到网页中。WebNet 中的 SSI 功能目前只支持插入文本内容到网页功能，页面代码中有 **<!--#include virtual="/xxx"-->** 或者 **<!--#include file="/xxx"-->** 标记存在时（**支持包含 .shtml 、shtm 后缀的页面文件**），将自动解析成对应文件中的信息。通过 SSI 文件嵌入功能，可以使整个页面处理模块化，从而实现修改指定文件就可以完成页面的改动。

- **支持文件上传功能**

支持浏览器上传文件到 WebNet 服务器，可以自定义文件处理函数，以实现把上传的文件存储在文件系统中或直接写入目标存储器。利用此功能可以实现：

1. 上传资源文件到文件系统，如图片和网页；
2. 上传存储器镜像以烧写系统中的存储器，如 SPI FLASH；
3. 上传配置文件到系统以更新系统设置；
4. 上传固件并直接写入 FLASH 以实现固件更新功能。

- **支持预压缩功能**

预压缩功能用于提前压缩网页资源文件（**支持包含 .gz 后缀的压缩文件**），降低对存储空间的需求，缩短文件系统读取时间，减少网络流量，提高网页加载速度。一般来讲，使用预压缩功能后，WebNet 服务器占用的存储空间仅为原来的 30%~40%。

- **支持缓存功能**

WebNet 缓存机制可以判断浏览器请求文件是否修改，从而决定是否发送完整文件内容到浏览器。 WebNet 软件包缓冲机制分为如下三个级别：

1. level 0：关闭缓存功能，即浏览器每次都会从 WebNet 完整的读取文件内容；

2. level 1：开启缓存功能，WebNet 通过读取请求文件的最后修改的时间，如果和本地文件相同，则返回 304  通知浏览器文件并无更新，不会发送文件；

3. level 2：开启缓存功能，在原来判断修改时间的基础上，添加缓存文件有效时间支持，操作有效时间浏览器可重新访问该文件。

- **支持断点续传功能**

WebNet 服务器支持断点续传功能，即客户端在下载文件中途出现错误，再次下载时只需要提供给 WebNet 服务器前一次文件下载的偏移量，服务器将从指定文件偏移量发送文件内容给客户端，断点续传功能可以确保文件上传快速、准确，提高服务器运行效率。

## 软件包性能测试

测试环境：AM1808，主频 400M， DDR2 内存

| 测试环境     | 页面文件在 SD 卡上，DM9000AEP 外置网卡 |
| ------------ | -------------------------------------- |
| 静态页面请求 | 142 pages/s                            |
| CGI 页面请求 | 429 pages/s                            |

| 测试环境     | 页面文件在 SD 卡上，EMAC 外置网卡 |
| ------------ | --------------------------------- |
| 静态页面请求 | 190 pages/s                       |
| CGI 页面请求 | 567 pages/s                       |

## 软件包资源占用

ROM：16 Kbytes

RAM：1.5 Kbytes + 1.1 Kbytes x 连接数

NOTE：以上数据为参考值，示例平台是 Cortex-M3，因为指令长度和框架不同，软件包资源占用有所不同。WebNet 在处理页面时，文件系统和模块内部也会有部分资源消耗。
