## eJet - a lightweight high-performance embeded Web Server

*eJet is a lightweight, high-performance, embedded Web server that implements the full-stack functions of the HTTP/1.1 protocol, including TLS/SSL, Forward/Reverse Proxy, FastCGI, Cookie, Web Cache, Access Log, HTTP Variable, HTTP Script, JSon-style configuration file, Virtual Host, HTTP Location, Directives such as Rewrite/Try_files, HTTP Tunnel, Application/Dynamic-Lib Callbacks, etc.  It's an ideal service platform hosting for upload/download of super-large files, website, PHP, CDN, Web Cache, embedded Web, etc.*

*eJet 是一个轻量级、高性能、嵌入式Web服务器，实现HTTP/1.1协议全栈功能，包括TLS/SSL、正向代理、反向代理、FastCGI、Cookie、Web Cache、访问日志、HTTP变量、HTTP Script脚本程序、JSon配置文件、虚拟主机、HTTP Location、Rewrite/Try_files等指令、HTTP Tunnel、应用回调和动态库回调等，是承载超大文件上传下载、网站、PHP、CDN、Web Cache、嵌入式Web等服务的理想平台。*


## 目录
* [一. eJet是什么？](#一-ejet是什么)
* [二. eJet系统流程和工作原理](#二-ejet系统流程和工作原理)
    * [2.1 Web服务器基本功能](#21-web服务器基本功能)
    * [2.2 eJet Web服务器启动流程](#22-ejet-web服务器启动流程)
    * [2.3 启动监听服务](#23-启动监听服务)
    * [2.4 http_pump作为事件驱动核心入口](#24-http_pump作为事件驱动核心入口)
    * [2.5 IOE_ACCEPT事件驱动eJet创建HTTPCon连接](#25-ioe_accept事件驱动ejet创建httpcon连接)
    * [2.6 IOE_READ事件驱动eJet读取请求数据](#26-ioe_read事件驱动ejet读取请求数据)
    * [2.7 解析HTTP请求数据](#27-解析http请求数据)
    * [2.8 创建HTTPMsg保存请求数据](#28-创建httpmsg保存请求数据)
    * [2.9 设置DocURI并启动HTTPMsg资源实例化](#29-设置docuri并启动httpmsg资源实例化)
    * [2.10 Proxy代理转发](#210-proxy代理转发)
        * [2.10.1 判定是否代理转发---正向代理或反向代理](#2101-判定是否代理转发---正向代理或反向代理)
        * [2.10.2 代理请求资源是否在本地缓存里](#2102-代理请求资源是否在本地缓存里)
        * [2.10.3 创建代理请求消息](#2103-创建代理请求消息)
        * [2.10.4 打开源服务器并建立通信连接](#2104-打开源服务器并建立通信连接)
        * [2.10.5 发送代理请求到源服务器](#2105-发送代理请求到源服务器)
    * [2.11 FastCGI转发](#211-fastcgi转发)
    * [2.12 读取并解析请求体](#212-读取并解析请求体)
    * [2.13 由Handler处理HTTPMsg](#213-由handler处理httpmsg)
        * [2.13.1 校验请求方法](#2131-校验请求方法)
        * [2.13.2 反向Proxy模式缓存内容处理](#2132-反向proxy模式缓存内容处理)
        * [2.13.3 正向Proxy模式校验](#2133-正向proxy模式校验)
        * [2.13.4 HTTP消息应用回调和动态库回调处理](#2134-http消息应用回调和动态库回调处理)
        * [2.13.5 读取并发送资源文件或路径下的缺省文件](#2135-读取并发送资源文件或路径下的缺省文件)
    * [2.14 发送响应到客户端](#214-发送响应到客户端)
    * [2.15 用状态机处理不完整请求](#215-用状态机处理不完整请求)
    * [2.16 设置可写通知产生IOE_WRITE事件处理网络拥塞](#216-设置可写通知产生ioe_write事件处理网络拥塞)
    * [2.17 利用定时器产生的IOE_TIMEOUT事件监管实例对象](#217-利用定时器产生的ioe_timeout事件监管实例对象)
* [三. eJet系统基本数据结构](#三-ejet系统基本数据结构)
    * [3.1 HTTPMgmt - eJet内核](#31-httpmgmt---ejet内核)
    * [3.2 HTTPMsg - 消息](#32-httpmsg---消息)
    * [3.3 HTTPCon - 通信连接](#33-httpcon---通信连接)
    * [3.4 HTTPListen - 监听服务](#34-httplisten---监听服务)
    * [3.5 HTTPHost - 虚拟主机](#35-httphost---虚拟主机)
    * [3.6 HTTPLoc - 资源位置](#36-httploc---资源位置)
    * [3.7 HTTPHeader - 头信息](#37-httpheader---头信息)
    * [3.8 HTTPUri - 资源地址URL](#38-httpuri---资源地址url)
    * [3.9 HTTPVar - HTTP变量](#39-httpvar---http变量)
    * [3.10 HTTPLog - 日志信息](#310-httplog---日志信息)
    * [3.11 CacheInfo - 缓存信息管理](#311-cacheinfo---缓存信息管理)
    * [3.12 HTTPForm - 表单信息](#312-httpform---表单信息)
    * [3.13 HTTPScript - 脚本程序](#313-httpscript---脚本程序)
    * [3.14 HTTPSrv - 源服务器](#314-httpsrv---源服务器)
    * [3.15 HTTPChunk - HTTP数据块](#315-httpchunk---http数据块)
    * [3.16 HTTPCookie - Cookie数据](#316-httpcookie---cookie数据)
    * [3.17 FcgiSrv - FastCGI服务器](#317-fcgisrv---fastcgi服务器)
    * [3.18 FcgiCon - FastCGI通信连接](#318-fcgicon---fastcgi通信连接)
    * [3.19 FcgiMsg - FastCGI消息](#319-fcgimsg---fastcgi消息)
* [四. eJet核心功能模块](#四-ejet核心功能模块)
    * [4.1 eJet资源管理架构](#41-ejet资源管理架构)
        * [4.1.1 三层资源定位架构](#411-三层资源定位架构)
        * [4.1.2 HTTP监听服务 - HTTPListen](#412-http监听服务---httplisten)
        * [4.1.3 HTTP虚拟主机 - HTTPHost](#413-http虚拟主机---httphost)
        * [4.1.4 HTTP资源位置 - HTTPLoc](#414-http资源位置---httploc)
    * [4.2 HTTP变量](#42-http变量)
        * [4.2.1 HTTP变量的定义](#421-http变量的定义)
        * [4.2.2 HTTP变量的应用](#422-http变量的应用)
        * [4.2.3 HTTP变量的类型和使用规则](#423-http变量的类型和使用规则)
        * [4.2.4 预定义的参数变量列表和实现原理](#424-预定义的参数变量列表和实现原理)
    * [4.3 HTTP Script脚本](#43-http-script脚本)
        * [4.3.1 HTTP Script脚本定义](#431-http-script脚本定义)
        * [4.3.2 Script脚本嵌入位置](#432-script脚本嵌入位置)
        * [4.3.3 Script脚本范例](#433-script脚本范例)
        * [4.3.4 Script脚本语句](#434-script脚本语句)
            * [4.3.4.1 条件语句](#4341-条件语句)
            * [4.3.4.2 赋值语句](#4342-赋值语句)
            * [4.3.4.3 返回语句](#4343-返回语句)
            * [4.3.4.4 响应语句](#4344-响应语句)
            * [4.3.4.5 rewrite语句](#4345-rewrite语句)
            * [4.3.4.6 addReqHeader语句](#4346-addreqheader语句)
            * [4.3.4.7 addResHeader语句](#4347-addresheader语句)
            * [4.3.4.8 delReqHeader语句](#4348-delreqheader语句)
            * [4.3.4.9 delResHeader语句](#4349-delresheader语句)
            * [4.3.4.10 try_files 语句](#43410-try_files-语句)
            * [4.3.4.11 注释语句](#43411-注释语句)
        * [4.3.5 Script脚本解释器](#435-script脚本解释器)
    * [4.4 JSon格式的系统配置文件](#44-json格式的系统配置文件)
        * [4.4.1 JSON语法特点](#441-json语法特点)
        * [4.4.2 eJet配置文件对JSON的扩展](#442-ejet配置文件对json的扩展)
            * [4.4.2.1 分隔符](#4421-分隔符)
            * [4.4.2.2 include指令](#4422-include指令)
            * [4.4.2.3 单行注释和多行注释](#4423-单行注释和多行注释)
            * [4.4.2.4 script语法](#4424-script语法)
        * [4.4.3 eJet配置文件](#443-ejet配置文件)
    * [4.5 事件驱动流程 http_pump](#45-事件驱动流程-http_pump)
    * [4.6 HTTP请求和响应](#46-http请求和响应)
        * [4.6.1 HTTP请求格式](#461-http请求格式)
        * [4.6.2 HTTP响应格式](#462-http响应格式)
        * [4.6.3 头和体的解析与编码](#463-头和体的解析与编码)
    * [4.7 HTTPMsg的实例化流程](#47-httpmsg的实例化流程)
        * [4.7.1 什么是HTTPMsg实例化](#471-什么是httpmsg实例化)
        * [4.7.2 匹配虚拟主机和资源位置](#472-匹配虚拟主机和资源位置)
        * [4.7.3 执行脚本程序](#473-执行脚本程序)
    * [4.8 HTTP MIME管理](#48-http-mime管理)
    * [4.9 HTTP URI管理](#49-http-uri管理)
    * [4.10 chunk_t数据结构](#410-chunk_t数据结构)
        * [4.10.1 chunk_t是什么数据结构](#4101-chunk_t是什么数据结构)
        * [4.10.2 chunk_t应用场景](#4102-chunk_t应用场景)
        * [4.10.3 chunk_t接口功能](#4103-chunk_t接口功能)
    * [4.11 HTTP请求/响应的发送流程（writev/sendfile）](#411-http请求响应的发送流程writevsendfile)
    * [4.12 eJet日志系统](#412-ejet日志系统)
    * [4.13 Callback回调机制](#414-callback回调机制)
        * [4.13.1 eJet回调机制](#4131-ejet回调机制)
        * [4.13.2 eJet全局回调函数](#4132-ejet全局回调函数)
        * [4.13.3 eJet动态库回调](#4133-ejet动态库回调)
        * [4.13.4 回调函数使用HTTPMsg的成员函数](#4134-回调函数使用httpmsg的成员函数)
    * [4.14 正则表达式的使用](#414-正则表达式的使用)
    * [4.15 TLS/SSL](#415-tlsssl)
        * [4.15.1 TLS/SSL、OpenSSL介绍](#4151-tlssslopenssl介绍)
        * [4.15.2 eJet集成OpenSSL](#4152-ejet集成openssl)
    * [4.16 Chunk传输编码解析](#416-chunk传输编码解析)
    * [4.17 反向代理和正向代理](#417-反向代理和正向代理)
        * [4.17.1 判断是否为代理请求](#4171-判断是否为代理请求)
        * [4.17.2 代理请求的实时转发](#4172-代理请求的实时转发)
        * [4.17.3 代理响应的实时转发](#4173-代理响应的实时转发)
    * [4.18 FastCGI机制和启动PHP的流程](#418-fastcgi机制和启动php的流程)
        * [4.18.1 FastCGI基本信息](#4181-fastcgi基本信息)
        * [4.18.2 eJet如何启用FastCGI](#4182-ejet如何启用fastcgi)
        * [4.18.3 FastCGI的通信规范](#4183-fastcgi的通信规范)
        * [4.18.4 FastCGI消息的实时转发](#4184-fastcgi消息的实时转发)
    * [4.19 两个通信连接的串联Pipeline](#419-两个通信连接的串联pipeline)
        * [4.19.1 两个通信连接串联成管道](#4191-两个通信连接串联成管道)
        * [4.19.2 两个连接速度不对等导致的流量拥塞](#4192-两个连接速度不对等导致的流量拥塞)
    * [4.20 HTTP Cache系统](#420-http-cache系统)
        * [4.20.1 HTTP Cache功能设置](#4201-http-cache功能设置)
        * [4.20.2 eJet系统Cache存储架构](#4202-ejet系统cache存储架构)
        * [4.20.3 eJet系统缓存处理流程](#4203-ejet系统缓存处理流程)
    * [4.21 HTTP Tunnel](#421-http-tunnel)
    * [4.22 HTTP Cookie机制](#422-http-cookie机制)
* [五. eJet为什么高性能](#五-ejet为什么高性能)
    * [5.1 事件驱动多线程ePump框架](#51-事件驱动多线程epump框架)
    * [5.2 零拷贝Zero-Copy技术](#52-零拷贝zero-copy技术)
    * [5.3 使用writev和sendfile提升发送效率](#53-使用writev和sendfile提升发送效率)
    * [5.4 内存池](#54-内存池)
    * [5.5 超大文件上传](#55-超大文件上传)
    * [5.6 不连续碎片存储读写chunk_t](#56_不连续碎片存储读写chunk_t)
* [六. eJet Web服务应用案例](#六-ejet-web服务应用案例)
    * [6.1 大型资源网站](#61-大型资源网站)
    * [6.2 承载PHP应用](#62-承载php应用)
    * [6.3 充当代理服务器](#63-充当代理服务器)
    * [6.4 Web Cache服务](#64-web-cache服务)
    * [6.5 作为CDN边缘分发](#65-作为cdn边缘分发)
    * [6.6 应用程序集成eJet](#66-应用程序集成ejet)
* [七. eJet相关的另外两个开源项目](#七-ejet相关的另外两个开源项目)
    * [adif 项目](#adif-项目)
    * [ePump项目](#epump项目)
* [八. 关于作者 老柯 (laoke)](#八-关于作者-老柯-laoke)
 
***

一. eJet是什么？
------
 
eJet Web服务器是利用 [adif数据结构和算法库](https://github.com/kehengzhong/adif) 和 [ePump框架](https://github.com/kehengzhong/epump)，用C语言开发的一个事件驱动模型、多线程、大并发连接的轻量级的高性能Web服务器，支持HTTP/1.0和HTTP/1.1协议，并支持HTTP Proxy、Tunnel等功能。

在Linux下，eJet Web服务器编译成动态库或静态库的大小约为300K，可集成嵌入到任何应用程序中，增加应用程序使用HTTP通信和服务承载的能力，使其具备像Nginx服务器一样强大的Web功能。

eJet Web服务器完全构建在ePump框架之上，利用ePump框架的多线程事件驱动模型，实现完整的HTTP请求<-->HTTP响应事务流程。eJet并没有创建进程或线程，利用ePump框架的事件驱动多线程，高效地运用服务器的CPU处理能力。

eJet接收和处理各TCP连接上的HTTP请求头和请求体，经过解析、校验、关联、实例化等处理，执行HTTP请求，或获取Web服务器特定目录下的文件，或代理客户端发起向源HTTP服务器的请求，或将HTTP请求通过FastCGI接口转发到CGI服务器，或将客户端HTTP请求交给上层设置的回调函数处理等。所有处理结果，最终以HTTP响应方式，包括HTTP响应头和响应体，通过客户端建立的TCP连接，返回给客户端。该TCP连接可以Pipe-line方式继续发送和接收多个HTTP请求和响应。

eJet服务器提供了作为Web服务器所需的其他各项功能，包括基于TLS/SSL的安全和加密传输、虚拟主机、资源位置Location的各种匹配策略、对请求URI执行动态脚本指令（包括rewrite、reply、return、try_files等）、在配置文件中使用HTTP变量、正向代理和反向代理、HTTP Proxy、FastCGI、HTTP Proxy Cache功能、HTTP Tunnel、MultiPart文件上传、动态库回调或接口函数回调机制、HTTP日志功能、CDN分发等。

eJet Web服务器采用JSon格式的配置文件，进行系统配置管理。对JSon语法做了一定的扩展，使得JSon支持include文件指令，支持嵌入Script脚本程序语言。使用扩展JSon功能的配置文件，可更加灵活、方便地扩展Web服务功能。

eJet系统大量采用了Zero-Copy、内存池、缓存等技术，来提升Web服务器处理性能和效率，加快了请求响应的处理速度，支撑更大规模的并发处理能力，支持更大规模的网络吞吐容量等。

eJet Web服务器既可以面向程序员、系统架构师提供应用程序开发接口或直接嵌入到现有系统中，也可以面向运维工程师部署完全类似Nginx Web服务器、Web Cache、CDN回源等商业服务系统，还是面向程序员提供学习、研究开发框架、通信系统等的理想平台。

开发eJet Web服务器的原则是尽可能不依赖于第三方代码和库，降低版权和复杂部署等因素带来的潜在风险。系统使用的第三方代码或库主要为：OpenSSL库、Linux系统自带的符合POSIX标准的正则表达式regex库。gzip压缩需要依赖zlib开源库，目前没有添加进来，所以eJet Web服务器暂时不提供gzip、deflate的压缩支持。


二. eJet系统流程和工作原理
------

### 2.1 Web服务器基本功能

Web服务器，常称网站服务器，将具有独立IP地址的服务器上承载的图、文、音频、视频和数据文件等资源，以URL方式来标识，以HTTP协议对外提供下载、浏览、上传、交互等服务。HTTP协议是一个请求-响应的事务型协议，Web服务器的基本工作流程基本是围绕请求应答来完成的，具体包括：
* 建立连接 —— 接受客户端连接，或拒绝并关闭连接；
* 接收请求 —— 从网络接口中读取 HTTP 请求头和请求体；
* 处理请求 —— 对请求头和请求体进行解析；
* 访问资源 —— 访问HTTP请求指定的资源；
* 构建响应 —— 创建带有头和体的 HTTP 响应；
* 发送响应 —— 将响应回送给客户端；
* 记录日志 —— 与已完成事的内容记录在日志文件中。

eJet Web服务器也是按照上述流程来设计和实现的，在这些环节中，增加了各种应用程序或配置接口，更好地满足服务需求，及为提升服务性能而设计了很多功能模块。

### 2.2 eJet Web服务器启动流程

在启动系统时，首先为各模块分配空间和资源，解析和读取系统配置信息，初始化各模块，并构建各模块数据结构，包括：
* 初始化SSL库模块；
* 根据配置，URI在解码/编码时，设置需要进行转义处理的字符的位掩码；
* 初始化HTTP请求代理，向Origin服务器发送HTTP请求时，根据配置信息，设置Proxy地址和端口
* 初始化HTTP访问日志模块；
* 初始化HTTP连接、HTTP消息等核心数据结构，并分配内存池；
* 加载HTTP MIME数据，并初始化MIME管理模块；
* 初始化HTTP Cache缓存管理模块；
* 初始化HTTP变量和变量管理模块；
* 设置并初始化HTTP响应状态码管理；
* 初始化Origin服务器管理模块；
* 构建SSL Context上下文环境；
* 初始化HTTP Overhead流量统计模块；
* 加载并初始化FastCGI模块；
* 初始化HTTP Cookie模块；
* 根据系统配置，初始化并加载HTTP Listen监听服务、虚拟主机服务、资源定位服务模块，包括配置中设定的Script脚本、配置选项等；

### 2.3 启动监听服务

eJet系统启动HTTP监听服务时，调用ePump框架提供的接口函数eptcp_mlisten，设置事件回调函数为http_pump，那么监听端口的所有事件都会发送到http_pump中，包括TCP连接建立请求等，http_pump是eJet系统事件驱动模型的核心。

要特别说明的是ePump框架中提供的接口函数eptcp_mlisten封装了很多操作系统细节，自动识别当前操作系统是否支持REUSEPORT，以及内核级针对多线程监听时的TCP连接事件在多线程（多进程）间的负载均衡，如果支持，就启用这个内核功能，如果不支持，就启用ePump系统实现的用户态下的TCP连接事件多线程负载均衡，最终确保高效利用CPU的并发处理能力。

### 2.4 http_pump作为事件驱动核心入口

eJet系统依赖ePump多线程事件驱动框架，本身不创建线程，也不创建进程，通过ePump的事件回调机制，处理所有来自ePump的事件，这是典型的事件驱动event-driven架构。

eJet系统将http_pump设置为ePump框架的事件回调入口函数，所有底层通信设施和定时器产生的任何事件，都会送到http_pump函数来处理，函数原型如下：
```
int http_pump (void * vmgmt, void * vobj, int event, int fdtype)
```
vmgmt是HTTPMgmt实例对象，vobj是产生该事件的iodev_t设备或定时器，event是事件类型，fdtype是iodev_t的文件描述类型。

### 2.5 IOE_ACCEPT事件驱动eJet创建HTTPCon连接

eJet系统的服务起点是http_pump模块接收到客户端的TCP连接请求，即IOE_ACCEPT事件，响应该事件的处理流程是创建HTTPCon连接，保存客户端请求的各种信息，包括iodev_t socket设备对象、四元组地址、是否为SSL连接等，启动定时器来管理当前连接的寿命，并将当前TCP连接的iodev_t设备绑定到一个负载最低的epump线程中，通过该线程监听并创建该TCP连接后续各类读、写等事件。

### 2.6 IOE_READ事件驱动eJet读取请求数据

客户端发起的TCP连接接受成功后，客户端开始发送HTTP Request，eJet系统http_pump模块会收到IOE_READ事件，即http_pump会被ePump的工作线程回调执行。响应该事件的处理流程是根据回调参数得到HTTPCon实例，如果是普通的TCP连接，则调用TCP非阻塞、零拷贝接口函数，读取内核中该连接上的数据存入缓冲区，如果是SSL连接，则调用SSL接口函数完成SSL握手、认证过程，并在加密连接上读取解密过的数据存入缓冲区。

### 2.7 解析HTTP请求数据

对HTTPCon内缓冲区的数据进行解析处理，根据HTTP协议规范约定，HTTP请求头和请求体之间有两个空行即4字节\r\n\r\n，因为请求体的内容是否存在、是什么内容格式、大小等信息都包含在HTTP请求头中。首先通过快速字符串模式匹配算法，定位出缓冲区数据中是否存在\r\n\r\n，即找到HTTP Request中头信息的结尾后，对请求行、请求头进行解析。

### 2.8 创建HTTPMsg保存请求数据

创建HTTPMsg实例，保存HTTP请求的各项信息，将HTTP请求头字节流存入HTTPMsg中的req_header_stream，基于这个流，解析出HTTP Method、URL、HTTP_VERSION和所有的HTTP请求头，请求头保存在hashtab中，方便快速查找。对特定的请求头做特殊解析处理，如针对Range头，需根据Range规范解析出客户端想要目标资源的部分内容，起始位置、长度等；需根据Cookie头来解析出各个Cookie信息；Connection头表示客户端希望当前TCP连接是否为Long-lived连接；Content-Type头中，如果存在multipart内容，则需以该格式解析后续的请求体；最主要的是请求体内容的传输编码格式是什么，需要Conent-Length头或Transfer-Encoding头来决定。

要强调一下的是，请求头信息都是小碎片内存开销，eJet系统采用内存池和保存偏移量等方式，降低了使用大量碎片内存分配导致的内存使用效率风险。

针对请求行、请求头信息解析成功后，构建并设置URL绝对地址，并将当前HTTPMsg实例对象加入到HTTPCon的消息队列中。校验当前请求内容是否合法，非法或不支持的请求，直接返回404或505等错误，并终止当前HTTPCon连接。

### 2.9 设置DocURI并启动HTTPMsg资源实例化

此刻，需启动HTTPMsg请求资源实例化流程，先设置当前请求URL地址为DocURI，得到当前HTTP请求的主机名、端口、路径、Query参数等信息。在当前监听服务HTTPListen下，根据HTTP请求的主机名，利用HTTPListen和HTTPHost管理结构，找到系统配置的虚拟主机对象HTTPHost，随后根据路径名，基于当前虚拟主机下资源位置的配置规则，将当前请求路径按照精准匹配、前缀匹配、正则表达式匹配等去匹配虚拟主机下的多个资源位置HTTPLoc。匹配成功后，分别指向HTTPListen监听服务下、虚拟主机HTTPHost下、资源位置HTTPLoc下的脚本程序，脚本程序中可能会包含rewrite、try_files等导致循环嵌套执行HTTPMsg请求资源实例化的指令。注：嵌套执行的总次数不超过16次。

完成了请求内容的解析和HTTPMsg请求资源实例化后，在接收并解析请求体之前，需要做两个判断：一是判断当前请求是否采用正向代理Forward Proxy或者反向代理Reverse Proxy进行转发；二是判断当前请求是否要使用FastCGI转发。

### 2.10 Proxy代理转发

#### 2.10.1 判定是否代理转发---正向代理或反向代理

判断是否为Proxy代理转发，如果请求URI是绝对URL地址，即URL地址包含了`http://domain:port`，并且其URL中的地址不是当前主机，那么当前HTTP请求需要做正向代理转发处理，将当前地址作为正向代理转发地址。如果HTTPMsg中的资源位置HTTPLoc中在配置文件中设置了反向代理，获取配置中的反向代理URL地址，如果配置中是正则表达式匹配，则需对反向代理URL地址做正则匹配后的变量替换处理，如果不是正则表达式匹配，则从请求路径中删除掉匹配子串，剩余部分添加加到配置中的反向代理URL中。最后，将HTTP请求的query参数添加到反向代理URL后面，并最终确认为反向Proxy代理转发。

#### 2.10.2 代理请求资源是否在本地缓存里

如果是Proxy代理转发，首先检查转发的请求资源是否已经保存在本地缓存了，其方法是检查当前HTTPLoc资源位置中Cache选项是否打开、CacheFile缓存文件是否设置，如果没有HTTPLoc资源位置，判断发送HTTP请求的配置项里Cache开关和CacheFile是否设置，CacheFile一般含有HTTP变量，经过变量取值处理后的文件为缓存文件。缓存文件如果存在，则资源已在本地，否则继续检查该缓存文件对应的缓存信息管理文件CacheInfo是否存在，如果存在的话，则将当前请求的资源内容区域，与CacheInfo中已经保存的缓存区域比对，如果请求区域的内容都保存在临时缓存文件中，则请求资源已经保存在本地。

#### 2.10.3 创建代理请求消息

如果请求资源已经保存在本地缓存中，则跳过当前Proxy代理转发流程，进行下一步处理。如果不在本地缓存，则创建一个新的代理请求HTTPMsg消息，设置代理请求URL地址，将源请求HTTPMsg的请求头复制到Proxy请求的HTTPMsg中，根据CacheInfo判断，如果本地缓存中保存了一部分请求内容，那么需要从剩余的未保存内容开始，设置Range请求头，去请求未保存部分，不用重新请求已经缓存了的内容。

#### 2.10.4 打开源服务器并建立通信连接

根据请求的目标IP地址和端口，打开或创建代表Origin服务器的HTTPSrv实例，并在HTTPSrv下创建或复用一个新的TCP连接HTTPCon，将代理消息HTTPMsg交给该TCP连接来发送，并设置当前TCP连接的任何后续读写事件，都采用当前工作线程，即跟源请求的TCP连接一起都由一个worker工作线程来处理事件，以确保处理环节的连续性pipeline。

#### 2.10.5 发送代理请求到源服务器

在面向Origin服务器的代理TCP连接上发送代理HTTPMsg消息，过程比较复杂。不仅仅要转发请求头，还要把源HTTP请求的消息体转发给目标服务器。由于传输网络的抖动、延迟、请求消息体数据大小不一、传输编码差异等因素，请求消息体不一定都存储在本地缓冲区，也不一能来一个数据立即转发一个，需要做解析、碎片化发送等处理，具体算法和流程见后面章节。完成Proxy代理转发后，当前客户端HTTP请求的读事件处理完毕。

### 2.11 FastCGI转发

如果当前请求不是Proxy代理转发，则需继续判断是否为FastCGI转发。根据HTTPMsg中的资源位置HTTPLoc的配置信息，判断是否为FastCGI转发模式，如果是FastCGI转发模式，则将转发URL，即FastCGI服务器地址URL返回出来。否则跳过FastCGI转发流程。

如果是FastCGI转发，首先使用FastCGI服务器URL地址，打开或创建FastCGI主机实例FcgiSrv，随后创建FastCGI转发消息FcgiMsg，打开或创建到达FastCGI服务器的Unix Socket连接或TCP连接FcgiCon，并将该消息绑定到连接上，启动发送流程。

### 2.12 读取并解析请求体

如果当前HTTP请求消息既不是Proxy代理转发，也不是FastCGI转发，则根据请求头判断是否有后续的消息体需要读取、解析和存储等处理。如果请求头中包含Content-Length，并且值大于0，或者包含Transfer-Encoding头，则需把当前HTTPCon缓冲区内容拷贝到HTTPMsg的请求体缓冲区req_body_stream中，或者根据系统配置文件中receive request中的body cache开关，以及是否启动Cache缓存的消息体大小阈值，来决定请求体是否启用Cache缓存，如果启用了请求体缓存，并请求消息体大小也大于缓存阈值，则将当前请求体的内容都存入到缓存文件中。对于Chunk编码的消息体，需要解析解码处理。将请求体内容，无论是在内存中，还是在缓存文件里，都添加到不连续碎片存储管理结构chunk_t中。随后，根据请求头Content-Type值，对请求体内容进行处理，如果是multipart格式，则将消息体各部分的内容解析到http_form_t中，如果是urlencoded格式，则解析到kev-value键值对链表中，如果是JSon格式，则解析成JSon数据结构，而解析出来的这些内容的存储结构都是HTTPMsg对象的成员。

### 2.13 由Handler处理HTTPMsg

#### 2.13.1 校验请求方法

处理完消息头或消息体后，封装了客户端请求所有信息的HTTPMsg会交给Handler即http_msg_handle来处理。Handler判断请求Method方法，如果是CONNECT时，建立TCP隧道，并返回隧道建立结果状态，生成HTTP响应发送给客户端。如果不是系统支持的方法，如GET、POST、DELETE、PUT、HEAD、OPTIONS、TRACE等，则直接返回405错误。

#### 2.13.2 反向Proxy模式缓存内容处理

Handler处理这些请求方法时，首先判断当前请求内容是否是Proxy模式下的缓存内容，如果是缓存内容，则将缓存文件添加到用于存储响应体的不连续碎片存储管理数据结构res_chunk_body中，当客户端请求时只是部分区域内容，添加到响应体时会自动根据请求头的Range头规范，把指定区域的缓存内容添加到响应体，设置响应状态码和响应内容类型，生成HTTP响应，并启动HTTP响应的发送流程发送给客户端。

#### 2.13.3 正向Proxy模式校验

如果HTTP请求的不是Proxy模式下的缓存内容，先检查一下当前请求是不是正向代理转发，并且当前HTTPListen监听的系统配置里不允许正向代理转发，返回403错误。再检查当前HTTPMsg实例中资源位置HTTPLoc是否存在，不存在则返回404错误。

#### 2.13.4 HTTP消息应用回调和动态库回调处理

针对请求各项检查完毕后，先看看是否设置了系统级的应用回调函数，如果存在，将当前HTTP请求的HTTPMsg实例对象作为参数，执行应用级的回调函数。随后，判断该HTTPMsg是否被成功地处理并发送了HTTP响应，如果尚未发送响应，则检查当前HTTPListen是否配置了动态库回调函数，如果存在，则加载动态库并执行动态库的回调函数。

#### 2.13.5 读取并发送资源文件或路径下的缺省文件

到这里，如果该HTTP请求的HTTPMsg还没有被处理并发送响应，则根据其资源位置HTTPLoc，获取其资源文件路径，判断该路径的资源文件是否存在，如果存在，则将该文件添加到响应体的res_chunk_body中，启动HTTP响应的发送流程。如果资源文件不存在，但资源文件路径是一个目录，根据资源位置HTTPLoc下配置的缺省文件列表，逐个查找该目录下的缺省文件是否存在，如果存在，则将该文件添加到响应体的res_body_chunk中，并发送给客户端。

最后，如果该HTTP请求的HTTPMsg还没有被各级接口成功地处理和发送响应，把404错误码发给客户端，eJet不知道客户端想要什么！

### 2.14 发送响应到客户端

到此为止，客户端请求数据通过IOE_READ事件，驱动eJet服务器的全部工作流程，只剩下最后环节，发送HTTP响应给客户端，eJet系统中发送HTTP响应的入口是Reply函数。当然大量的工作是检查响应头是否完整、响应数据是否一致、状态码是否呼应了响应体内容等，需要补充添加或删除响应头等操作，完成响应头数据处理后，对响应数据进行编码。编码过程中，如果发现请求时要求响应内容进行压缩，则根据客户端支持的压缩算法，对响应体内容进行压缩处理。响应头进行编码后的字节流保存在res_stream中，并添加res_body_chunk中的最前面。调用发送函数，将res_body_chunk中的内容发送给客户端，具体细节流程参见后面章节。

### 2.15 用状态机处理不完整请求

以上IOE_READ事件驱动流程，是比较理想的通信过程，通常情况没有那么乐观，由于网络抖动、传输延迟、大并发访问等因素，会导致一个客户端请求数据（包括请求头和请求体）并不是一次性到达，也就是http_pump收到IOE_READ事件时，很多情况下是一个不完整的HTTP请求数据包，甚至多次收到IOE_READ事件并读取数据时到缓冲区时，也是不完整的，需要以上各个处理流程和环节，记录处理状态，基于有限状态自动机FSM模型，在不同状态下完成不同的处理动作。

### 2.16 设置可写通知产生IOE_WRITE事件处理网络拥塞

在调用发送函数发送数据到对方时，由于网络拥塞、带宽不足、客户端处理能力有限等因素，导致Web服务器内核的TCP发送缓冲区已满，后续数据不可写入了，需要调用ePump框架的iodev_add_notify函数，对当前TCP连接设置可写通知监听（writable readiness notification）。即ePump框架监控到该TCP连接一旦可写入数据了，ePump框架就会发送IOE_WRITE事件，驱动http_pump来执行可写入处理，依然根据回调参数得到HTTPCon实例，基于HTTPCon上当前正在发送中的HTTPMsg对象，得到该请求或响应的发送信息，继续将rex_body_chunk中的内容发送到对方，具体TCP发送流程参见后面章节。

### 2.17 利用定时器产生的IOE_TIMEOUT事件监管实例对象

在处理请求和响应时，eJet系统会启动定时器来管理实例对象的寿命周期，如创建HTTPCon对象时，会启动一个life定时器，跟踪管理当前连接的寿命，即所有数据处理完毕且没有其他数据需要处理时，该HTTPCon连接存续10秒左右后，就需要关闭，这是基于定时器来实现的业务流程。

当定时器超时后，ePump框架产生IOE_TIMEOUT事件，回调http_pump函数。根据回调参数，获取到定时器类型、定时器绑定的对象，譬如是HTTPCon连接对象还是Origin服务器HTTPSrv对象等，再分别调用各个实例对象的定时器超时处理逻辑。eJet系统定义了4类定时器，接收客户端请求的HTTPCon连接管理定时器、主动连接Origin服务器时建立的HTTPCon连接管理定时器、主动连接远程主机时以防超时的定时器、Origin服务器HTTPSrv寿命管理定时器。


三. eJet系统基本数据结构
------

eJet Web服务器系统虽然使用标准C语言编写实现的，但系统数据结构的设计遵循了面向对象思想，将管理不同功能的数据属性和操作抽象出来，封装成标准的数据结构和函数实现，以实例对象方式，将实现的各级功能模块加载驻留，并被其他模块继承或复用。

根据eJet Web服务器功能目标，围绕HTTP请求-响应基本事务流程，设计了如下基本数据结构：

* **HTTPMgmt** --- eJet管理入口
* **HTTPMsg** --- 封装请求和响应信息的HTTP消息
* **HTTPCon** --- HTTP通信连接管理
* **HTTPListen** --- HTTP监听服务
* **HTTPHost** --- HTTP虚拟主机
* **HTTPLoc** --- HTTP资源位置
* **HTTPHeader** --- HTTP头信息
* **HTTPUri** --- HTTP URI管理
* **HTTPVar** --- HTTP变量
* **HTTPLog** --- HTTP日志
* **CacheInfo** --- HTTP缓存信息管理
* **HTTPForm** --- HTTP表单管理
* **HTTPScript** --- HTTP脚本
* **HTTPSrv** --- HTTP Origin服务器管理
* **HTTPChunk** --- HTTP Chunk数据管理
* **HTTPCookie** --- HTTP Cookie数据
* **FcgiSrv** --- FastCGI服务器管理
* **FcgiCon** --- FastCGI通信连接
* **FcgiMsg** --- 封装FastCGI请求和响应的消息

### 3.1 HTTPMgmt - eJet内核

整个eJet系统使用HTTPMgmt数据结构作为统一的资源管理入口，所有的配置、内存管理、监听管理、消息、缓存等实例对象都在HTTPMgmt下。

### 3.2 HTTPMsg - 消息

每个HTTP请求和响应的事务信息由HTTPMsg实例对象来存储和管理，HTTPMsg对象内容HTTP请求的请求头、请求体、响应头、响应体、虚拟主机、资源位置、缓存、代理等信息。成功返回HTTP响应给客户端后，HTTPMsg被回收销毁。

### 3.3 HTTPCon - 通信连接

客户端发起建立的TCP连接、eJet主动对外发起HTTP请求建立的TCP连接，都是由HTTPCon数据结构来管理。HTTPCon管理TCP连接四元组地址、当前正在接收处理或排队处理的HTTPMsg列表、HTTP隧道、TCP连接iodev_t设备对象等信息；还有HTTP连接的工作状态、连接寿命等管理；最主要的是维持一个可动态扩展的接收缓冲区，接收来自对方的任何数据进行解析和处理。

### 3.4 HTTPListen - 监听服务

HTTPListen数据结构管理HTTP监听的本地地址和本地端口，是否需要SSL连接，SSL所需的证书、私钥、CA校验证书，端口监听实例对象，每个HTTPMsg产生后需进行实例化，会执行Listen级别的Script脚本，每个HTTPListen下属多个HTTPHost主机，等等。接受的HTTPCon连接和HTTPMsg消息实例，必须关联到某个HTTPListen中。

### 3.5 HTTPHost - 虚拟主机

HTTPHost代表Host虚拟主机，同一个IP地址和端口下（HTTPListen）可以有很多个虚拟主机，即可以承载很多不同域名的网站。Host主机名既可以是映射到本机IP地址的域名，也可以直接是本机IP地址，HTTP请求头中的Host值就对应相应的Host主机。HTTPHost管理的数据包括主机域名或IP地址、反向代理URL地址、访问文件系统的根路径、通过SSL的SNI机制选择不同的域名下的证书、私钥，以及管理多个HTTPLoc，对HTTPMsg实例化处理时进行URI信息的精准匹配、前缀匹配、正则表达式匹配等来确定最终的HTTPLoc，还有400-520之间错误状态码页面管理等等。

### 3.6 HTTPLoc - 资源位置

HTTPLoc代表访问资源的最终位置，通过请求URI的文档路径来唯一标识，定位资源所在的最终位置，是客户端的连接请求是哪个监听端口，就确定属于哪个HTTPListen，根据HTTP请求头的Host值，匹配出HTTPHost虚拟主机，然后根据DocURI下的请求路径，分别采用精准匹配、前缀匹配、正则表达式匹配来找到最终的HTTPLoc资源位置。HTTPLoc信息包括：匹配路径和匹配模式、资源位置类型（Server、Proxy、FastCGI）、资源根路径、代理URL、Cache文件、执行脚本等。

### 3.7 HTTPHeader - 头信息

HTTP头信息是由name和value键值对构成，中间使用冒号（：）分隔，其中Name只能是字母和下划线组成。管理每条头信息的数据结构为HTTPHeader，分别包括头信息的Name和Value信息。需要注意的是，eJet系统中HTTPHeader内的name和value都是指针，指向HTTPMsg中的header_stream，并没有分配实际存储空间，这也是Zero-Copy思想的一部分。

### 3.8 HTTPUri - 资源地址URL

对URI进行存储和解析处理的数据结构是HTTPUri，eJet系统在接收到客户端请求后，一般在HTTPMsg中有三个HTTPUri实例对象来保存三种URI，一个是请求URI，第二个是经过资源位置实例化后的DocURI，第三个是绝对URI。设置URI后，会自动地将该URI解析分解到不同的字段中，如Host、Port、Path、Query、File等。

### 3.9 HTTPVar - HTTP变量

HTTPVar变量是指在eJet服务器运行期间，可通过Script脚本程序或配置文件里动态地读取访问当前HTTP请求响应相对应的HTTPMsg实例对象内特定数据的变量，一般在配置文件、访问日志等地方需要动态地配置或使用这些变量。HTTPVar变量包括全局变量、局部变量、Location变量，变量的引用必须以$开头，后跟变量名，如果变量后面还有连续紧随的其他字符串，则需用{}来包括住变量名。

### 3.10 HTTPLog - 日志信息

每个HTTP请求和响应的信息都要写入日志文件，方便运维和其他统计系统进行处理和分析。HTTPLog数据结构保存日志文件名、文件句柄、要写入日志文件的字段列表等信息，待写入日志文件的字段采用HTTPVar变量方式，在配置文件中设定。HTTPMsg在关闭之前，将配置文件设定的这些变量内容，从HTTPMsg实例变量以及其他实例对象中提取出来，统一写入access.log日志文件中。

### 3.11 CacheInfo - 缓存信息管理

在正向代理或反向代理模式下，客户端的请求都会转移到Origin源服务器，并将Origin服务器的响应转发给客户端。大量的客户端请求，将会导致转发和转收的效率不高，需要采用HTTP Cache系统，将响应内容存储在本地。HTTP Cache存储系统是由Raw缓存文件和缓存信息管理文件组成，Raw缓存文件负责存储实际的文件介质内容，缓存信息管理文件与数据结构CacheInfo一致，每个缓存文件都有一个全局唯一的CacheInfo，管理缓存的各种信息，包括缓存文件名、缓存文件的MIME类型、文件大小、实际缓存的文件大小、缓存策略、文件创建和更新时间，最主要成员是FragPack，记录Raw缓存文件里的所有已下载碎片块存储的位置和大小，确保哪个位置区域是否保存了文件内容。系统根据CacheInfo，可精准地知道缓存文件的存储信息和存储内容读写访问。

### 3.12 HTTPForm - 表单信息

客户端采用HTTP Post方法上传多个需要用户输入的信息内容到Web服务器，包括上传本地文件等，这种情况一般采用Content-Type为multipart/form-data的内容编码方式，各内容之间用客户端随机产生的boundary字符串分隔。eJet Web服务器设计了HTTPForm数据结构来管理multipart/form-data内容的解析和存储，一般情况下，上传内容过大会自动保存到缓存文件里，HTTPForm接口函数对缓存文件进行解析，分别解构出各个字段的名称，该字段的内容在缓存文件中的位置和大小，如果该字段是文件，则记录文件名，以及该上传文件在缓存文件中的起始位置和大小。应用程序可以使用HTTPForm来访问客户端上传的各个字段名称和字段内容，如果上传的是文件，可调用HTTPForm接口，将缓存文件中相应区域的内容提取出来，写入到应用程序指定的目录中。

### 3.13 HTTPScript - 脚本程序

eJet Web服务器的配置文件采用JSon格式，在http.listen（对应HTTPListen对象）、http.listen.host（对应HTTPHost对象）、http.listen.host.location（对应HTTPLoc对象）这三种对象下，通过对JSon语法进行扩展，都可以配置增加script对象，script对象的内容格式是参考C语言语法规范的脚本程序，由eJet系统在特定时刻解释并执行。当客户端发起的每个HTTP请求到达eJet Web服务器时，eJet解析请求并创建HTTPMsg对象，HTTPMsg对象实例化过程主要是根据请求信息匹配和设定HTTPMsg自己的HTTPListen、HTTPHost、HTTPLoc，在匹配到这三个对象的那一刻，eJet系统首先会调用HTTPScript接口来解释执行这三个对象下配置的Script对象中的脚本程序。像rewrite、return、reply、try_files、if、else等指令和语法都是脚本程序的基本内容。脚本程序的动态执行使得客户端的请求，以更加灵活机动的方式被处理。

### 3.14 HTTPSrv - 源服务器

eJet Web服务器可充当HTTP客户端向远程HTTP服务器发起HTTP请求，或充当代理模式发起HTTP请求，建立的HTTPCon连接主要是根据目标IP地址和端口来区分，具有相同目的IP地址和端口的HTTPCon请求，可以复用来发送后续相同地址的HTTP请求，这是用HTTPSrv数据结构来管理这些具有相同目的IP地址和端口的HTTPCon连接，这样，HTTPSrv就代表了HTTP Origin服务器。HTTPSrv包含一个HTTPMsg请求消息队列，当有HTTP请求发送到某个HTTP Origin服务器时，直接将该请求消息添加到该队列中，HTTPSrv会在当前连接中均衡分配一个HTTPCon或创建一个新的HTTPCon，来发送该请求。

### 3.15 HTTPChunk - HTTP数据块

HTTP请求和响应的消息体Body在传输过程有两种方式来标识或编码，一种是采用Content-Length，另一种是采用Transfer-Encoding为chunked的传输编码。后者是将Body数据分成一个个Chunk数据块，每个数据块前头是16进制的数据块长度和两个换行符\r\n，chunk数据块后面再跟两个换行符\r\n，最后是以长度为0的Chunk数据块来结尾。这种编码方式特别适合不知道实际长度的内容的传输，如实时压缩的内容、直播流媒体内容等。eJet系统设计HTTPChunk数据结构来解析、存储Chunk数据块内容，尤其是连续传输的Chunk数据块因为网络抖动等原因断续接收时，需要HTTPChunk结构来跟踪Chunk块状态。

### 3.16 HTTPCookie - Cookie数据

OSI七层协议模型中，作为Transaction事务层的HTTP协议是State-less协议，并没有保存通信双方的会话状态，而是通过Cookie机制来维持Session。Web Server通过在HTTP Response头中增加Set-Cookie头来设置Cookie信息，客户端在随后的HTTP Request请求中增加Cookie头将Cookie信息携带上。Cookie内容是由很多个name/value键值对组成，同一个Cookie下不同的name/value键值对是用分号（;）隔开，应用服务器可以根据Cookie键值对来保存会话状态，譬如id、登录会话串等。在Set-Cookie规范设置Cookie时，每个Cookie的属性中必须包含Domain、Path，指定该Cookie属于哪个域名主机的哪个路径，一般还包含max-age和expires，指定该Cookie的寿命周期。eJet系统通过HTTPCookie来解析、保存和管理这些Cookie信息。

### 3.17 FcgiSrv - FastCGI服务器

HTTP请求的资源如果是PHP等内容时，eJet会启动FastCGI模块，将HTTP请求内容通过FastCGI协议发送到FastCGI服务器，如php-fpm，FastCGI服务器执行脚本程序后，将响应内容通过FastCGI协议返回给eJet Web服务器，组装成HTTP响应返回给客户端。通常FastCGI服务器跟eJet Web服务器位于同一台服务器上，与FastCGI服务器之间的通信，一般采用进程间通信IPC机制，如Unix Socket或TCP协议。eJet采用FcgiSrv数据结构来标识管理FastCGI服务器，服务器的标识一般为 unix:/dev/shm/php-cgi.sock 或 fastcgi://127.0.0.1:9000。每个FcgiSrv的角色类似HTTPSrv，代表某个FastCGI服务器，一个eJet Web服务器可以部署配置多个FastCGI服务器。每个FastCGI服务器管理FcgiMsg消息队列，建立和维持多个FastCGI协议的连接FcgiCon。

### 3.18 FcgiCon - FastCGI通信连接

通过Unix Socket或TCP协议，来承载FastCGI协议的通信服务，用FcgiCon数据结构来管理每一个可靠通信连接。首先需要维持一个通信接收缓冲区，任何来自对方的数据，通过事件通知驱动接收接口，存储在缓冲区中用于解析和处理。其次保存该连接的iodev_t对象，以及维持连接所需的定时器，当前正在发送或接收的FcgiMsg消息，或管理处于FIFO排队队列中的消息。每个FcgiCon隶属于FcgiSrv，一个FcgiSrv下有多个FcgiCon，共同均衡地承担传输任务。

### 3.19 FcgiMsg - FastCGI消息

FastCGI协议定义了传输数据规范，数据是由一个到多个FastCGI Record构成，Record都是8字节对齐，每个Record包含一个8字节长的头部，最大不超过65KB大小的可变长度消息体，最后是为了补齐8字节对齐所需的padding字节。FastCGI协议Record类型共有10种，分别为BEGIN_REQUEST、ABORT_REQUEST、END_REQUEST、PARAMS、STDIN、STDOUT、STDERR、DATA、GET_VALUES、GET_VALUES_RESULT。FcgiMsg封装了FastCGI数据规范的这些信息，对FastCGI信息内容进行解析或编码，负责传输过程中消息内容状态的跟踪记录，作为FastCGI通信接口的最基本数据传输单元。


四. eJet核心功能模块
------

### 4.1 eJet资源管理架构

#### 4.1.1 三层资源定位架构

eJet Web服务器的资源管理结构分成三层：
* **HTTP监听服务HTTPListen** - 对应的是监听本地IP地址和端口后的TCP连接
* **HTTP虚拟主机** - 对应的是请求主机名称domain
* **HTTP资源位置HTTPLoc** - 对应的是主机下的各个资源目录

一个eJet Web服务器可以启动一个到多个监听服务HTTPListen，一个监听服务下可以配置一个到多个HTTP虚拟主机，一个虚拟主机下可以配置多个资源位置HTTPLoc。这里的‘多个’没有数量限制，取决于系统的物理和内核资源限制。

#### 4.1.2 HTTP监听服务 - HTTPListen

HTTP监听服务HTTPListen是指eJet Web服务器在启动时，需要绑定本地某个服务器IP地址和某个端口后，启动TCP监听服务，等候接收客户端发起TCP连接和HTTP请求数据，每个接受的HTTPCon连接一定属于某个HTTP监听服务HTTPListen。严格来说，HTTPListen负责接受HTTPCon连接，并将请求数据存储到HTTPCon的接收缓冲区，所以监听服务对应的是TC连接资源管理，即对应的是请求资源的domain和端口。

HTTP监听服务的配置信息格式参考如下：
```
listen = {
    local ip = *; #192.168.1.151
    port = 443;
    forward proxy = on;

    ssl = on;
    ssl certificate = cert.pem;
    ssl private key = cert.key;
    ssl ca certificate = cacert.pem;

    request process library = reqhandle.so

    script = {
        #reply 302 https://ke.test.ejetsrv.com:8443$request_uri;
        addResHeader X-Nat-IP $remote_addr;
    }

    host = {.....}
    host = {.....}
    host = {.....}
}
```

一台物理服务器可以安装多个网卡，每个网卡配置一个独立IP地址，HTTP监听服务可以监听某一个IP地址上的某个端口，也可以监听所有IP地址上的同一个端口。能启动监听服务的端口数量理论上是65536个，其中小于1024的端口需要有root超户权限才能监听。

HTTP监听服务HTTPListen依赖于底层ePump框架的eptcp_mlisten接口函数，通过该接口，让每一个epump监听线程都去监听指定IP地址和端口上的连接请求和数据请求服务。对于支持REUSEPORT的操作系统内核，大量客户端发起的并发连接，将会通过内核accept系统调用均衡地分摊到各epump线程处理，对于不支持REUSEPORT的操作系统，ePump框架负责大并发连接在各监听线程间的负载均衡。

HTTP监听服务HTTPListen可以设置当前监听为需要SSL的安全连接，并配置SSL握手所需的私钥、证书等。配置为SSL安全连接监听服务后，客户端发起的HTTP请求都必须是以https://开头的URL。

在HTTP监听服务HTTPListen里，可以设置Script脚本程序，执行各种针对请求数据进行预判断和预处理的指令。这些脚本程序的执行时机是在收到完整的HTTP请求头后进行的。

eJet系统提供了动态库回调机制，使用动态库回调，既可以扩展eJet Web服务器能力，也可以将小型应用系统附着在eJet Web服务器上，处理客户端发起的HTTP请求。

HTTP监听服务HTTPListen下可管理多个虚拟主机HTTPHost，采用主机名称为索引主键的hashtab来管理下属的虚拟主机表。当当前监听服务的端口收到TCP请求和数据后，根据Host请求头的主机名称，来精确匹配定位出该请求的HTTP虚拟主机HTTPHost。


#### 4.1.3 HTTP虚拟主机 - HTTPHost

在HTTPListen监听服务下，可以配置多个虚拟主机，虚拟主机HTTPHost是eJet Web服务器资源管理体系的第二层，将HTTPCon缓冲区的数据进行解析，创建HTTPMsg来保存解析后的HTTP请求数据，HTTP协议规范中，请求头Host携带的值内容是URL中domain信息，所以HTTP虚拟主机HTTPHost，对应的就是请求域名，或者就是一个网站。一个监听服务HTTPListen下可以寄宿大量的通过虚拟主机HTTPHost来管理的网站。

HTTP虚拟主机的配置信息格式参考如下：
```
host = {
    host name = *; #www.ejetsrv.com
    type = server | proxy | fastcgi;
    gzip = on;

    ssl certificate = cert.pem;
    ssl private key = cert.key;
    ssl ca certificate = cacert.pem;

    script = {
        #reply 302 https://ke.test.ejetsrv.com:8443$request_uri;
        addResHeader X-Nat-IP $remote_addr;
    }

    error page = {
        400 = 400.html;
        504 = 504.html;
        root = /opt/ejet/errpage;
    }

    root = /home/hzke/sysdoc;

    location = {...}
    location = {...}
    location = {...}
}
```

HTTP虚拟主机的名称一般是域名格式，即多级名称体系，包含顶级域名、二级域名、三级域名等，通过DNS系统，将该域名解析到当前eJet Web服务器所在的IP地址上，如果在该IP地址上启动HTTPListen服务，那么所有使用该域名的请求都会指向到对应的HTTPHost虚拟主机。

eJet系统根据功能服务形式，对虚拟主机定义了几种类型：Server、Proxy、FastCGI等，这几种类型可以同时并存，可或在一起。

虚拟主机HTTPHost下可以设置资源的缺省目录，下属的资源位置HTTPLoc都可以复用虚拟主机的缺省目录。

如果当前虚拟主机HTTPHost的上级监听服务是建立在安全连接SSL上，那么在有多个网站即多个虚拟主机情况下，需要为每个网站配置属于该网站域名的证书、私钥等安全身份标识信息，客户端在向同一个监听服务发送请求后，采用TLS SNI机制和eJet中实现的SSL域名选择回调，来完成域名和证书的选择。

HTTPHost虚拟主机下可以设置Script脚本程序，虚拟主机下的脚本程序被执行时机是在创建HTTPMsg实例，并设置完DocURI后开始执行资源位置实例化流程，在该流程中分别执行HTTPListen的Script脚本、HTTPHost的Script脚本、HTTPLoc的Script脚本。脚本程序的执行按照上述优先级来进行，使用脚本程序的指令来预处理HTTP请求的各类数据。

一个虚拟主机HTTPHost下可以配置多个资源位置HTTPLoc，代表访问当前域名下的不同目录。虚拟主机HTTPHost采用多种方式管理下属的资源位置HTTPLoc实例，主要包括三种：
* 精确匹配请求路径的虚拟主机表 - 以请求路径名称为索引的资源位置索引表
* 对请求路径前缀匹配的虚拟主机表 - 以请求路径前缀名称为索引的资源位置字典树
* 对请求路径进行正则表达式运算的虚拟主机表 - 对正则表达式字符串为索引建立的资源位置列表

进入当前虚拟主机后，到底采用哪个资源位置HTTPLoc，匹配规则和顺序是按照上述列表的排序来进行的，首先根据HTTP请求的路径名在资源位置索引表中精准匹配，如果没有，则对请求路径名的前缀在资源位置字典树中进行匹配检索，如果还没有匹配上，最后对资源位置列表中的每个HTTPLoc，利用其正则表达式字符串，去匹配当前请求路径名，如果还是没有匹配的资源位置HTTPLoc，那么使用当前虚拟主机的缺省资源位置。

#### 4.1.4 HTTP资源位置 - HTTPLoc

HTTP资源位置HTTPLoc代表的是请求资源在某个监听服务下的某个虚拟主机里的目录位置，HTTPLoc代表的是请求路径，根据HTTPMsg中的客户端请求数据，最终基于各种资源匹配规则，找到HTTPListen、HTTPHost、HTTPLoc后，基本确定了当前请求的资源位置、处理方式等。一个网站对应的虚拟主机下，可以有多种功能和资源类别的资源位置HTTPLoc，如图像文件放置在image为根的目录下，PHP文件需要采用FastCGI转发给php-fpm解释器等。

HTTP资源位置的配置信息格式参考如下：
```
location = {
    type = server;
    path = [ "\.(h|c|apk|gif|jpg|jpeg|png|bmp|ico|swf|js|css)$", "~*" ];

    root = /opt/ejet/httpdoc;
    index = [ index.html, index.htm ];
    expires = 30D;

    cache_file = <script>
           if ($request_uri ~* 'laoke')
               return "${host_name}_${server_port}${req_path_only}${req_file_only}";
           else if (!-f $root$request_path) {
               return "$root$request_path is not a regular file";
           } else if (!-x $root$request_path) {
               return "$root$request_path is not an executable file";
           } else
               return "${request_header[host]}${req_path_only}else.html";
         </script>;
}

location = {
    path = [ '^/view/([0-9A-Fa-f]{32})$', '~*' ];
    type = proxy;
    passurl = http://cdn.ejetsrv.com/view/$1;

    root = /opt/cache/;
    cache = on;
    cache file = /opt/cache/${request_header[host]}/view/$1;
}

location = {
    type = fastcgi;
    path = [ "\.(php|php?)$", '~*'];

    passurl = fastcgi://localhost:9000;

    index = [ index.php ];
    root = /opt/ejet/php;
}

location = {
    path = [ '/' ];
    type = server;

    script = {
        try_files $uri $uri/ /index.php?$query_string;
    };

    index = [ index.php, index.html, index.htm ];
}
```

HTTP资源位置HTTPLoc是通过路径名path和匹配类型matchtype来作为其标识，路径名为配置中设置的名称，客户端请求的路径名通过匹配类型定义的匹配规则来跟设置的路径名进行匹配，如果符合匹配，则该请求使用此资源位置HTTPLoc。

匹配规则matchtype一般定义在配置文件中path数组里的第二项，分为如下几种：
* 精准匹配，使用等于号'='
* 前缀匹配，使用'^~'这两个符号
* 区分大小写的正则表达式匹配，使用'~'符号
* 不区分大小写的正则表达式匹配，使用'~*'这两个符号
* 通用匹配，使用'/'符号，如果没有其他匹配，任何请求都会匹配到

匹配的优先级顺序为：
  (location =) > (location 完整路径) > (location ^~ 路径) > 
  (location ~,~* 正则顺序) > (location 部分起始路径) > (/)

eJet系统根据功能服务形式，对资源位置HTTPLoc定义了几种类型：Server、Proxy、FastCGI等，通常情况下，一个资源位置HTTPLoc只属于一种类型。

HTTP资源位置HTTPLoc都需要一个缺省的根目录，指向当前资源所在的根路径，客户端请求的路径都是相对于当前HTTPLoc下的root跟目录来定位文件资源的。对于Proxy模式，根目录一般充当缓存文件的根目录，即需要对Proxy代理请求回来的内容缓存时，都保存在当前HTTPLoc下的root目录中。

每个HTTPLoc下都会有缺省文件选项，可以配置多个缺省文件，一般设置为index.html等。使用缺省文件的情形是客户端发起的请求只有目录形式，如`http://www.xxx.com/`，这时该请求访问的是HTTPLoc的根目录，eJet系统会自动地依次寻找当前根目录下的各个缺省文件是否存在，如果存在就返回缺省文件给客户端。不过需要注意的是，eJet系统中这个流程是在设置DocURI时处理的。

HTTP资源位置如果是Proxy类型或FastCGI类型，则必须配置转发地址passurl，转发地址passurl一般都为绝对URL地址，含有指向其他服务器的domain域名，passurl的形式取决HTTPLoc资源类型。

反向代理（Reverse Proxy）就是将HTTPLoc的资源类型设置为Proxy模式，通过设置passurl指向要代理的远程服务器URL地址，来实现反向代理功能。在反向代理模式下，passurl可以是含有匹配结果变量的URL地址，这个地址指向的是待转发的下一个Origin服务器，匹配变量如果为$1、$2等数字变量，即表示基于正则表达式匹配路径时，把第一个或第二个匹配字符串作为passurl的一部分。当然passurl可以包含任何全局变量或配置变量，使用这些变量可以更灵活方便地处理转发数据。

在反向代理模式下，HTTPLoc资源位置下有一个cache开关，如果设置cache=on即打开Cache功能，则需要在当前HTTPLoc下设置cachefile缓存文件名。对于不同的请求地址，cachefile必须随着请求路径或参数的变化而变化，所以cachefile的取值设置需要采用HTTP变量，或者使用Script脚本来动态计算cachefile的取值。

HTTPLoc下一般都会部署Script脚本程序，包括rewrite、reply、try_files等，根据请求路径、请求参数、请求头、源地址等信息，决定当前资源位置是否需要重写、是否需要转移到其他地址处理等。


### 4.2 HTTP变量

#### 4.2.1 HTTP变量的定义

HTTP变量是指在eJet Web服务器运行期间，能动态地访问HTTP请求、HTTP响应、HTTP全局管理等实例对象中的存储空间里的数据，或者访问HTTP配置文件的配置数据等等，针对这些存储空间的访问，而抽象出来的名称叫做HTTP变量。

变量的引用必须以$开头，后跟变量名，如果变量名后面还有连续紧随的其他字符串，则需用{}来包括住变量名，其基本格式为：$变量名称， ${变量名称}， ${ 变量名称 }，等等

#### 4.2.2 HTTP变量的应用

使用HTTP变量的场景主要在JSon格式的配置文件中，给各个配置项目增加动态的可编程接口，就需要基于不同的HTTP请求的信息，做判断、比较、赋值、拷贝、串接等操作，这些都离不开变量，需要不同的变量名去访问不同HTTP请求中的不同信息内容，通过配置中使用变量：访问变量的值，进行条件判断、比较、匹配、加减乘除、赋值等。变量的使用样例可参考如下：
```
access log = {
    log2file = on;
    log file = /var/log/access.log;
    format = [ '$remote_addr', '-', '[$datetime[stamp]]', '"$request"', '"$request_header[host]"',
               '"$request_header[referer]"', '"$http_user_agent"', '$status', '$bytes_recv', '$bytes_sent' ];
}

script = {
    reply 302 https://ke.test.ejetsrv.com:8443$request_uri;
}

cache file = /opt/cache/${request_header[host]}/view/$1;

params = {
    SCRIPT_FILENAME   = $document_root$fastcgi_script_name;
    QUERY_STRING      = $query_string;
    REQUEST_METHOD    = $request_method;
    CONTENT_TYPE      = $content_type;
    CONTENT_LENGTH    = $content_length;
}

script = {
    if ($query[fid])
        cache file = $real_path$query[fid]$req_file_ext;
    else if ($req_file_only)
        cache file = $real_path$req_file_only;
    else if ($query[0])
        cache file = ${real_path}${query[0]}$req_file_ext;
    else
        cache file = ${real_path}index.html;
}
```

#### 4.2.3 HTTP变量的类型和使用规则

eJet系统中，共定义了四种HTTP变量类型，分别为：
* 匹配变量 - 基于资源位置HTTPLoc模式串匹配HTTP请求路径时匹配串，通过数字变量来访问，如$1,$2等；
* 局部变量 - 由script脚本在执行过程中用set指令或赋值符号“=”设置的变量；
* 配置变量 - 配置文件中Listen、Host、Location下定义的JSon Key变量，以系统会使用到的常量定义为主；
* 参数变量 - 变量名称由系统预先定义、但值内容是在HTTPMsg创建后被赋值的变量，参数变量的值是只读不可写。

变量的使用规则符合高级语言的约定，对于同名变量，取值时优先级顺序为：
  $匹配变量 > $局部变量 > $配置变量 > $参数变量

HTTP变量的值类型是弱类型，根据赋值、运算的规则等上下文环境的变化，来确定被使用时变量是数字型、字符型等。除了匹配变量外，其他变量的名称必须是大小写字母和下划线_组合而成，其他字符出现在变量名里则该变量一定是非法无效变量。变量的定义很简单，前面加上美元符号$，后面使用变量名称，系统就会认为是HTTP变量。美元符号后的变量名称也可以通过大括号{}来跟跟其他字符串区隔。

如果变量的值内容包含多个，那么该变量是数组变量，数组变量是通过中括号[]和数字下标序号来访问数组的各个元素，如$query[1]访问是请求参数中的第一个参数的值。

匹配变量的名称为数字，以美元号$冠头，如$1,$2...，其数字代表的是使用HTTPLoc定义的路径模式串，去匹配当前HTTP请求路径时，被匹配成功的多个子串的数字序号。匹配变量的寿命周期是HTTPMsg实例化成功即准确找到HTTPLoc资源位置实例后开始，到HTTP响应被成功地发送到客户端后，HTTPMsg消息被销毁时为止。

局部变量的名称由字母和下划线组成，是script脚本在执行过程中用set指令或赋值符号“=”设置的变量，其寿命周期是从变量被创建之后到该HTTPMsg被销毁这段期间，而HTTPMsg则是用户HTTP请求到达时创建，成功返回Response后被摧毁。

配置变量是JSon格式的配置文件中定义的Key-Value对中，以Key为名称的变量，变量的值是设置的Value内容。在配置文件中位于Location、Host、Listen下定义的Key-Value赋值语句对，左侧为变量名，右侧为变量值，用$符号可以直接引用这些变量定义的内容；在Listen、Host、Location下定义的配置变量，主要是以系统中可能使用到的常量定义为主，这些常量定义也可以使用script脚本来动态定义其常量值，此外，用户可以额外定义系统配置中非缺省常量，我们称之为动态配置变量。

参数变量是系统预定义的有固定名称的一种变量类型，参数变量一般指向HTTP请求的各类信息、eJet系统定义的全局变量等。参数变量的名称是eJet系统预先定义并公布，但大部分变量的内容是跟HTTP请求HTTPMsg相关的，即不同的请求HTTPMsg，参数变量名的值也是随着变化的。一般要求，参数变量是只读不可写变量，即参数变量的值不能被脚本程序改变，只能读取访问。

#### 4.2.4 预定义的参数变量列表和实现原理

相比其他三种变量，参数变量是被使用最多、最有访问价值的变量，参数变量是系统预先定义的固定名称变量，变量的值是随着HTTP请求HTTPMsg的不同而不同。通过参数变量，配置文件中可以根据请求的信息，灵活动态地决定相关配置选项的赋值内容，从而扩展eJet服务器的能力，减少因额外功能扩展升级eJet系统的定制开销。

参数变量一般由eJet系统预先定义发布，其变量的值内容是跟随HTTP请求HTTPMsg的变化而变化，但变量名称是全局统一通用，所以参数变量也有时称为全局变量。

eJet系统预定义的参数变量如下：
* **remote_addr** - HTTP请求的源IP地址
* **remote_port** - HTTP请求的源端口
* **server_addr** - HTTP请求的服务器IP地址
* **server_port** - HTTP请求的服务器端口
* **request_method** - HTTP请求的方法，如GET、POST等
* **scheme** - HTTP请求的协议，如http、https等
* **host_name** - HTTP请求的主机名称
* **request_path** - HTTP请求的路径
* **query_string** - HTTP请求的Query参数串
* **req_path_only** - HTTP请求的只含目录的路径名
* **req_file_only** - HTTP请求路径中的文件名称
* **req_file_base** - HTTP请求路径中的文件基本名
* **req_file_ext** - HTTP请求路径中文件扩展名
* **real_file** - HTTP请求对应的真实文件路径名
* **real_path** - HTTP请求对应的真实文件所在目录名
* **bytes_recv** - HTTP请求接收到的客户端字节数
* **bytes_sent** - HTTP响应发送给客户端的字节数
* **status** - HTTP响应的状态码
* **document_root** - HTTP请求的资源位置根路径
* **fastcgi_script_name** - HTTP请求中经过脚本运行后的DocURI的路径名
* **content_type** - HTTP请求的内容MIME类型
* **content_length** - HTTP请求体的内容长度
* **absuriuri** - HTTP请求的绝对URI
* **uri** - HTTP请求源URI的路径名
* **request_uri** - HTTP请求源URI内容
* **document_uri** - HTTP请求经过脚本运行后的DocURI内容
* **request** - HTTP请求行
* **http_user_agent** - HTTP请求用户代理
* **http_cookie** - HTTP请求的Cookie串
* **server_protocol** - HTTP请求的协议版本
* **ejet_version** - eJet系统的版本号
* **request_header** - HTTP请求的头信息数组，通过带有数字下标或请求头名称的中括号来访问
* **cookie** - HTTP请求的Cookie数组，通过带有数字下标或Cookie名称的中括号来访问
* **query** - HTTP请求的Query参数数组，通过带有数字下标或参数名称的中括号来访问
* **response_header** - HTTP响应的头信息数组，通过带有数字下标或响应头名称的中括号来访问
* **datetime** - 系统日期时间数组，不带中括号是系统时间，带createtime或stamp的中括号则访问HTTPMsg创建时间和最后时间
* **date** - 系统日期数组，同上
* **time** - 系统时间，同上

随着应用场景的扩展，根据需要还可以扩展定义其他名称的参数变量。总体来说，使用上述参数变量，基本可以访问HTTP请求相关的所有信息，能满足绝大部分场景的需求。

系统中预定义的参数变量，都是指向特定的基础数据结构的某个成员变量，在该数据结构实例化后，其成员变量的地址指针就会被动态地赋值给预定义的参数变量，从而将地址指针指向的内容关联到参数变量上。

在设置预定义参数变量名时，一般需要设置关联的数据结构、数据结构的成员变量地址或位置、成员变量类型（字符、短整数、整数、长整数、字符串、字符指针、frame_t）、符号类型、存储长度等，eJet系统中维持一个这样的参数变量数组，分别完成参数变量数据的初始化，通过hashtab_t来快速定位和读写访问数组中的参数变量。
 
获取参数变量的实际值时，需要传递HTTPMsg这个数据结构的实例指针，根据参数变量名快速找到参数变量数组的参数变量实例，根据参数变量的信息，和传入的实例指针，定位到该实际成员变量的内存指针和大小，从内存中取出该成员变量的值。
 

### 4.3 HTTP Script脚本

#### 4.3.1 HTTP Script脚本定义

eJet系统在配置文件上扩展了Script脚本语言的语法定义，对JSon语法规范进行扩展，定义了一套符合JavaScript和C语言的编程语法，并提供Script脚本解释器，实现一定的编程和解释执行功能。

Script脚本是由一系列符合定义的语法规则而编写的代码语句组成，代码语句风格类似Javascript和C语言，每条语句由一到多条指令构成，并以分号;结尾。

#### 4.3.2 Script脚本嵌入位置

HTTP Script脚本程序的嵌入位置，共有两种。第一种嵌入位置是在配置文件的Listen、Host、Location下，通过增加JSon对象script，将脚本程序作为script对象的内容，来实现配置文件中嵌入脚本编程功能。在这种位置中，插入script脚本代码的语法共定义了三种：
```
  script = {....};
  script = if()... else...;
  <script> .... </script>
```
另外一种嵌入Script脚本程序的位置，是在JSon中的Key-Value对中，在Value里增加特殊闭合标签<script> Script Codes </script>，在标签里面嵌入Script脚本代码，执行完代码后返回的内容，作为Key的值，这种方式使得JSon规范中Key的值可以动态地由Script脚本程序计算得来。在Listen、Host或Location的常量赋值中，Value内容可以是script脚本，如
```
  cache file = <script> if ()... return... </script>
```

对adif 基础库中的json.c文件做了修改扩展，使得Json对象都能支持script脚本定义的这几种语法，如果某个对象下有名称为script的数据项，就认为该数据项下的Value值为脚本内容。这就将名称script作为Json的缺省常量名称了，使用时轻易不要使用script作为变量名。
 
#### 4.3.3 Script脚本范例

HTTP Script脚本程序示例如下：
```
 script = {
     if ($query[fid]) "cache file" = $req_path_only$query[fid]$req_file_ext;
     else if ($req_file_only) "cache file" = ${req_path_only}index.html;
     else "cache file" = $req_path_only$req_file_only; 
 }

 cache file = <script> if ($query[fid]) return $req_path_only$query[fid]$req_file_ext;
                        else if ($req_file_only) return ${req_path_only}index.html;
                        else return $req_path_only$req_file_only; 
              </script>

 <script>
     if ($query[fid]) "cache file" = $req_path_only$query[fid]$req_file_ext;
     else if ($req_file_only) "cache file" = ${req_path_only}index.html;
     else "cache file" = $req_path_only$req_file_only; 
 </script>

 <script>
     if ($scheme == "http://") rewrite ^(.*)$  https://$host$1;
 </script>
```

HTTP Script脚本程序的解释执行，是在创建HTTPMsg实例并设置完DocURI后，开始执行资源位置实例化流程，在实例化过程中，分别执行HTTPListen的Script脚本、HTTPHost的Script脚本、HTTPLoc的Script脚本。

#### 4.3.4 Script脚本语句

script脚本是由一系列语句构成的程序，语法类似于JavaScript和C语音，主要包括如下语句：
 
##### 4.3.4.1 条件语句

条件语句主要以if、else if、else组成，基本语法为：
```
  if (判断条件) { ... } else if (判断条件) { ... } else { ... }
```

判断条件至少包含一个变量或常量，通过对一个或多个变量的值进行判断或比较，取出结果为TRUE或FALSE，来决定执行分支，判断条件包括如下几种情况：
* (a) 判断条件中只包含一个变量；
* (b) 判断条件中包含了两个变量；
* (c) 文件或目录属性的判断；
 
判断比较操作主要包括：
* (a) 变量1 == 变量2，判断是否相等，两个变量值内容相同为TRUE，否则为FALSE
* (b) 变量1 != 变量2，判断不相等，两个变量值内容不相同为TRUE，否则为FALSE
* (c) 变量名，判断变量值，变量定义了、且变量值不为NULL、且变量值不为0，则为TRUE，否则为FALSE
* (d) !变量名，变量值取反判断，变量未定义，或变量值为NULL、或变量值为0，则为TRUE，否则为FALSE
* (e) 变量1 ^~ 变量2，变量1中的起始部分是以变量2开头，则为TRUE，否则为FALSE
* (f) 变量1 ~ 变量2，在变量1中查找变量2中的区分大小写正则表达式，如果匹配则为TRUE，否则为FALSE
* (g) 变量1 ~* 变量2，在变量1中查找变量2中的不区分大小写正则表达式，如果匹配则为TRUE，否则为FALSE
* (h) -f 变量，取变量值字符串对应的文件存在，则为TRUE，否则为FALSE
* (i) !-f 变量，取变量值字符串对应的文件不存在，则为TRUE，否则为FALSE
* (j) -d 变量，取变量值字符串对应的目录存在，则为TRUE，否则为FALSE
* (k) !-d 变量，取变量值字符串对应的目录存在，则为TRUE，否则为FALSE
* (l) -e 变量，取变量值字符串对应的文件、目录、链接文件存在，则为TRUE，否则为FALSE
* (m) !-e 变量，取变量值字符串对应的文件、目录、链接文件不存在，则为TRUE，否则为FALSE
* (n) -x 变量，取变量值字符串对应的文件存在并且可执行，则为TRUE，否则为FALSE
* (o) !-x 变量，取变量值字符串对应的文件不存在或不可执行，则为TRUE，否则为FALSE
 
##### 4.3.4.2 赋值语句

赋值语句主要由set语句构成，eJet系统中局部变量的创建和赋值是通过set语句来完成的。其语法如下：
```
  set $变量名  value;
```
 
##### 4.3.4.3 返回语句

返回语句也即是return语句，将script闭合标签内嵌入的Scirpt脚本代码执行运算后的结果，或Key-Value对中Value内嵌的脚本程序，解释执行后的结果返回给Key变量，基本语法为：
```
  return $变量名;
  return 常量;
```

其使用形态如下：
```
  cache file = <script> if ($user_agent ~* "MSIE") return $real_file; </script>;
```

##### 4.3.4.4 响应语句

响应语句也就是reply语句，执行该语句后，eJet系统将终止当前HTTP请求HTTPMsg的任何处理，直接返回HTTP响应给客户端，其语法如下：
```
  reply  状态码  [ URL或响应消息体 ];
```

如果返回的状态码是 444，则直接断开 TCP 连接，不发送任何内容给客户端。
 
调用Reply指令时，可以使用的状态码有：204，400，402-406，408，410, 411, 413, 416 与 500-504。如果不带状态码直接返回 URL 则被视为 302。其使用形态如下：
```
  if ($http_user_agent ~ curl) {
      reply 200 'COMMAND USER\n';
  }   
  if ($http_user_agent ~ Mozilla) {
      reply 302 http://www.baidu.com?$args;
  }      
  reply 404;
```
 
eJet系统在解释器解释执行Script代码时，先执行Listen下的script脚本、再执行Host下的script脚本，最后再执行Location下的script脚本。在执行下一个脚本之前，先判断刚刚执行的script脚本是否已经Reply了或者已经关闭当前HTTPMsg了。如果Reply了或关闭当前消息了，则直接返回，无需继续解析并执行后续的script脚本程序。
 
##### 4.3.4.5 rewrite语句

eJet系统中的URL重写是通过Script脚本来实现的，分别借鉴了Apache和Nginx的成功经验。

rewrite语句实现URL重写功能，当客户HTTP请求到达Web Server并创建HTTPMsg后，分别依次执行Listen、Host、Location下的script脚本程序，rewrite语句位于这些script脚本程序之中，rewrite语句会改变请求DocURL，一旦改变请求DocURL，在依次执行完这些script脚本程序之后，继续基于新的DocURL去匹配新的Host、Location，并继续依次执行该Host、Location下的script脚本程序，如此循环，是否继续循环执行，取决于rewrite的flag标记。
 
rewrite基本语法如下：
```
  rewrite regex replacement [flag];
```
 
执行该语句时是用regex的正则表达式去匹配DocURI，并将匹配到的DocURI替换成新的DocURI（replacement），如果有多个rewrite语句，则用新的DocURI，继续执行下一条语句。
 
flag标记可以沿用Nginx设计的4个标记外，还增加了proxy或forward标记。其标记定义如下：
* (a) last
    * 停止所有rewrite相关指令，使用新的URI进行Location匹配。
* (b) break
    * 停止所有rewrite相关指令，不再继续新的URI进行Location匹配，直接使用当前URI进行HTTP处理。
* (c) redirect
    * 使用replacement中的URI，以302重定向返回给客户端。
* (d) permament
    * 使用replacement中的URI，以301重定向返回给客户端。
* (e) proxy | forward
    * 使用replacement中的URI，向Origin服务器发起Proxy代理请求，并将Origin请求返回的响应结果返回给客户端。
 
由于reply语句功能很强大，rewrite中的redirect和permament标记所定义和实现的功能，基本都在reply中实现了，这两个标记其实没有多大必要。
 
rewrite使用案例如下：
```
rewrite  ^/(.*) https://www.ezops.com/$1 permanent;

rewrite ^/search/(.*)$ /search.php?p=$1?;
请求的URL: http://xxxx.com/search/some-search-keywords
重写后URL: http://xxxx.com/search.php?p=some-search-keywords

rewrite ^/user/([0-9]+)/(.+)$ /user.php?id=$1&name=$2?;
请求的URL: http://xxxx.com/user/47/dige
重写后URL: http://xxxx.com/user.php?id=47&name=dige

rewrite ^/index.php/(.*)/(.*)/(.*)$ /index.php?p1=$1&p2=$2&p3=$3?;
请求的URL: http://xxxx.com/index.php/param1/param2/param3
重写后URL: http://xxxx.com/index.php?p1=param1&p2=param2&p3=param3

rewrite ^/wiki/(.*)$ /wiki/index.php?title=$1?;
请求的URL：http://xxxx.com/wiki/some-keywords
重写后URL：http://xxxx.com/wiki/index.php?title=some-keywords

rewrite ^/topic-([0-9]+)-([0-9]+)-(.*)\.html$ viewtopic.php?topic=$1&start=$2?;
请求的URL：http://xxxx.com/topic-1234-50-some-keywords.html
重写后URL：http://xxxx.com/viewtopic.php?topic=1234&start=50

rewrite ^/([0-9]+)/.*$ /aticle.php?id=$1?;
请求的URL：http://xxxx.com/88/future
重写后URL：http://xxxx.com/atricle.php?id=88
```

在eJet系统中，replacement后加？和不加？是有差别的，加？意味着query参数没了，不加则会自动把源URL中的query串（?query）添加到替换后的URL中。
 
##### 4.3.4.6 addReqHeader语句

特定情况下，需要对客户端请求消息添加额外的请求头，交给后续处理程序，如应用层处理程序、PHP程序、Proxy、Origin服务器等等，来处理或使用到这些信息。譬如在作为HTTP Proxy功能时，发送给远程Origin服务器的请求中都需要添加两个请求头：一个是X-Real-IP，另一个是X-Forwarded-For，使用本语句可以很方便地实现了。
 
其基本语法为：
```
  addReqHeader  <header name>  <header value>;
```
<header name>不能是空格字符，以字母开头后跟字母、数字和下划线_的字符串，可以用双引号圈定起来；
<header value>是任意字符串，可以以引号包含起来，字符串中可包含变量。
 
使用案例如下：
```
if ($proxied) {
    addReqHeader X-Real-IP $remote_addr;
    addReqHeader X-Forwarded-For $remote_addr;
}
```
 
##### 4.3.4.7 addResHeader语句

其基本语法为：
```
 addResHeader  <header name>  <header value>;
``` 
 
##### 4.3.4.8 delReqHeader语句

其基本语法为：
```
 delReqHeader  <header name>;
```
 
##### 4.3.4.9 delResHeader语句

其基本语法为：
```
  delResHeader  <header name>;
``` 

##### 4.3.4.10 try_files 语句

try_files 是一个重要的指令，建议位于Location、Host下面。使用该指令，依次测试列表中的文件是否存在，存在就将其设置DocURI，如不不存在，则将最后的URI设置为DocURI，或给客户端返回状态码code。
 
try_files基本语法如下：
```
  try_files file ... uri;
或
  try_files file ... =code;
``` 

##### 4.3.4.11 注释语句

Script脚本程序中，如果一行除去空格字符外，以#号打头，那么当前行为注释行，不被解释器解释执行；另外通过C语言代码块注释标记 /*  xxx  */也被eJet系统采用。


#### 4.3.5 Script脚本解释器

eJet系统在处理HTTPMsg的实例化过程中，成功定位到HTTPHost、HTTPLoc等资源位置后，开始解释执行这三个层级资源管理框架下的脚本程序，执行的顺序依次为HTTPListen、HTTPHost、HTTPLoc下的Script脚本程序。

eJet系统的Script解释器是逐行逐字进行扫描和识别，提取出Token后，分别匹配上述语句指令，再递归根据各个语句的扫描、识别和处理。这里细节不做描述！


### 4.4 JSon格式的系统配置文件

#### 4.4.1 JSON语法特点

JSON的全称是JavaScript Object Notation，是一种轻量级的数据交换格式。JSON的文本格式独立于编程语言，采用name:value对存储名称和数据，可以保存数字、字符串、逻辑值、数组、对象等数据类型，是理想的数据交换语法格式，简洁干练，易于扩展、阅读和编写，也便于程序解析和生成。

正是由于JSon语法的简单和强扩展性、采用可保存各种数据类型的name/value对语法、可嵌套JSON子对象等特性，与配置文件的配置属性特别吻合，所以，eJet系统使用JSon格式来保存、传递、解析系统配置文件。

#### 4.4.2 eJet配置文件对JSON的扩展

##### 4.4.2.1 分隔符

eJet系统使用adif中的JSon库来解析、访问配置文件信息。JSon语法缺省格式以冒号(:)来分隔name和value，以单引号(')或双引号(")来包含name和value串，以逗号(,)作为name/value对的分隔符，以中括号[]表示数组，以大括号{}表示对象。

eJet系统采用JSon作为配置文件语法规范，为了兼容传统配置文件的编写习惯，将JSon基础语法做了一些扩展，即分隔name与value的冒号(:)换成等于号(=)，分隔name/value对之间的逗号(,)换成分号(;)，其他基础语法不变。

##### 4.4.2.2 include指令

由于配置信息数据较大，需要使用不同的文件来保存不同的配置信息，借鉴C语言/PHP语言的include宏指令，eJet系统的JSon语法引入了include指令。扩展语法中将把"include"作为JSon语法的关键字，不会被当做对象名称和值内容来处理，而是作为嵌入另外一个文件到当前位置进行后续处理的特殊指令。其语法规范如下：
```
include <配置文件名>;
```
解析JSon内容时，如果遇到include指令，就将include指令后面的文件内容加载到当前指令位置，作为当前文件内容的一部分，进行解析处理。

##### 4.4.2.3 单行注释和多行注释

为了增加配置文件中代码的可读性，需要对相关的定义添加详细说明、注解等内容，方便使用人员快速阅读和理解。

为支持注释功能，eJet系统的配置文件对JSON语法做了相应扩展，增加了单行注释符号#和多行注释(/* */)，其语法规范如下：
```
# 这是单行注释，如果井号(#)不在JSon某个Key-Value对的引号里面，那么以井号开头，井号后面的内容都是注释

/* 注意：多行注释是以连在一起的/和*开始
         以连在一起的*和/结尾，中间的内容都是注释
   多行注释开闭符号，必须不能在Key-Value对的引号里面
 */
```
注释的内容在解析时直接忽略跳过，不会被系统解析和处理。

##### 4.4.2.4 script语法

使用JSON格式的数据都是由name/value对构成，eJet系统中需要在配置文件中支持Script脚本程序，灵活动态地处理HTTP请求。

eJet配置文件对JSON语法格式扩展了一种固定名称的script对象，将名称"script"作为特殊对象的名称关键字，即以script为名称的对象，其内容不能作为JSON子对象处理，而是作为Script脚本程序内容，存放在对象名为script的对象中。其语法规范如下：
```
script = {
    if ($request_uri ~* '^/topic/[0-9](*)/(.*)\.mp4$') {
        set $video_flag 1;
    }
};
```
在同一个JSon对象下，可以有多个script对象，自动构成script对象数组。

另外，使用特殊的开闭标签<script>和</script>，也可以定义脚本程序。在这两个开闭标签中间的内容，即是Script脚本程序，并将这些内容存储到配置文件定义的任意name名称对象中，其语法规范如下：
```
cache file = <script>
       if ($request_uri ~* 'laoooke')
           return "${host_name}_${server_port}${req_path_only}${req_file_only}";
       else if (!-f $root$request_path) {
           return "${host_name}_${server_port}${req_path_only}${index}";
       } else if (!-x $root$request_path) {
           return "$root$request_path is not an executable file";
       } else
           return "${request_header[host]}${req_path_only}else.html";
     </script>;
```
这样，"cache file"对象的内容就是一段脚本程序，需要在解释执行到这里时，才真正具有实际数据。

#### 4.4.3 eJet配置文件

基于对JSon的扩展，eJet系统使用了JSon风格的配置文件，下面是一个实际部署eJet系统的配置文件样例：
```
/* max FD number allowed open in a process */
rlimit nofile =  65535;
 
pid lock file = /var/lock/subsys/ejet.pid;
 
epump = {
    event notification = epoll; #select
    epump threads = 3;
    worker threads = 20;
}
 
http = {
    /* ! * ' ( ) ; : @ & = + $ , / ? # [ ] */
    url not escape char = "-_.~!*'();:@&=+$,/?#][";
 
    cookie file = ./cookie.txt;
 
    include mime.types;
    include 8091_upload.conf;
 
    access log = {
        log2file = on;
        log file = /var/log/ejet-access.log;
 
        format = [ '$remote_addr', '-', '[$datetime[stamp]]', '"$request"', 
                   '"$request_header[host]"', '"$request_header[referer]"', '"$http_user_agent"',
                   '$status', '$bytes_recv', '$bytes_sent'
                 ];
    }
 
    receive request = {
        max header size = 32K;
 
        #if body-length is greater thant it, body will be saved to cache file
        body cache = on;
        body cache threshold = 64K;  
 
        keepalive timeout = 10;
        connection idle timeout = 10;
        header idle timeout = 10;
        header timeout = 30;
        request handle timeout = 180;
    } 
 
    send request = {
        max header size = 32K;
 
        connecting timeout = 8;
        keepalive timeout = 3;
        connection idle timeout = 180;
 
        /* if request is sending by https:// */
        ssl certificate = cert.pem;
        ssl private key = cert.key;
        ssl ca certificate = cacert.pem;
 
        /* if request HTTPMsg existing Location, Location->root is in place */
        root = /opt/ejet;
        cache = on;
        cache file = <script>
                       if ($req_file_only)
                           return "${host_name}_${server_port}${req_path_only}${req_file_only}";
                       else
                           return "${host_name}_${server_port}${req_path_only}index.html";
                     </script>;
 
        /* next proxy host and port when sending http request */
        proxy setting = {
            /* left-side is regular express to request host:port, right-side is proxy host and port */
            ^(.+)sina.com.cn$ = 214.147.4.5:8080;
        };
    } 
 
    proxy = {
        connect tunnel = on;
        tunnel keepalive timeout = 30;
        auto redirect = on;
 
        buffer size = 256k;
    }
 
    listen = {
        local ip = *;
        port = 443;
        forward proxy = off;
 
        ssl = on;
        ssl certificate = cert.pem;
        ssl private key = cert.key;
        ssl ca certificate = cacert.pem;
 
        host = {
            host name = *; #www.downsha.com
            type = server;
 
            location = {
                type = server;
                path = [ "(\.(.+)$)|/", "~*" ];
 
                root = /opt/ejet;
                index = [ index.html, index.htm ];
                expires = 30D;
            }
        }
    }
 
    listen = {
        local ip = *;
        port = 80;
        forward proxy = on;
 
        #request process library = reqhandle.so
 
        script = {
            addResHeader X-Nat-IP $remote_addr;
        }
 
        host = {
            host name = cache1.cdn.yunzhai.cn;  #DNS dynamically resolving
 
            location = {
                type = proxy;
                path = [ "(\.(.+)$)|/", "~*" ];
 
                passurl = http://cdn.yunzhai.cn;  #origin server
                root = /opt/ejet;
                cache = on;

                script = {
                    if ($query[fid])
                        cache file = $real_path$query[fid]$req_file_ext;
                    else if ($req_file_only)
                        cache file = $real_path$req_file_only;
                    else if ($query[0])
                        cache file = ${real_path}index.html;
                    else
                        cache file = ${real_path}index.html;
                };
                <script>
                    rewrite ^(.*)\.php$ http://vcloud.yunzhai.cn/cdn/$1 forward;
                </script>;
            }
        }
 
        host = {
            host name = *; #www.downsha.com
            type = server | proxy | fastcgi;
            gzip = on;
            root = /data/webroot/wordpress;
 
            error page = {
                400 = 400.html; 401 = 401.html; 402 = 402.html; 403 = 403.html;
                404 = 404.html; 405 = 405.html; 406 = 406.html; 500 = 500.html;
                501 = 501.html; 502 = 502.html; 503 = 503.html; 504 = 504.html;
                root = /opt/ejet/errpage;
            }
 
            /*  =   表示精确匹配
                ' ' 空格开头，表示以该字符串为前缀的匹配，不是正则匹配
                ^~  表示uri以某个常规字符串开头，不是正则匹配
                ~   表示区分大小写的正则匹配;
                ~*  表示不区分大小写的正则匹配
                /   通用匹配, 如果没有其它匹配,任何请求都会匹配到
 
                匹配的优先级顺序为：
                    (location =) > (location 完整路径) > (location ^~ 路径) > 
                    (location ~,~* 正则顺序) > (location 部分起始路径) > (/)
             */
 
            location = {
                type = server;
                path = [ "\.(h|c|apk|gif|jpg|jpeg|png|bmp|ico|swf|js|css)$", "~*" ];
 
                root = /data/webroot/httpdoc;
                index = [ index.html, index.htm ];
            }
 
            location = {
                type = fastcgi;
                path = [ "\.(php|php?)$", '~*'];
 
                passurl = unix:/run/php-fpm/www.sock;
                index = [ index.php ];
            }
 
            location = {
                path = [ '/admin/', '^~' ];
                type = proxy;
                passurl = http://admin.doansha.com/;
 
                script = {
                    rewrite ^/xxx/([0-9]+)/.*$ /pump.so?id=$1;
                };
            }
 
            location = {
                path = [ '^/view/([0-9A-Fa-f]{32})$', '~*' ];
                type = proxy;
                passurl = http://cdn.yunzhai.cn/view/$1;
 
                script = {
                    addReqHeader X-Forwarded-For $remote_addr;
                    addReqHeader X-Real-IP2 $remote_addr;
                };

                root = /opt/cache/;
                cache = on;
                cache file = /opt/cache/${request_header[host]}/view/$1;
            }
 
            location = {
                path = [ '/topic-([0-9]+)-([0-9]+)-(.*)\.html$', '~*' ];
                type = proxy;
                passurl = https://ke.test.ejetsrv.com/bbs.so?topic=$1&start=$2;
            }
 
            location = {
                path = [ '/' ];
                type = server;
 
                script = {
                    try_files $uri $uri/ /index.php?$query_string;
                };
 
                #root = .;
                index = [ index.php, index.html, index.htm ];
            }
        }
    }
 
    fastcgi = {
        connecting timeout = 10;
        keepalive timeout = 30;
        connection idle timeout = 90; 
        fcgi server alive timeout = 120;
 
        buffer size = 256k;
 
        params = {
            SCRIPT_FILENAME   = $document_root$fastcgi_script_name;
            QUERY_STRING      = $query_string;
            REQUEST_METHOD    = $request_method;
            CONTENT_TYPE      = $content_type;
            CONTENT_LENGTH    = $content_length;
 
            SCRIPT_NAME       = $fastcgi_script_name;  #脚本名称   
            REQUEST_URI       = $request_uri;          #请求的地址带参数  
            DOCUMENT_URI      = $document_uri;         #不带Query参数
            DOCUMENT_ROOT     = $document_root;        #在Location配置中root的值   
            SERVER_PROTOCOL   = $server_protocol;      #协议HTTP/1.0或HTTP/1.1
            REQUEST_SCHEME    = $scheme;
 
            
            GATEWAY_INTERFACE = CGI/1.1;               # FastCGI/1.0;           #CGI/1.1  cgi 版本  
            SERVER_SOFTWARE   = ejet/$ejet_version;    #ejet 版本号，可修改、隐藏  
            
            REMOTE_ADDR       = $remote_addr;          #客户端IP  
            REMOTE_PORT       = $remote_port;          #客户端端口  
            SERVER_ADDR       = $server_addr;          #服务器IP地址  
            SERVER_PORT       = $server_port;          #服务器端口  
            SERVER_NAME       = $host_name;            #服务器名，在Host配置中指定的host name
 
            # PHP only, required if PHP was built with --enable-force-cgi-redirect  
            REDIRECT_STATUS   = 200;  
        }
    };
 
    gzip = {
        min length = 1k;
        buffer = 64k;
        compress level = 2;
        http version = 1.1;
        types = [ text/plain, application/x-javascript, text/css, application/xml ];
        vary = on;
    }
}
```

### 4.5 事件驱动流程 http_pump

eJet系统是建立在ePump框架之上的Web服务器，eJet是ePump的事件驱动框架（Event-Driven Architecture）的回调应用。ePump框架基于事件监听机制，产生网络监听事件、网络读事件、网络写事件、连接成功事件、定时器超时事件等，并将事件均衡派发到各个工作线程中，通过工作线程来回调eJet的处理函数，这种通过设备监听、产生事件、以事件驱动工作线程来回调应用接口函数的流程，就是事件驱动架构模型。

驱动eJet工作的ePump事件包括如下：
```
/* event types include getting connected, connection accepted, readable,
 * writable, timeout. the working threads will be driven by these events */
#define IOE_CONNECTED        1
#define IOE_CONNFAIL         2
#define IOE_ACCEPT           3
#define IOE_READ             4
#define IOE_WRITE            5
#define IOE_INVALID_DEV      6
#define IOE_TIMEOUT          100
#define IOE_USER_DEFINED     10000
```

eJet系统中处理这些事件的回调函数是http_pump，其原型如下：
```
int http_pump (void * vmgmt, void * vobj, int event, int fdtype)
```

其中fdtype是产生这些事件的文件描述符类型、定时器等，其定义如下：
```
/* the definition of FD type in the EventPump device */
#define FDT_LISTEN            0x01
#define FDT_CONNECTED         0x02
#define FDT_ACCEPTED          0x04
#define FDT_UDPSRV            0x08
#define FDT_UDPCLI            0x10
#define FDT_USOCK_LISTEN      0x20
#define FDT_USOCK_CONNECTED   0x40
#define FDT_USOCK_ACCEPTED    0x80
#define FDT_RAWSOCK           0x100
#define FDT_FILEDEV           0x200
#define FDT_TIMER             0x10000
#define FDT_USERCMD           0x20000
#define FDT_LINGER_CLOSE      0x40000
#define FDT_STDIN             0x100000
#define FDT_STDOUT            0x200000
```

eJet系统没有创建任何线程和进程，却能充分利用CPU执行全部HTTP请求和响应的所有流程，完全是被动的，即被这些事件所驱动。

启动ePump的事件驱动，首先需要根据eJet系统的需求，调用ePump框架提供的API，创建相应的iodev_t设备对象和iotimer_t定时器对象，只有这些对象才会被ePump框架监控和触发，从而产生相应的事件，驱动eJet工作。

eJet系统中调用ePump创建事件源的几个API函数如下：
```
/* Note: automatically detect if Linux kernel supported REUSEPORT. 
   if supported, create listen socket for every current running epump threads
   and future-started epump threads.
   if not, create only one listen socket for all epump threads to bind. */
void * eptcp_mlisten (void * vpcore, char * localip, int port, void * para,
                      IOHandler * cb, void * cbpara);

void * eptcp_accept (void * vpcore, void * vld, void * para, int * retval,
                     IOHandler * cb, void * cbpara, int bindtype);
 
void * eptcp_connect (void * vpcore, char * ip, int port,
                      char * localip, int localport, void * para,
                      int * retval, IOHandler * cb, void * cbpara);

void * iotimer_start (void * vpcore, int ms, int cmdid, void * para,
                      IOHandler * cb, void * cbpara);
```

eJet系统完全依赖于ePump极其高效的多线程调度机制，充分利用ePump框架对多核CPU并行处理的调用，使得eJet系统具备支撑大并发、大容量访问的物理基础。

### 4.6 HTTP请求和响应

HTTP请求和响应构成HTTP消息，从客户端发送给服务器端的HTTP消息是HTTP请求，从服务器端到客户端的消息是HTTP响应。HTTP请求和响应的消息格式包括一个起始行、0个或者多个Header字段、一个空行、可能存在的消息体。

#### 4.6.1 HTTP请求格式

按照[RFC 2616](https://datatracker.ietf.org/doc/rfc2616/?include_text=1)规范，HTTP请求消息包括请求行、请求头、消息体，具体格式如下：
```
  Method SP Request-URI SP HTTP-Version CRLF
  *(( general-header | request-header | entity-header ) CRLF)
  CRLF
  [ message-body ]
其中
  Method = "OPTIONS" | "GET" | "HEAD" | "POST" | "PUT" | "DELETE" | "TRACE" | "CONNECT"
  Request-URI    = "*" | absoluteURI | abs_path | authority
  HTTP-Version   = "HTTP" "/" 1*DIGIT "." 1*DIGIT
  http_URL = "http:" "//" host [ ":" port ] [ abs_path [ "?" query ]]
```
HTTP请求消息的三部分：请求行、请求头、消息体，前两部分都是纯文本内容，头与体之间由一个空行\r\n隔开。请求行由三部分构成：请求方法、请求URI地址、协议版本。请求URL地址是标识资源位置的定位符，其基本格式为：<协议>://<主机>:[端口]/<路径>?[参数]

#### 4.6.2 HTTP响应格式

按照RFC 2616规范，HTTP响应消息包括状态行、响应头、响应体，具体格式如下：
```
  HTTP-Version SP Status-Code SP Reason-Phrase CRLF
  *(( general-header | response-header | entity-header ) CRLF)
  CRLF
  [ message-body ]
其中
  HTTP-Version   = "HTTP" "/" 1*DIGIT "." 1*DIGIT
```
响应行中的状态码是由3个整型数字构成，状态原因短语是对状态码的简要文字描述。状态码中的第一位代表响应状态的类别，后两位是此类状态的原因，共有五类状态：
* 1xx - 信息，表示请求已收到，继续处理中
* 2xx - 成功，表示请求已成功接收、理解和处理
* 3xz - 重定向，表示请求需要导向到其他地址处理
* 4xx - 客户侧错误，表示请求语法错误、或不能正确理解
* 5xx - 服务侧错误，表示服务器未能处理请求

#### 4.6.3 头和体的解析与编码

eJet从对方接收HTTP请求或响应的字节流时，首先需要解析起始行、头信息，根据这些信息，再判断消息体内容是否存在，如果存在其大小和格式是什么。

根据上述规范，在头和体之间有一个空行，加上前面的换行符号，这四个字符\r\n\r\n就是头部区域的结尾，eJet当作是头部结尾符。解析过程首先要做的是搜索接收缓冲区字节流是否存在这四个字符，如果不存在，就继续等待接收数据到缓冲区，直到遇到这四个结尾符。这个过程需要做错误判断，如果接收缓冲区总长度超过32K字节，还没有收到头部结尾符，则任务客户端发起错误的、恶意的HTTP请求，直接关闭连接。

找到结尾符后，如果不存在HTTPMsg实例，先创建HTTPMsg实例，对结尾符之前的字节流内容按照上述规范进行解析，并保存到HTTPMsg对象成员中，对于HTTP请求，则包括请求方法、请求URI地址、协议版本、所有的请求头等，对于HTTP响应，包括协议版本、状态码、状态原因、响应头等，解析成功后，需分析和预处理这些数据，进行合规性校验和判断。

HTTPMsg对头信息的管理是采用hashtab和动态数组arr_t，方便快速定位和遍历，每个头信息基础数据结构为HeaderUnit，保存的是偏移地址和长度，并没有为每个头分配空间，以防止大量的内存碎片。

HTTPMsg在处理中，eJet系统或上层回调函数都可以根据需要，添加额外的请求头和响应头，发送给对方。

编码过程主要是对HTTPMsg中保存的起始行、动态数组中的头信息按照规范生成以\r\n分隔的文本格式的字节流。

对消息体的解析和编码比较复杂，消息体格式分成三个层级，第一层是传输编码，第二层是内容编码，第三层内容类型编码。

传输编码一般采用Content-Length头来标识的消息体Entity内容的长度，或采用Transfer-Encoding: chunked方法对消息体进行切片分块编码，这两种方法是HTTP/1.1中对消息体在传输之前采用的标准传输方式。

Content-Length方式比较简单，将实际消息体长度指定到头中，消息体内容跟在编码完的头信息之后，顺序传输给对方。

Transfer-Encoding传输编码的样例格式如下：
```
Chunk format as following:
 24E5CRLF            #chunk-sizeCRLF  (first chunk begin)
 24E5(byte)CRLF      #(chunk-size octet)CRLF
 38A1CRLF            #chunk-sizeCRLF  (another chunk begin)
 38A1(byte)CRLF      #(chunk-size octet)CRLF
 ......              #one more chunks may be followed
 0CRLF               #end of all chunks
 X-bid-for: abcCRLF  #0-more HTTP Headers as entity header
 .....               #one more HTTP headers with trailing CRLF
 CRLF
```
对Transfer-Encoding: Chunk模式的解析，eJet系统是用HTTPChunk数据结构和函数来实现的。

传输过程中，最棘手的问题是收到的消息内容不完整，而且内存不够大时，这些内容分别存储在不同的地方，如有些存储在内存中，有些存储在缓存文件中。

### 4.7 HTTPMsg的实例化流程

#### 4.7.1 什么是HTTPMsg实例化

eJet接受客户端发起的TCP连接，接收该连接上的HTTP请求数据，解析HTTP请求头，创建HTTPMsg来保存请求数据后，需要理解客户端发送HTTP请求的目的，即确定HTTP请求的资源在哪个虚拟主机下的那个存储位置，这个过程就是HTTPMsg的实例化流程。

如4.1中所述，eJet Web服务器的资源管理结构分成三层：
* **HTTP监听服务HTTPListen** - 对应的是监听本地IP地址和端口后的TCP连接
* **HTTP虚拟主机** - 对应的是请求主机名称domain
* **HTTP资源位置HTTPLoc** - 对应的是主机下的各个资源目录
 
一个eJet Web服务器可以监听本机的一个或多个IP地址、一个或多个端口，等候不同客户端的TCP连接请求，分别对应到多个监听服务HTTPListen；在每一个监听服务下，可以配置一个到多个HTTP虚拟主机HTTPHost，每个虚拟主机对应的是一个网站，管理不同类别的文件资源、网络资源等位置信息HTTPLoc；每个资源位置包含具体的文件存储路径，或网络地址等信息。

#### 4.7.2 匹配虚拟主机和资源位置

HTTPMsg的实例化是以DocURI地址的信息来匹配虚拟主机和资源位置的，DocURI的缺省地址是客户端发起的HTTP请求URI。

HTTPMsg的实例化过程中改变的地址是DocURI，再次匹配虚拟主机和资源位置的也是DocURI的信息。对用户请求URI进行资源定位过程中，由于补足资源目录下的缺省文件名、或使用rewrite、try_files等指令改变请求地址等操作，都会改变DocURI。

当eJet接受客户端连接时创建HTTPCon，并绑定监听服务HTTPListen实例，当接收到请求数据后，创建HTTPMsg，并将该连接上的HTTPListen实例传递到的HTTPMsg对象保存。

HTTPMsg根据DocURI的主机名称，查找当前HTTPListen下的主机表，用Hashtab的精准匹配，找到HTTPHost虚拟主机实例对象。

绑定了HTTPHost后，使用DocURI的路径名，分别采用路径名进行精准匹配、前缀匹配、正则表达式匹配三种匹配规则，找到资源位置HTTPLoc，如果三种匹配都没有匹配到，则采用缺省的资源位置HTTPLoc。

#### 4.7.3 执行脚本程序

HTTPMsg实例对象设置了三个层级的资源对象后，分别读取各自的脚本程序，解释并执行这些程序代码。

执行脚本程序的优先级是: 首先执行HTTPListen监听服务下的脚本程序，其次执行HTTPHost虚拟主机下的脚本程序，最后执行HTTPLoc资源位置下的脚本程序。

脚本程序执行过程中，如果调用Reply指令直接给客户端返回响应，那么终止当前所有的Script脚本运行，退出实例化过程，并完成响应的发送后，终止当前请求服务。

如果执行脚本时，调用了rewrite、try_files指令，并且重新改写了DocURI，则会出现HTTPMsg实例化过程的嵌套执行，即重新执行4.7.2节描述的重新匹配虚拟主机和资源位置，并执行新的脚本程序。需注意的是，eJet系统中HTTPMsg实例化过程中，递归嵌套次数不超过16次。

脚本程序执行期间，可根据请求信息（如IP地址、终端类型、特定请求头、请求目的URL等），利用各种脚本指令，动态设置或改变成员变量值或相关属性。

### 4.8 HTTP MIME管理

媒体类型MIME的全称为Multipurpose Internet Mail Extensions，是表示文档、文件或字节流的性质和格式的标准，其标准规范定义在[RFC 6838](https://tools.ietf.org/html/rfc6838)里。

Web服务器是基于MIME类型来识别和处理文档类型的，不同于操作系统中的文件扩展名。不过国际机构IANA在分配和管理MIME类型时，对MIME类型和扩展名之间建立了对应关系。

MIME使用文本字符串来标识类型，对大小写不敏感。MIME的语法如下：
```
    type/subtype
```
其中type表示文档类型的主类别，subtype是该主类别下的细分子类别。

常用的MIME类型的主类别主要包括如下：
| 类型 | 描述 | 示例 |
| ---- | ---- | ---- |
| text | 普通文本 | text/plain, text/html, text/css, text/javascript |
| image | 图像格式 | image/png, image/jpeg, image/gif |
| audio | 音频格式 | audio/mp3, audio/aac, audio/mpeg |
| video | 视频格式 | video/mp4, video/mpeg2-ts, video/ogg |
| application | 二进制数据 | application/octet-stream |

eJet系统中对MIME类型数据结构进行了定义和管理，并将IANA定义的1000多种MIME类型进行了初始化，并为每个MIME类型单独分配了一个数字ID和APPID，并和OS的文件扩展名之间建立了对应关系。
```c
typedef struct mime_item {
    uint32    mimeid; 
    uint32    appid;
    char      extname[16]; 
    char      mime[96]; 
} MimeItem; 

    { 2000, 1000, ".gif", "image/gif" },
    { 2010, 1010, ".jpg", "image/jpeg" },
    { 2011, 1010, ".jpeg", "image/jpeg" },
    { 2012, 1010, ".jpe", "image/jpeg" },
    { 2013, 1010, ".jfif", "image/jpeg" },
    { 2014, 1010, ".jfi", "image/jpeg" },
    { 2015, 1010, ".jpg", "image/pjpeg" },
    { 2016, 1010, ".jpg", "image/jpg" },
    { 2020, 1020, ".png", "image/png" },
    { 2021, 1020, ".x-png", "image/png" },
    { 2022, 1020, ".png", "image/x-png" },
       ......
```

如果应用系统中自定义了新的MIME类型，在缺省MIME中不存在，则可以在系统配置文件中，在http.mime types配置对象下，增加新的MIME类型，方便eJet系统识别和理解，新增加的MIME配置信息如下：
```
mime types = {
    application/vnd.wap.wmlc                       = .wmlc;
    application/x-perl                             = .pl .pm;
    ......
};
```
eJet系统自动地将系统配置文件中定义的MIME类型和缺省定义的MIME类型整合在一起，并提供统一的获取MIME类型的接口。在根据扩展名获取MIME类型时，优先获取配置文件中的MIME类型，在配置文件没有定义时，获取系统缺省定义的MIME类型。

eJet系统根据HTTP请求，在其资源位置对应的目录体系下，找到要返回给客户端的资源文件前，根据文件扩展名在MIME管理系统中，找到对应的MIME类型，或者没有扩展名时，根据系统命令file确定文件的MIME类型。


### 4.9 HTTP URI管理

URI是Uniform Resource Identifiers的缩写，以一种简单而又可扩展的方式来定位资源。URI规范是由[RFC 2396](https://tools.ietf.org/html/rfc2396)来定义和描述的。

HTTP URI是以HTTP协议来定位网络资源的URI格式，基于语法如下：
```
http_URL = "http: | https:" "//" host [ ":" port ] [ abs_path [ "?" query ]]
```

如果port不存在，则在http模式下port缺省为80，在https模式下port缺省为443。

在eJet系统中，HTTP URI定位资源是通过三层资源架构来实现的，即HTTPListen、HTTPHost、HTTPLoc。

在创建HTTPMsg保存请求信息时，使用了三个成员来保存HTTP URI，即RequestURI、DocURI、AbsURI，客户端发出的请求URI、经过实例化处理后的文档URI、绝对URI地址。

eJet作为HTTP Server时，HTTP请求URI是相对URI地址，即不包含HTTP完整URI中的scheme、domain、port部分，只有abs_path和query部分，domain和port信息包含在HTTP请求头Host中。

eJet作为HTTP Proxy时，注意是正向代理，HTTP请求URI是绝对地址，以http打头的地址。

HTTPMsg中，通过字段req_url_type来标识当前请求的地址类型，0-相对地址，1-绝对地址。

无论是相对地址还是绝对地址，eJet系统在解析HTTP请求的起始行、请求头信息后，都要把成员AbsURI设置成完整的绝对地址，把请求地址设置为DocURI的初始地址。


### 4.10 chunk_t数据结构

#### 4.10.1 chunk_t是什么数据结构

chunk_t是针对碎片数据块、不连续数据块、跨越多个物理存储系统的数据块等，进行统一的、序列化的、连续性管理的基本数据结构，对外提供线性访问数据的接口，碎片数据内容可以来自内存块、文件、网络、回调接口函数等。

chunk_t数据结构的目标：
* 解决不同存储介质上的、不连续的、碎片化数据融合读写访问
* 使用更低的内存来达到更大吞吐能力
* 实现高性能HTTP超大文件的上传下载

chunk_t是[adif基础数据结构](https://github.com/kehengzhong/adif)中实现的一个重要功能模块。

#### 4.10.2 chunk_t应用场景

作为不连续的、碎片化的、处于不同存储介质上的数据块管理基础设施，chunk_t可解决很多棘手的数据读写访问问题。这些问题的源头一般是内存空间有限、处理的数据量超级大、异常崩溃时的持续存储等需要决定的。

使用chunk_t数据结构的应用场景包括：

* （1）在需要大量大块内存做数据存储时，由于大并发所需的大量的大内存块分配，导致内存资源不足，需借助文件作为缓存来将内存数据存储到外存中，这样导致内存数据、外存数据混合在一起进行检索、查询、遍历、读写等操作；

* （2）应用程序在处理请求或响应时，会从不同接口使用不同内存块接收数据，如网络数据、DB数据等，而产生多个不连续的数据块，为减少通信过程中数据频繁拷贝导致的性能损耗，一般不会分配更大的内存块来合并这些碎片内容，而是逻辑上将它们合并在一起，进行检索、查询、遍历、读写处理；

* （3）由于网络波动、CPU和网卡接口处理异步性等原因，调用内核接口读取网络数据时，每次读取的数据大小不一样、数据读取时机也是随机的，那么对数据的解析和处理不连贯，如需要读到\r\n\r\n这四个字节时，才认为HTTP头全部收到，并才可以进行头信息解析处理，在未收到这四个字节前，数据每次到达和到达大小的随机性，需要一个缓存来连续累积保存这些字节流，方便收全相关字节流后统一处理。为了提升效率，尽可能零拷贝，那么将接收到的内存块移出来，合在一起，进行连续性的操作；

eJet系统中，一个常见的应用场景是处理HTTP请求和响应时，经常是动态添加请求体或响应体的数据，这些数据包括内存指针型的数据，如HTTP头经过编码后的数据、XML数据、JSon数据、读数据库时返回的数据等，还包括文件数据，如图片文件、HTML文件等，或者文件的一部分数据。尤其是在处理HTTP请求时，需要返回给客户端的响应体来自上层应用的回调函数，这些回调函数提供的响应体内容，可能是动态的字节数据块，而这些可能是不连续的内容。
 
chunk_t数据结构对不连续的碎片数据存储进行管理，提供连续顺序的访问接口来读写数据。

#### 4.10.3 chunk_t接口功能

使用chunk_t数据结构，可随意添加任意内存块数据（已分配的、未分配要求分配的）、文件数据（文件名、FILE文件指针、FD文件描述符）或文件中的部分数据、回调函数界定的数据等，这些数据按照添加的顺序，进行统一管理，提供按照序号逐一访问每一个字节、指定偏移量访问内容块、模式匹配查找检索、指定偏移和范围写入到第三方文件和Socket中、遍历和跳转等。

Linux内核提供了多缓冲区数据读写系统调用writev，其定义如下：
```c
ssize_t writev(int fd, const struct iovec *iov, int iovcnt);
```

chunk_t数据结构和writev系统调用是天作之合，保存在chunk_t中的不连续的、碎片数据，通过writev的一次系统调用就可以解决多缓冲区的发送到文件描述符的问题，比多次调用write来逐个发送chunk_t的碎片缓冲区的效率高很多倍。

此外Linux内核提供了在文件描述符之间传输数据的系统调用sendfile，其定义如下：
```c
ssize_t sendfile(int out_fd, int in_fd, off_t *offset, size_t count);
```

sendfile对两个文件描述符拷贝数据是在内核中完成的，不消耗CPU和内存，非常高效快速地实现文件描述符之间的数据转储。同样地，chunk_t数据结构和sendfile系统调用的配合也是天衣无缝，当chunk_t中存在文件数据时，如果要写入到网络socket，或另外一个文件，调用sendfile便可高效地将其中的文件数据转储出去。

这两个系统调用是Zero-Copy零拷贝技术的主要承载接口，chunk_t是根据零拷贝思想设计的数据结构，很好地封装并解决了复杂数据的高效存储、访问、读写问题。

chunk_t提供了一个接口，用于writev和sendfile的调用，其接口定义如下：
```c
int chunk_vec_get (void * vck, int64 offset, chunk_vec_t * pvec, int httpchunk)
```

调用该接口函数，可以获取到依照顺序而存储起来的多个碎片内存块，来调用writev函数，或获取到文件后，调用sendfile函数。


### 4.11 HTTP请求/响应的发送流程（writev/sendfile）

eJet系统发送HTTP响应到客户端、或者发送HTTP请求到Origin服务器端，采用的流程是一样的，就是利用chunk_t数据结构管理和保存待发送的数据，通过writev和sendfile系统调用，将数据发送给对方。

发送流程是通过TCP连接上建立的的HTTPCon实例来发送数据，由于一个连接上可以同时发送多个HTTP请求，那么必须按照流水线pipeline方式，顺序处理每个请求和响应，前面的HTTPMsg（包含请求和响应）没有发送成功之前，不能发送后续的HTTPMsg。

以发送HTTPMsg的响应到客户端为例，发送过程是从当前HTTPCon中的msg_list中以FIFO方式取出第一个HTTPMsg：如果该请求是一个Proxy请求，则调用Proxy相关的发送流程；如果该请求的响应尚未处理完毕，或者所有响应的内容都已发送完毕，则终止发送过程，否则开始发送响应数据。

HTTPMsg中为了发送数据到通信对方，必须有几个重要成员变量，一个是类型为数据结构chunk_t的rex_body_chunk，一个当前发送成功的总长度rex_stream_sent；其次是确保当前TCP连接的几个内核选项NOPUSH状态需要设置为TRUE、NODELAY状态需复位为FALSE，以保证当前TCP数据在填充满缓冲区后再发送出去，提升发送效率。

用已发送长度作为当前位置，判断chunk_t类型的rex_body_chunk是否结束，如果没有结束：
* 调用chunk_vec_get获取若干个连续碎片缓冲区或文件；
* 如果为待发送内容为连续多个缓冲区，调用writev接口发送；
* 如果为文件，调用sendfile来发送；
* 发送成功，统计字节数，并移除chunk_t类型rex_body_chunk中的已发送成功的碎片块；
* 循环执行上述流程，直至所有数据发送完毕。

当前HTTPMsg发送成功后，将当前请求和响应信息写入日志后，关闭当前HTTPMsg，接着从HTTPCon中的msg_list弹出后续HTTPMsg继续发送操作。

由于采用了chunk_t数据结构，并充分利用writev和sendfile系统调用，使得发送流程非常简洁高效。

### 4.12 eJet日志系统

eJet日志系统是记录HTTP请求和响应的日志信息。每个HTTP请求和响应的信息很多，决定哪些内容写入access.log是在配置文件中用HTTP变量来动态配置的。

eJet日志系统是记录Web服务器收到客户端HTTP请求并返回响应的基本日志信息，日志条目内容是在配置文件中动态配置，在http.access log项下，设置了AccessLog的配置项。首先配置是否需要启动AccessLog的log2file = on/off，设置配置文件名，然后最主要的是配置写入日志文件的每条记录都包含哪些内容字段，这些内容字段是由各个HTTP变量组成，通过HTTP变量动态地获取每个请求和响应的基本信息。

```
access log = {
    log2file = on;
    log file = /var/log/access.log;

    format = [ '$remote_addr', '-', '[$datetime[stamp]]', '"$request"', 
               '"$request_header[host]"', '"$request_header[referer]"', '"$http_user_agent"',
               '$status', '$bytes_recv', '$bytes_sent'
             ];
}
```
 
采用HTTPLog结构来管理访问日志，包括配置信息、日志文件句柄、写入文件的互斥锁、日志内容空间等。日志的写入时机是在HTTPMsg创建、处理完所有操作、并将响应成功返回客户端、准备关闭HTTPMsg之前，来写入请求响应过程中的所有动态信息到文件中。这个时机点，是完成所有请求、处理、返回响应后才开始写入日志信息，在极端情况下，响应发送过程的时间有可能会非常长，如在下载一个大文件时，持续时间会长达几分钟、几十分钟或几个小时；流媒体推送流的时间也很长等等.
 
一般HTTP AccessLog是很多应用系统，如CDN服务平台，作为统计实时流量的重要来源，常需要按日、周、月等时间周期来汇总日志信息，HTTPLog的未来实现版本，需要考虑生成的日志文件是一个固定的文件、还是每天开始时生成一个新日志文件、还是每周或每月开始时生成新文件。

### 4.13 Callback回调机制

#### 4.13.1 eJet回调机制

eJet系统提供了HTTP请求消息交付给应用程序处理的回调机制，回调机制是事件驱动模型中底层系统异步调用上层处理函数的编程模式，上层应用系统需事先将函数实现设置到底层系统的回调函数指针中。

eJet系统提供了两种回调机制，一种是在启动eJet时，设置的全局回调函数，另一种是在系统配置文件中位于监听服务下的动态库配置回调机制。

#### 4.13.2 eJet全局回调函数

全局回调函数的设置是在启动eJet系统时，应用层可以实现HTTP消息处理函数，来处理所有HTTP请求的HTTPMsg，这是程序级的回调机制，需要将eJet代码嵌入到应用系统中来实现回调处理。

设置全局回调的API如下：
```c
int http_set_reqhandler (void * vmgmt, RequestHandler * reqhandler, void * cbobj);
```
其中，vmgmt是eJet系统创建的全局管理入口HTTPMgmt对象实例， reqhandler是应用层实现的回调函数，cbobj是应用层回调函数的第一个回调参数，eJet每次调用回调函数时，必须携带的第一个参数就是cbobj。

应用层回调函数的原型如下：
```c
typedef int RequestHandler (void * cbobj, void * vmsg);
```
其中，cbobj是设置全局回调函数时传递回调参数，vmsg是当前封装HTTP请求的HTTPMsg实例对象。

应用程序将系统管理所需的数据结构（包括应用层配置、数据库连接、用户管理等）封装好，创建并初始化一个cbobj对象，作为设置回调函数时的回调参数。通过回调参数，已经HTTPMsg请求对象，可以将请求信息和应用程序内的数据对象建立各种关联关系。

#### 4.13.3 eJet动态库回调

eJet系统另外一种回调是使用动态库的回调方式，这是松耦合型的、修改配置文件就可以完成回调处理的方式。应用程序无需改动eJet的任何代码，只需在配置中添加含有路径的动态库文件名，即可以实现回调功能，其中动态库必须实现三个固定名称的函数，且遵循eJet约定的函数原型定义。

配置文件中添加动态库回调的位置：
```
listen = {
    local ip = *;
    port = 8181;

    request process library = reqhandle.so
......
```

eJet系统启动期间，加载配置文件后，解析三层资源架构的第一步HTTPListen时，其配置项下的动态库会被加载，加载过程为：
* 加载配置项指定动态库文件；
* 根据函数名http_handle_init，获取动态库中的初始化函数指针；
* 根据函数名http_handle，获取动态库中的回调处理函数指针；
* 根据函数名http_handle_clean，获取动态库中的清除函数指针；
* 执行动态库初始化函数，并返回初始化后的回调参数对象。

在eJet系统退出时，会调用http_handle_clean来释放初始化过程分配的资源。

动态库在实现回调时，必须含有这三个函数名：http_handle_init、http_handle、http_handle_clean，其函数原型定义如下：
```c
typedef void * HTTPCBInit     ();
typedef void   HTTPCBClean    (void * hcb);
typedef int    RequestHandler (void * cbobj, void * vmsg);
```
其中回调函数http_handle的第一个参数cbobj是由http_handle_init返回的结果对象，vmsg即是eJet系统的HTTPMsg实例对象。


#### 4.13.4 回调函数使用HTTPMsg的成员函数

eJet系统通过传递HTTPMsg实例对象给回调函数，来处理HTTP请求。HTTP对象封装了HTTP请求的所有信息，回调函数在处理请求时，可以添加各种响应数据到HTTPMsg中，包括响应状态、响应头、响应体等。

访问请求头信息或添加响应数据的操作，既可以直接对HTTPMsg的成员变量进行数据读取或写入，也可以通过调用HTTPMsg内置的指针函数来进行处理，HTTPMsg中封装了很多函数调用，通过这些函数，基本可实现eJet系统HTTP请求处理的各种操作。这些例子函数如下：
```c
......
char * (*GetRootPath)     (void * vmsg);
 
int    (*GetPath)         (void * vmsg, char * path, int len);
int    (*GetRealPath)     (void * vmsg, char * path, int len);
int    (*GetRealFile)     (void * vmsg, char * path, int len);
int    (*GetLocFile)      (void * vmsg, char * p, int len, char * f, int flen, char * d, int dlen);
 
int    (*GetQueryP)       (void * vmsg, char ** pquery, int * plen);
int    (*GetQuery)        (void * vmsg, char * query, int len);
int    (*GetQueryValueP)  (void * vmsg, char * key, char ** pval, int * vallen);
int    (*GetQueryValue)   (void * vmsg, char * key, char * val, int vallen);

int    (*GetReqContentP)    (void * vmsg, void ** pform, int * plen);
 
int    (*GetReqFormJsonValueP)  (void * vmsg, char * key, char ** ppval, int * vallen);
int    (*GetReqFormJsonValue)   (void * vmsg, char * key, char * pval, int vallen);

int    (*SetStatus)      (void * vmsg, int code, char * reason);
int    (*AddResHdr)      (void * vmsg, char * na, int nlen, char * val, int vlen);
int    (*DelResHdr)      (void * vmsg, char * name, int namelen);
 
int    (*SetResEtag) (void * vmsg, char * etag, int etaglen);

int    (*SetResContentType)   (void * vmsg, char * type, int typelen);
int    (*SetResContentLength) (void * vmsg, int64 len);

int    (*AddResContent)       (void * vmsg, void * body, int64 bodylen);
int    (*AddResContentPtr)    (void * vmsg, void * body, int64 bodylen);
int    (*AddResFile)          (void * vmsg, char * filename, int64 startpos, int64 len);

int    (*Reply)          (void * vmsg);
int    (*RedirectReply)  (void * vmsg, int status, char * redurl);
......
```


### 4.14 正则表达式的使用

正则表达式是用来检索、替换那些符合某个模式或规则的文本，是对字符串操作的一种逻辑公式，用事先定义好的一些特定字符、及这些特定字符的组合，组成一个“规则字符串”，这个“规则字符串”用来表达对字符串的一种过滤逻辑。

eJet系统中，正则表达式主要用途是模式匹配，将配置文件中预先定义好的模式串，去匹配HTTP请求中的特定变量的内容，根据匹配结果来决定后续操作。

eJet使用正则表达式的场景主要有四种：
* 根据HTTP请求的路径，进行精准匹配、前缀匹配和正则表达式模式匹配，最终完成资源定位；
* Script脚本中，if语句中条件比较时，使用正则表达式的匹配操作；
* Script脚本中，rewrite语句中使用正则表达式模式串，匹配当前HTTP请求的路径，来决定替换操作；
* eJet系统作为HTTP客户端发起向Origin服务器的HTTP请求时，根据配置来决定是否设置Proxy正向代理，左侧为正则表达式模式串，需要匹配目标主机名；

eJet系统使用Linux自带的POSIX标准的正则表达式函数regcomp、regexec、regfree，来实现上述功能，基本能满足解析和匹配需求。

### 4.15 TLS/SSL

#### 4.15.1 TLS/SSL、OpenSSL介绍

SSL的全称为Secure Socket Layer，即安全套接字层，是Netscape于90年代研发，位于TCP协议之上，利用PKI安全加密体系来实现认证和加密传输，SSL当前最新版本为3.0。

SSL协议分为两层：
* SSL记录协议（SSL Record Protocol）：在TCP之上，为高层协议提供数据封装、压缩、加密等功能，定义传输格式
* SSL握手协议（SSL Handshake Protocol）：在SSL记录协议之上，对通讯双方进行身份认证、协商加密算法、交换密钥等。

TLS的全称是Transport Layer Security，即传输层安全协议，当前最新版为TLS 1.3，是IETF（Internet Engineering Task Force，互联网工程任务组）制定的一种新的协议，建立在SSL 3.0协议规范之上，是SSL 3.0的后续版本。

同样TLS协议由两层组成：
* TLS 记录协议（TLS Record）
* TLS 握手协议（TLS Handshake）

SSL/TLS协议提供的服务主要有：
* 认证。认证客户端和服务器，确保数据发送到正确的客户端和服务器；
* 加密。加密数据以防止数据中途被窃取；
* 一致性。维护数据的完整性，确保数据在传输过程中不被改变

实现TLS/SSL协议的开源软件是OpenSSL，是澳洲人Eric Young、Tim Hudson于90年代开源的SSLeay基础上演变过来的，采用标准C语言编写，广泛用于使用加密和安全的环境。

#### 4.15.2 eJet集成OpenSSL

客户端发起HTTP请求，如果scheme是https，则需要建立SSL/TLS over TCP的安全连接到eJet服务器系统，

eJet系统作为服务器端接收客户端HTTP请求和作为客户端向Origin服务器发送HTTP请求时，都会使用到SSL连接，调用OpenSSL的方法有一些差别。

eJet作为服务器端使用SSL时，使用OpenSSL的基本流程共有九个步骤。

* 1. 初始化OpenSSL库

系统初始化时，首先调用SSL_library_init初始化OpenSSL库，调用SSL_add_ssl_algorithms()添加SSL缺省算法，加载错误信息定义串，如果根据SSL连接实例能获取到HTTPCon对象，需创建SSL连接索引，并利用该连接索引，将SSL Socket连接实例和HTTPCon对象关联。

* 2. 配置证书和私钥

在系统配置Listen下，设置HTTPListen监听服务下是否支持SSL，及缺省的证书、私钥和CA证书，并在每个域名对应的虚拟主机下，配置启用SSL所需的服务器证书、私钥和CA证书。示例如下：
```
listen = {
    local ip = *; # any IP address
    port = 443;
    forward proxy = off;

    ssl = on;
    ssl certificate = cert.pem;
    ssl private key = cert.key;
    ssl ca certificate = cacert.pem;

    host = {
        host name = www.yunzhai.cn
        type = server;

        ssl certificate = yzcert.pem;
        ssl private key = yzcert.key;
        ssl ca certificate = yzcacert.pem;
...... 
    }
......
}
```

* 3. 初始化SSL_Ctx

在系统初始化最后，开始启动HTTPListen服务前，加载监听服务和其下各虚拟主机时，分别根据证书、私钥和CA证书，创建HTTPListen的缺省SSL_Ctx实例，或创建各虚拟主机HTTPHost下的SSL_Ctx。

创建SSL_Ctx的过程先调用SSL_CTX_new创建实例，随后加载证书和私钥，并校验证书和私钥是否匹配，如果存在CA证书，还需加载CA证书。

最后，启用SNI（Server Name Indication）机制，设置一个回调函数来处理不同域名对应不同的证书和私钥，在SSL启动Handshake时，先发送ClientHello请求，其中携带了当前连接对应的域名，服务器端收到ClientHello时，会以域名为参数，调用回调函数，选择与之相对应的SSL_Ctx。

* 4. 接受连接并创建SSL Socket

eJet服务器收到客户端的TCP连接请求时，创建HTTPCon实例，保存连接信息后，HTTPCon需关联HTTPListen，并根据HTTPListen中的ssl_link配置选项，来创建SSL Socket连接实例，其过程主要包括：使用SSL_new创建SSL实例，调用SSL_set_fd设置当前连接的文件描述符，调用SSL_set_ex_data将当前SSL对象和HTTPCon实例对象关联起来。最后，设置当前HTTPCon的ssl_handshaked状态为未建立握手状态。

* 5. 根据域名选择对应的SSL_Ctx

一个监听端口下，可以有多个证书，用于不同的主机名，客户端HTTPS请求到达时，需要使用合适的证书来完成后续SSL握手和加密通信，这是采用TLS规范的SNI机制来实现的。

在创建SSL_Ctx时，需设置多域名选择的回调函数，SSL握手开始时的ClientHello请求携带请求的域名名称，回调函数根据SSL_get_servername获取到域名名称，在当前HTTPListen下查找该名称对应的虚拟主机HTTPHost，并调用SSL_set_SSL_CTX，将当前HTTPCon中的SSL连接的SSL_Ctx上下文实例设置为该HTTPHost下的sslctx，即可实现证书选择和切换操作。

* 6. SSL握手

对于接受客户端请求的情形，SSL握手过程是在SSL_accept中实现的，由于网络抖动等因素，握手过程中往来的数据需要通过多次读写事件来驱动完成，在http_pump处理IOE_READ和IOE_WRITE时，需要判断当前HTTPCon的ssl_handshaked状态，如果没有握手成功，则响应这两个ePump事件时，都需要调用SSL_accept。

eJet还需要根据SSL_accept的错误状态码，来添加对当前TCP连接的读就绪或写就绪监听处理，并在http_pump中处理读写事件。这是非阻塞通信下建立SSL连接必须要注意的步骤。

如果SSL_accept返回成功，则将HTTPCon的ssl_handshaked设置为已完成握手状态，并调用http_cli_recv来接收SSL上的数据。

* 7. 在SSL连接上接收数据

eJet系统封装了一个针对HTTPCon的数据接收函数，同时兼容有SSL连接和没有SSL连接这两种情况，函数定义如下：
```c
int http_con_read (void * vcon, frame_p frm, int * num, int * err);
```
ePump框架在当前连接有数据可读时，回调http_pump处理IOE_READ事件，如果完成了握手过程，则调用这个函数来读取数据。如果是SSL连接，该函数调用SSL_read来读取数据，如果读取成功，返回的是解密完成后的数据长度，并将解密后的数据存入缓冲区，注意：这里有两次拷贝（从内核拷贝到临时缓冲区，再从临时缓冲区拷贝到目标缓冲区），需要优化。

如果SSL_read返回0，则当前连接出现故障，需关闭连接。如果返回值小于0，则调用SSL_get_error来处理错误码，对于SSL_ERROR_WANT_READ和SSL_ERROR_WANT_WRITE两种情况需要调用ePump接口设置添加读写就绪监听。

* 8. 在SSL连接上发送数据

eJet系统中发送数据流程一般是用chunk_t数据结构管理数据，调用writev和sendfile将数据通过网络发送给对方，在SSL连接情况下，eJet系统同样封装了两个类似的函数：
```c
int http_con_writev (void * vcon, void * piov, int iovcnt, int * num, int * err);
int http_con_sendfile (void * vcon, int filefd, int64 pos, int64 size, int * num, int * err);
```
这两个函数同时兼容有SSL连接和没有SSL连接这两种情况，在没有SSL连接情况下，直接调用tcp_writev和tcp_sendfile。

在有SSL连接情况下，调用SSL_write函数，要写入的明文数据调用SSL_write后被加密并传输给对方。如果发送成功返回的是写入数据的长度，如果返回0，则当前连接出现故障，需关闭连接。如果返回值小于0，则需要调用SSL_get_error来处理错误码，对于SSL_ERROR_WANT_READ和SSL_ERROR_WANT_WRITE两种情况需要调用ePump接口设置添加读写就绪监听。

* 9. 关闭SSL连接

在处理完成数据读写操作，或者网络错误等情况，当前HTTPCon会被关闭，如果是SSL连接则需释放SSL实例，分别调用SSL_shutdown和SSL_free来完成资源释放。

以上九个步骤是eJet系统作为HTTP服务器时使用SSL连接来传输数据的基本流程，对于eJet系统作为HTTP客户端情形，过程基本类似，这里不再赘述。


### 4.16 Chunk传输编码解析

HTTP 1.1协议增加了Transfer-Encoding: chunked的头类型，表示消息体的内容长度不能确定，需采用分块传输编码方式，将消息体发送给对方。

Chunked Transfer Coding分块传输编码是由多个Chunk块组成，每个Chunk块包括两部分，十六进制的分块数据长度加上可选的分块扩展加上\r\n、实际分块数据加上\r\n，分块传输编码的结尾是以分块数据长度为0的分块组成。

分块传输数据格式如下：
```
chunked body = chunk-size[; chunk-ext-nanme [= chunk-ext-value]]\r\n
               ...
               0\r\n
               [footer]
               \r\n
```
chunk size是以16进制表示的长度，footer一般是以\r\n结尾的entity-header，一般都忽略掉。

eJet系统使用HTTPChunk数据结构来解析chunk分块传输编码的消息体数据，使用chunk_t数据结构来打包分块传输编码。HTTPChunk数据结构包含chunk_t成员实例，用于存储解析成功的Chunk数据块，每一个Chunk数据块解析状态和信息用ChunkItem来存储管理，HTTPChunk中用item_list来管理多个ChunkItem。

采用chunk分块传输编码的消息体，实际情况是一边传输一边解析，解析过程要充分考虑到当前接收缓冲区内容的不完整性，这是由HTTPChunk里的http_chunk_add_bufptr来实现的，函数定义如下：
```c
int http_chunk_add_bufptr (void * vchk, void * vbgn, int len, int * rmlen);
```
vbgn和len指向需解析的消息体数据，rmlen是解析成功后实际用于chunk分块传输编码的字节数量。

eJet在遇到chunk分块传输编码的消息体时，每次收到读事件，就将数据读取到缓冲区，将缓冲区所有数据交给这个解析函数解析处理，返回的rmlen值是被解析和处理的字节数，将处理完的数据从缓冲区移除掉。通过http_chunk_gotall来判断是否接收到全部chunk分块传输编码的所有数据，如果没有，循环地用新接收的数据调用该函数来解析和处理，直至成功接收完毕。


### 4.17 反向代理和正向代理

#### 4.17.1 判断是否为代理请求

反向代理是将不同的Origin服务器代理给客户端，客户端不做任何代理配置发起正常的HTTP请求到反向代理服务器，反向代理服务器根据配置的路径规则，代理访问不同的Origin服务器并将响应结果返回给客户端，让客户端认为反向代理服务器就是其访问的Origin服务器。

正向代理需要求客户端设置正向代理服务器地址，明确给定Origin服务器地址，要求正向代理服务器想给定的Origin服务器转发请求和响应。

上面描述的反向代理服务器，在这里就是eJet Web服务器，除了充当Web服务器功能外，还可以充当正向代理服务器和反向代理服务器。

eJet系统在HTTPMsg实例化完成后，首先要检查的是当前请求是否为Proxy代理请求:
* 是否在rewrite时启动forward到一个新的Origin服务器的动作，如果是则代理转发到新的URL
* 是否为正向代理，正向代理的请求地址request URI是绝对URI，如果是则代理转发到绝对URI上
* 判断当前资源位置HTTPLoc是否配置了反向代理，以及反向代理指向的Origin服务器，如果是，根据规则生成访问Origin服务器的URL地址

以上三种情况中，第一种和第三种为反向代理，第二种为正向代理，对应的配置样例如下：
```
location = { #rewrite ... forward
    type = server;
    path = ['/5g/', '^~' ];
    script = {
        rewrite ^/5g/.*tpl$ http://temple.ejetsrv.com/getres.php forword;
    }
}

# HTTP请求行是绝对URI地址
GET http://cdn.ejetsrv.com/view/23C87F23D909B47E2187A0DB83AF07D3 HTTP/1.1
....

location = { # 反向代理配置
    path = [ '^/view/([0-9A-Fa-f]{32})$', '~*' ];
    type = proxy;
    passurl = http://cdn.ejetsrv.com/view/$1;
......
}
```

无论是正向代理，还是反向代理，最后转发请求的操作流程基本类似，即需明确指向新Origin服务器的URL地址，作为下一步转发地址，主动建立到Origin服务器的HTTPCon连接，组装新的HTTPMsg请求，发送请求并等候响应，将响应结果转发到源HTTPMsg中，发送给客户端。

如果是代理请求，包括正向代理或反向代理，eJet需要做Proxy代理转发处理，基本流程已经在[2.10 Proxy代理转发](#210-proxy代理转发)中描述过了，这里不多介绍。

#### 4.17.2 代理请求的实时转发

需要重点介绍的是实时转发源请求到Origin服务器的流程。代理转发时先创建一个代理转发的HTTPMsg实例，将源请求HTTPMsg实例的请求数据复制到代理请求HTTPMsg中，如果HTTP请求含有请求消息体时，代理转发流程有两种实现方式：
* 一种方式是存储转发，即接收完所有的HTTP请求消息体后，再复制到代理转发HTTPMsg中，最后发送出去
* 另一种方式实时转发，即接收一部分消息体就发送一部分消息体，直到全部发送完毕

为了确保代理转发效率和降低存储消耗，eJet系统采用实时转发模式。

源请求的消息体内容保存在HTTPCon的rcvstream中，响应IOE_READ事件时将网络内容读取到该缓冲区后，就要调用http_proxy_srv_send来实时转发。转发的数据包括代理请求头、上次未发送成功的消息体、及当期位于HTTPCon缓冲区中的rcvstream，严格按照接收的顺序来发送。

每次未发送成功的消息体，将会从HTTPCon的rcvstream中拷贝出来，转存到代理请求HTTPMsg中的req_body_stream中，作为临时缓冲区保存累次未能发送的消息体。当从源HTTPCon中接收到新数据、或到Origin服务器的目的HTTPCon中可写就绪时，都会启动http_proxy_srv_send的实时发送流程，而优先发送的消息体就是代理请求中req_body_stream中的内容。

源请求的消息体有三种情况：
* 没有消息体内容
* 存在以Content-Length来标识大小的消息体内容
* 存在以Transfer-Encoding标识分块传输编码的消息体内容

实时转发需要处理这三种情况，最终通过http_con_writev来发送给对方。发送不成功的剩余内容，需要从源HTTPCon中拷贝到代理请求HTTPMsg中的req_body_stream中。

实时转发最大问题是拥塞问题，即源HTTPCon上的请求数据发送速度很快，但到Origin服务器的目的HTTPCon连接的发送速度比较慢，导致大量的数据会堆积到代理消息HTTPMsg中req_body_stream中，消耗大量内存，严重时会导致内存消耗过大系统崩溃。

代理消息实时转发模式的拥塞问题根源在于两条线路传输速度不对等导致，只要发送侧速度大于接收侧速度，拥塞问题就会出现。解决拥塞问题需从源头来考虑，判断是否拥塞的标准是堆积的内存缓冲区超过一定的阈值，一旦内存堆积超过阈值，就断定为拥塞，需限制客户端继续发送任何内容，直到解除拥塞后继续发送。

拥塞控制的细节参见4.19章节的内容。

#### 4.17.3 代理响应的实时转发

代理请求转发给Origin服务器后，会返回响应消息，包括响应头和响应体，响应头的接收和处理可参见[4.6 HTTP请求和响应](#46-http请求和响应)。

和HTTP请求消息的实时转发类似，代理消息的响应也需要实时转发给客户端。

根据代理HTTPMsg内部成员proxiedl连判断当前消息是否为代理，对Origin返回的响应头信息进行预处理：
* 如果是301/302跳转，当前代理消息是反向代理，并且系统允许自动重定向，则需重新发送重定向请求；
* 如果需要缓存到本地存储系统，采用缓存处理流程，见4.20章节
* 其他情形就按照代理响应来处理

复制所有的响应状态码和响应头到源HTTPMsg中，并将响应HTTPCon的接收缓冲区rcvstream数据实时转发到源HTTPCon中，同样地，HTTPCon中没有发送不成功的数据，转存到源HTTPMsg中的res_body_stream中临时缓存起来。每次当源HTTPCon可写就绪、或代理HTTPCon有数据可读并读取成功后，都会调用http_proxy_cli_send，优先发送的是堆积在res_body_stream中的数据。

其他后续流程类似请求消息的实时转发。

### 4.18 FastCGI机制和启动PHP的流程

#### 4.18.1 FastCGI基本信息

FastCGI是CGI（Common Gateway Interface）的开放式扩展规范，其技术规范见网址[http://www.mit.edu/~yandros/doc/specs/fcgi-spec.html](http://www.mit.edu/~yandros/doc/specs/fcgi-spec.html)

对静态HTML页面中嵌入动态脚本程序的内容，如PHP、ASP等，需要由特定的脚本解释器来解释运行，并动态生成新的页面，这个过程需要eJet Web服务器和脚本程序解释器之间有一个数据交互接口，这个接口就是CGI接口，考虑到性能局限，早期的独立进程模式的CGI接口发展成FastCGI接口规范。习惯地，我们把解释器称之为CGI服务器。

使用CGI接口规范的页面脚本程序可以使用任何支持标准输入STDIN、标准输出STDOUT、环境变量的编程语言来编写，如PHP、Perl、Python、TCL等。在传统CGI规范的fork-and-execute模式中，Web服务器会为每个HTTP请求，创建一个新进程、解释执行、返回响应、销毁进程，这是个很重的工作流程。

FastCGI对CGI这种重模式进行了简化，脚本解释器和Web服务器之间的交互，通过Unix Socket或TCP协议来实现，Web服务器收到需要解释执行的HTTP请求时，建立并维持通信连接到CGI服务器，按照FastCGI通信规范发送请求，并接收响应，这个流程相比CGI模式，大大提升了性能和并发处理能力。

PHP解释器名称为php-fpm（php FastCGI Processor Manager），作为FastCGI通信服务器监听来自Web服务器的连接请求，并接收连接上的数据，进行解析、解释执行后，返回响应给Web服务器端。php-fpm的配置项中，启动监听服务：
```
; The address on which to accept FastCGI requests.
; Valid syntaxes are:
;   'ip.add.re.ss:port'    - to listen on a TCP socket to a specific IPv4 address on
;                            a specific port;
;   '[ip:6:addr:ess]:port' - to listen on a TCP socket to a specific IPv6 address on
;                            a specific port;
;   'port'                 - to listen on a TCP socket to all addresses
;                            (IPv6 and IPv4-mapped) on a specific port;
;   '/path/to/unix/socket' - to listen on a unix socket.
; Note: This value is mandatory.
listen = /run/php-fpm/www.sock
;listen = 9000
```

#### 4.18.2 eJet如何启用FastCGI

eJet收到客户端的HTTP请求并创建HTTPMsg和完成HTTPMsg实例化后，根据资源位置HTTPLoc是否将资源类型设置为FastCGI、并且设置了指向CGI服务器地址的passurl，如果都设置这两个参数，则当前请求会被当做FastCGI请求转发给CGI服务器。

启用FastCGI的参数配置如下：
```
location = {
    type = fastcgi;
    path = [ "\.(php|php?)$", '~*'];

    passurl = fastcgi://127.0.0.1:9000;
    #passurl = unix:/run/php-fpm/www.sock;

    index = [ index.php ];
    root = /data/wwwroot/php;
}
```
只要是请求DocURL中路径名称是以.php或.php5等结尾，当前请求都会被FastCGI转发。

在获取转发URL地址时，是复制配置中的passurl地址，即CGI服务器地址，不能把HTTP请求中的路径和query参数信息添加在这个转发URL后面。转发地址有两种形态：
* 采用TCP协议的CGI服务器地址，以fastcgi://打头，后跟IP地址和端口，或域名和端口；
* 采用Unix Socket的CGI服务器地址，以unix:打头，后跟Unix Socket的路径文件名。

passurl地址指向CGI服务器，eJet服务器可以支持很多个CGI服务器。

eJet获取到FastCGI转发地址后，根据该地址创建或打开CGI服务器FcgiSrv对象实例，建立TCP连接或Unix Socket连接到该服务器的FcgiCon实例，为当前HTTP请求创建FcgiMsg消息实例，将HTTP请求信息按照FastCGI规范封装到FcgiMsg中，并启动发送流程，将请求发送到CGI服务器。

#### 4.18.3 FastCGI的通信规范

FastCGI通信依赖于C/S模式的可靠的流式的连接，协议定义了十种通信PDU（Protocol Data Unit）类型，每个PDU都由两部分组成：一部分是FastCGI Header头部，另一部分是FastCGI消息体，FastCGI的PDU是严格8字节对齐，PDU总长度不足8的倍数，需要添加Padding补齐8字节对齐。FastCGI的PDU头格式如下：
```c
typedef struct fastcgi_header {
    uint8           version;
    uint8           type;
    uint16          reqid;
    uint16          contlen;
    uint8           padding;
    uint8           reserved;
} FcgiHeader, fcgi_header_t;
```
上面定义的协议头格式中，version版本号1个字节，缺省值为1，type为PDU类型1个字节，共计定义了10种类型，reqid为PDU的序号，两字节BigEndian整数，contlen是PDU消息体的内容长度，两字节BigEndian整数，1字节的padding是PDU消息体不是8字节的倍数时，需要补齐8字节对齐所填充的字节数，保留1字节。

其中PDU类型共有十种，分别定义如下：
```c
/* Values for type component of FCGI_Header */
#define FCGI_BEGIN_REQUEST       1
#define FCGI_ABORT_REQUEST       2
#define FCGI_END_REQUEST         3
#define FCGI_PARAMS              4
#define FCGI_STDIN               5
#define FCGI_STDOUT              6
#define FCGI_STDERR              7
#define FCGI_DATA                8
#define FCGI_GET_VALUES          9
#define FCGI_GET_VALUES_RESULT  10
#define FCGI_UNKNOWN_TYPE       11
#define FCGI_MAXTYPE            (FCGI_UNKNOWN_TYPE)
```
其中从Web服务器发送给CGI服务器的PDU类型为：BEGIN_REQUEST、ABORT_REQUEST、PARAMS、STDIN、GET_VALUES等，从CGI服务器返回给Web服务器的PDU类型为：END_REQUEST、STDOUT、STDERR、GET_VALUES_RESULT等。

根据PDU type值，PDU消息体格式也都不一样，分别定义为：
```c
typedef struct {
    uint8  roleB1;
    uint8  roleB0;
    uint8  flags;
    uint8  reserved[5];
} FCGI_BeginRequest;

/* Values for role component of FCGI_BeginRequest */
#define FCGI_RESPONDER           1
#define FCGI_AUTHORIZER          2
#define FCGI_FILTER              3
```
BEGIN_REQUEST是发送数据到CGI服务器时，第一个必须发送的PDU。其中的角色role是两个字节组成，高位在前、低位在后，一般情况role值为RESPONSER，即要求CGI服务器充当Responder来处理后续的PARAMS和STDIN请求数据。字段flags是指当前连接keep-alive还是返回数据后立即关闭。

第二个需发送到CGI服务器的PDU是PARAMS，其格式是由FcgiHeader加上带有长度的name/value对组成，PDU消息体格式如下：
```c
typedef struct {
    uint8    namelen;    //namelen < 0x80
    uint32   lnamelen;   //namelen >= 0x80
    uint8    valuelen;   //valuelen < 0x80
    uint32   lvaluelen;  //valuelen >= 0x80
    uint8  * name;       //[namelen];
    uint8  * value;      //[valuelen];
} FCGI_PARAMS;
```
FastCGI中的PARAMS PDU是将HTTP请求头信息和预定义的Key-Value头信息发送给CGI服务器，这些信息都是Key-Value键值对。如果key或value的数据长度在128字节以内，其长度字段只需一个字节即可，如果大于或等于128字节，则其长度字段必须用BigEndian格式的4字节。在对HTTP请求头和预定义头Key-Value对信息封装编码成PARAMS PDU时，每个Header字段的编码格式为：先是Header的name长度，再是value长度，随后是name长度的name数据内容，最后是value长度的value数据内容。
```
1字节namelen或4字节namelen + 1字节valuelen或4字节valuelen + name + value
```
所有头信息按照上述编码格式打包完成后，总长度如果不是8的倍数，计算需不全8字节对齐的padding数量，将这些数据填充到FcgiHeader中。

第三个要发送到CGI服务器的PDU是STDIN，STDIN PDU是由FcgiHeader加上实际数据组成。注意的是STDIN数据长度不能大于65535，如果HTTP请求中消息体数据大于65535，需要对消息体拆分成多个STDIN包，使得每个STDIN PDU的消息体长度都在65536字节以下。需要特别注意的是，所有数据内容拆分成多个STDIN PDU完成后，最后还需要添加一个消息体长度为0的STDIN PDU，表示所有的STDIN数据发送完毕。

当eJet系统收到HTTP请求并需要FastCGI转发是，按照以上三类数据包协议格式，将HTTP请求打包封装，并发送成功后，就等等CGI服务器的处理和响应了。

CGI服务器返回的PDU一般如下：

如果出现请求格式错误或其他错误，会返回STDERR数据，其消息体是错误内容，将错误内容取出来可以直接返回给客户端。

正常情况下，CGI服务器会返回一个到多个STDOUT PDU，STDOUT的消息体是实际的数据内容，最大长度小于65536。需要将这些STDOUT的内容整合在一起，作为HTTP响应内容。需注意的是STDOUT内容中，也包含部分HTTP响应头信息，其格式遵循HTTP规范，每个响应头有key-value对构成，以\r\n换行符结束，响应头和响应体之间相隔一个空行\r\n。

全部STDOUT数据结束后，紧接着返回的是END_REQUEST PDU，其格式是8字节的FcgiHeader，加上8字节的消息体，其消息体定义如下：
```c
typedef struct {
    uint32     app_status;
    uint8      protocol_status;
    uint8      reserved[3];
} FCGI_EndRequest;

/* Values for protocolStatus component of FCGI_EndRequest */
#define FCGI_REQUEST_COMPLETE    0
#define FCGI_CANT_MPX_CONN       1
#define FCGI_OVERLOADED          2
#define FCGI_UNKNOWN_ROLE        3
```

eJet服务器收到END_REQUEST时，就表示CGI服务器已经返回全部的响应数据了，将这些数据发送给客户端，即可结束当前处理。

#### 4.18.4 FastCGI消息的实时转发

eJet系统将HTTP请求实时转发给CGI服务器，基本过程跟Proxy代理转发类似，包括实时转发、流量拥塞控制等。

其中在接收CGI服务器的响应数据时，需要解析以流式返回的STDOUT PDU的数据，但响应数据的总长度并未返回，eJet对这些响应数据的实时转发是采用Transfer-Encoding分块传输编码模式。为了减少响应数据的多次拷贝，FcgiCon中每次数据读就绪时，存入rcvstream缓冲区的数据，连同rcvstream一起移入到发起HTTP请求的源HTTPMsg内的res_rcvs_list列表中，并将解析成功的内容指针存入到res_body_chunk里，类似客户端访问本地文件一样，通过http_cli_send发送给客户端。

细节不多赘述。


### 4.19 两个通信连接的串联Pipeline

#### 4.19.1 两个通信连接串联成管道

eJet服务器充当转发服务，如代理转发服务、FastCGI转发服务、HTTP Tunnel服务等，需要建立一个管道一样的设施，将通信两端的数据发送和接收，进行实时转发。eJet系统分别于两端维持两个通信连接，这两个连接上任何一个连接上的数据传输，都要实时转发到另外一个连接上，以确保通信的高效率，这两个连接事实上构建了一个传输管道Pipe，在管道上流动的数据遵循线性串行方式，即Pipeline数据传输。

在使用ePump框架管理这两个通信连接管道时，每个连接对应的iodev_t设备，会被ePump框架监听到读就绪（Readable Readiness）、写就绪（Writable Readiness）状态变化，进而产生IOE_READ和IOE_WRITE事件。ePump框架基于负载均衡，可能会把两个iodev_t设备绑定到不同的worker工作线程中，导致处理这两个iodev_t设备读写事件的线程不同，这会导致潜在的同步和访问冲突等问题，所以，需要确保管道两侧的通信连接的读写事件，必须由一个线程来处理。

为了保障管道两侧的通信连接运行在同一个worker工作线程里，eJet系统在创建为源请求HTTPCon创建代理连接HTTPCon后，需调用ePump框架的iodev_workerid_set函数，将新建立的iodev_t设备连接的工作线程设置为当前线程，当前线程一般为源HTTPCon的事件处理线程。

由于创建的TCP连接是非阻塞模式，连接成功事件IOE_CONNECTED一般在调用创建操作后过一段时间到来，由于线程同步问题在创建代理连接后调用iodev_workerid_set，可能有较低的几率设置不成功。所以，还需要在收到响应头后，继续调用该函数：
```c
if (clicon) iodev_workerid_set(pcon->pdev, iodev_workerid(clicon->pdev));
```

#### 4.19.2 两个连接速度不对等导致的流量拥塞

eJet服务器充当Proxy时，Origin服务器到Proxy之间的链路传输速度一般都高于Proxy到客户端之间的传输速度，这样会导致数据在内存中的堆积，如果请求的是流媒体文件且数据数据足够大、这种传输速度不对等的时间持续越久，就会导致流量拥塞在速度慢的通信连接那一侧的缓冲区里，内存终将消耗过大而使得系统崩溃。

解决双连接串联管道传输速度不对等导致的流量拥塞问题，共有两种思路：
* 解决思路一：将Origin服务器上收到的数据全部写入到本地文件缓存中，可参见[4.20 HTTP Cache系统](#420-http-cache系统)。
* 解决思路二：对Origin服务器上的传输链路进行限速，以达到跟客户侧链路速度一样的传输速度。

理论上限速的实现需要依赖TCP协议栈中的流量控制机制，当客户端数据堵塞时，告知Origin发送方TCP协议栈，接收方的数据拥塞了，基于滑动窗口探测机制来实现流量控制。

有关TCP拥塞控制算法可参考慢启动（Slow Start）、拥塞避免（Congestion Avoidance）、快速重传（Fast Restransmission）、快速恢复（Fast Recovery）等协议流程。

从应用层来说，激活TCP拥塞控制机制的方法是放弃接收TCP RecvBuffer中的数据，使得系统中该Socket内核的RecvBuffer填满，进而引起TCP的传输拥塞，使得发送方不再发送数据。

eJet系统中，当给通信速度较慢的客户侧转发数据失败，并且未发送数据累积到内存中超过一定阈值时，就把另一侧速度快的那个连接的iodev_t的读事件监听删除，这样就接收不到Origin服务器的读事件，RecvBuffer数据不会被读出来，激发内核启动拥塞控制机制。而当客户端的数据可写入事件到来并成功写入了内存累积的数据后，判断累计未发送数据是否降低到阈值以下，如果降低了，就将速度快的那个通信连接iodev_t的读监听重新添加，并读取拥塞在内核中的数据到应用层中。

### 4.20 HTTP Cache系统

#### 4.20.1 HTTP Cache功能设置

HTTP Cache是指Web服务器充当HTTP Proxy代理服务器（包括正向代理和反向代理），通过HTTP协议向Origin服务器下载文件，然后转发给客户端，这些文件在转发给客户端的同时，缓存在代理服务器的本地存储中，下次再有相同请求时，根据相关缓存策略决定本地文件是否被命中，如果命中，则该请求无需向Origin服务器请求下载，直接将缓存中命中的文件读取出来返回给客户端，从而节省网络开销。
 
在配置文件中配置正向代理或反向代理的地方，都可以开启cache功能，并基于配置脚本动态设置缓存文件名等缓存选项。
```
location = {
    path = [ '^/view/([0-9A-Fa-f]{32})$', '~*' ];
    type = proxy;
    passurl = http://cdn.yunzhai.cn/view/$1;

    # 反向代理配置缓存选项
    root = /opt/cache/;
    cache = on;
    cache file = /opt/cache/${request_header[host]}/view/$1;
}

send request = {
    max header size = 32K;

    /* 正向代理配置的缓存选项 */
    root = /opt/cache/fwpxy;
    cache = on;
    cache file = <script>
                   if ($req_file_only)
                       return "${host_name}_${server_port}${req_path_only}${req_file_only}";
                   else if ($index)
                       return "${host_name}_${server_port}${req_path_only}${index}";
                   else
                       return "${host_name}_${server_port}${req_path_only}index.html";
                 </script>;
}
```
 
在配置中启动了缓存功能后，还要根据Origin服务器返回的响应头指定的缓存策略，来决定当前下载文件是否保存在本地、缓存文件保存多长时间等。HTTP响应头中有几个头是负责缓存策略的：
```
Expires: Wed, 21 Oct 2020 07:28:00 GMT            (Response Header)
Cache-Control: max-age=73202                      (Response Header)
Cache-Control: public, max-age=73202              (Response Header)

Last-Modified: Mon, 18 Dec 2019 12:35:00 GMT      (Response Header)
If-Modified-Since: Fri, 05 Jul 2019 02:14:23 GMT  (Request Header)
 
ETag: 627Af087-27C8-32A9E7B10F                    (Response Header)
If-None-Match: 627Af087-27C8-32A9E7B10F           (Request Header)
```
Proxy代理服务器需要处理Origin服务器返回的响应头，主要是Expires、Cache-Control、Last-Modified、ETag等。根据Cache-Control的缓存策略决定当前文件是否缓存：如果是no-cache或no-store，或者设定了max-age=0，或者设定了must-revalidate等都不能将当前文件保存到缓存文件中。如果设置了max-age大于0则根据max-age值、Expires值、Last-Modified值、ETag值来判断下次请求是否使用该缓存文件。

#### 4.20.2 eJet系统Cache存储架构

eJet系统是否启动缓存由配置信息来设定。如果是反向代理，HTTP请求对应的HTTPLoc下的反向代理开关cache是否开启，即cache=on，cache file项是否设置，来决定是否启动缓存功能；如果是正向代理，在send request选项中，是否启动cache，以及cache file命名规则是否设置，决定是否启动缓存管理。

启动了cache功能，还需要根据当前请求转发给Origin后，返回的响应头中，是否有Cache管理的头信息，来确定当前返回的响应体是否缓存，以及确定当前缓存的相关信息。
 
缓存的Raw文件内容存储在上述配置中以cache file命名的文件中，当文件所有内容全都下载并存储起来前，文件名后需要增加扩展名.tmp，以表示当前存储文件正在下载中，还不是一个完整的文件，但已经缓存的内容则可以被命中使用。
 
cache管理信息则存储在缓存信息管理文件（Cache Information Management File）中，简称为CacheInfo文件，CacheInfo文件的存储位置在Raw缓存文件所在目录下建立一个隐藏目录.cacheinfo，CacheInfo文件就存放该隐藏目录下，CacheInfo文件名是在Raw存储文件后增加后缀.cacinf，譬如Raw缓存文件为foo.jpg，则缓存信息管理文件路径为：
        .cacheinfo/foo.jpg.cacinf
 
CacheInfo文件的结构包括三部分：Cache头信息（96字节）、Raw存储碎片管理信息。Cache头信息是固定的96字节，其结构如下：
```c
/* 96 bytes header of cache information file */
typedef struct cache_info_s {
    char         * cache_file;
    void         * hcache;
    char         * info_file;
    void         * hinfo;

    uint8          initialized;
 
    uint32         mimeid;
    uint8          body_flag;
    int            header_length;
    int64          body_length;
    int64          body_rcvlen;
 
    /* Cache-Control: max-age=0, private, must-revalidate
       Cache-Control: max-age=7200, public
       Cache-Control: no-cache */
    uint8          directive;     //0-max-age  1-no cache  2-no store
    uint8          revalidate;    //0-none  1-must-revalidate
    uint8          pubattr;       //0-unknonw  1-public  2-private(only browser cache)
 
    time_t         ctime;
    time_t         expire;
    int            maxage;
    time_t         mtime;
    char           etag[36];
 
    FragPack     * frag;
 
} CacheInfo;
```
 
在头信息之后存放的是存储内容碎片管理信息，每个碎片单元为8字节：
```c
typedef struct frag_pack {
    int64    offset;
    int64    length;
} FragPack;
```

内存中采用动态有序数组来管理每一个碎片块，相邻块就需要合并成一个块，完整文件只有一个块。将这些碎片块信息按照8字节顺序存储在这个区域中。每当文件有新内容写入时，内存碎片块数组要完成合并等更新，并将最新结果更新到这个区域。碎片块信息管理的是Raw存储文件中从Origin服务器下载并实际存储的数据存储状态，每块是以偏移量和长度来唯一标识，相邻的碎片块合并，完整文件只有一个碎片块。

#### 4.20.3 eJet系统缓存处理流程

eJet系统作为正向代理或反向代理服务器，实现边下载边缓存、完整缓存时无需代理转发直接返回缓存内容给客户端等功能，可以实现对大大小小的Origin文件的实时缓存功能，包括碎片存储、随机存储等。
 
**（1）全局管理CacheInfo对象**

系统维护一个全局的CacheInfo对象哈希表，以Raw缓存文件名作为唯一标识和索引，如果存在多个用户请求同一个需要缓存的Origin文件时，只打开或创建一个CacheInfo对象，该对象成员由互斥锁来保护。而每个对同一Origin文件的HTTP请求，请求位置、偏移量、读写Raw缓存文件的句柄等都保存在各自的HTTPMsg实例对象中。

CacheInfo对象是管理和存放Raw缓存文件的各项元信息，对外暴露的主要接口是：
    `cache_info_open, cache_info_create, cache_info_close, cache_info_add_frag等`

用户发起Origin文件请求时，先调用cache_info_open打开CacheInfo对象，如果不存在，则在收到Origin的成功响应后，调用cache_info_create创建CacheInfo对象。每次调用cache_info_open时，如果CacheInfo对象已经在内存中，则将count计数加1，只有count计数为0时才可以删除释放CacheInfo对象。当HTTPMsg成功返回给用户后，需要关闭CacheInfo对象，调用cache_info_close，首先将count计数减1，如果count大于0，直接返回不做资源释放。
 
**（2）向Origin服务器转发Proxy代理请求**

eJet收到HTTP客户请求时，如果是Proxy请求，则调用http_proxy_cache_open检测并打开缓存，先根据请求URL对应的HTTPLoc配置信息或正向代理对应的send request配置信息，决定当前代理模式下的HTTP请求是否启用了Cache功能，如果启用了Cache功能，并且Cache File变量设置了正确的Raw缓存文件名，将该缓存文件名保存在HTTPMsg对象的res_file_name中。

检查该缓存文件是否存在，如果存在则直接将该缓存文件返回给客户端即可。注：在没有收到全部字节数据之前Raw缓存文件名是实际缓存文件后加.tmp做扩展名。

如果该文件不存在，以该缓存文件名为参数，调用cache_info_open打开CacheInfo对象，如果不存在缓存信息对象CacheInfo，则返回并直接将客户端请求转发到Origin服务器。

如果存在CacheInfo对象，也就是存在以.tmp为扩展名的Raw缓存文件和以.cacinf为扩展名的缓存信息文件，则判断当前请求的内容（Range规范指定的请求区域）是否全部包含在Raw缓存文件中，如果包含了，则直接将该部分内容返回给客户端，无需向Origin服务器发送HTTP下载请求；如果不包含，则需要向Origin服务器发送请求，但本地缓存中已经有的内容不必重新请求，而是将客户端请求的区域（Range规范指定的范围）中尚未缓存到本地的起始位置和长度计算出来，组成新的Range规范，向Origin发送HTTP请求。
 
**（3）处理Origin服务器返回的响应头**

当HTTP请求转发到Origin服务器并返回响应后，正常情况是将Proxy代理请求HTTPMsg中所有的响应头全部复制一份到源请求HTTPMsg的响应头中，包括状态码也复制过去。

但对于启用了Cache=on并且CacheInfo也已经打开的情况，则需要修正源请求HTTPMsg的响应头，即调用http_cache_response_header来完成：删除掉不必要的响应头，修正HTTP响应体的内容传输格式，即选择Content-Length方式还是Transfer-Encoding: chunked方式，并将状态码修改成206还是200，修改Content-Range的值内容，因为源请求的Range和向Origin服务器发起的Proxy代理请求的Range不一定是一致的。并根据CacheInfo信息决定是否增加Expires和Cache-Control等响应头，等等
 
随后，对Origin服务器返回的HTTP响应头进行解析，调用http_proxy_cache_parse来完成：分别解析Expires、ETag、Last-Modified、Cache-Control等响应头，基于这些响应头信息，再次判断当前响应内容是否需要缓存Cache=on。
 
如果不需要缓存：则将Cache设置为off，并关闭已经打开的CacheInfo（甚至删除掉CacheInfo文件和Raw缓存文件），最主要的是检查源请求的Range范围和Proxy代理请求的Range范围是否一致，如果不一致，则需要重新将源HTTP请求原样再发送一次，并清除当前Proxy代理请求的所有信息。由于将源HTTP请求HTTPMsg中Cache设置为off了，后续重新发送的Proxy代理请求将不启用缓存功能，直接使用实时转发模式。如果两个请求的Range一致，则直接将当前代理请求的响应体内容采用实时转发模式，发送给客户端。
 
如果需要缓存：解析出响应头中的Content-Range中的信息，如果之前用cache_info_open打开CacheInfo对象失败，则此时需调用cache_info_create来创建CacheInfo对象，如果创建失败（内存不够、目录不存在等）则关闭缓存功能，用实时转发模式发送响应。随后，提取此次响应的信息，并保存到CacheInfo对象中，打开或创建Raw缓存文件，最重要的几点是：打开或创建的Raw缓存文件句柄存放在源请求的HTTPMsg中，并将该文件seek写定位到Range或Content-Range头中指定的偏移位置上，在此位置上存放Proxy代理请求中的响应体。最后，将CacheInfo对象的最新内容写入到缓存信息文件中。
 
**（4）存储Origin服务器返回的响应体**

任何开启了Cache功能的HTTP请求，只要请求的内容不在本地缓存中，都需要向Origin服务器以Proxy模式转发HTTP请求，在处理完代理请求的响应头后，需要将响应体存储到Raw缓存文件适当位置，将存储位置信息更新到缓存信息文件中，并启动向客户端发送响应。
 
存储Proxy代理请求的响应体是调用http_proxy_srv_cache_store来实现的：先验证当前源HTTPMsg是否为pipeline后面的请求消息，是否Cache=on等。将代理请求HTTPcon接收缓冲区中的内容作为要存储的响应体内容，进行简单解析判断，
 
（a）如果响应体是Content-Length格式：计算还剩余多少内容没收到，并对比接收缓冲区内容。如果剩余内容为0，则已经全部收到了请求的内容，关闭当前HTTP代理消息，并将res_body_chunk设置为结束。如果还有很多剩余内容没收到，则将接收缓冲区写入到.tmp的Raw缓存文件中，写文件句柄在源HTTPMsg对象中，将写入成功数据块的文件位置和长度信息，追加到CacheInfo对象中，并更新到缓存信息文件里，将代理请求HTTPCon缓冲区中已经写入Raw缓存文件的内容删除掉。最后再判断，刚才从缓冲区追加写入到文件的内容是否全部收齐了，如果收齐了，关闭当前HTTP代理消息。
 
（b）如果响应体是Transfer-Encoding: chunked格式：这种格式并不知道响应体总长度是多少，也不知道剩余还有多少内容，返回的响应体是以一块一块数据块编码方式，每个数据块前面是当前数据块长度（16进制）加上\r\n，每个数据块结尾也加上\r\n为结尾。只有收到一个长度为0的数据块，才知道全部响应体已经结束和收齐了。由于网络传输的复杂性，每次接收数据时，并不一定会完整地收齐一个完整的数据块，所以需要将接收缓冲区的数据交给http_chunk模块判断，是否为接续块、是否收到结尾块等。
 
处理接收缓冲区数据前，先判断是否收齐了全部响应体，如果收齐了，设置res_body_chunk结束状态，关闭当前代理消息。将接收缓冲区的所有内容添加到http_chunk中解析判断，得出缓冲区的内容哪些是接续的数据块，是否收齐等，将接收缓冲区中那些接续数据块部分写入到.tmp的Raw缓存文件中，其中写文件句柄存放在源HTTPMsg对象中，更新总长度，删除接收缓冲区中已经写入的内容，并将写入成功的数据块的文件位置和长度信息，追加到CacheInfo对象中，并更新到缓存信息文件里。最后判断，如果全部数据块都接收齐全了，关闭当前HTTP代理消息，关闭当前HTTP代理消息，同时正式计算并确定当前收齐了所有数据，设置实际的文件长度。
 
（c）最后启动发送缓存文件数据到客户端。
 
**（5）向源HTTPMsg的客户端发送响应**

发送的响应包括响应头和位于缓存文件中的响应体，调用http_proxy_cli_cache_send来处理：
 
通过HTTP的承载协议TCP来发送数据前，需要有序地整理待发送的数据内容，一般情况下，待发送的数据内容包括缓冲区数据、文件数据（完整文件内容、部分文件内容等）、未知的需要网络请求的数据等等，这些数据的总长度有可能知道、也可能不知道，这些待发送数据一般情况下，都位于不同存储位置，譬如在内存中、硬盘上、网络里等，其特点是分布式的、不连续的、碎片化的、甚至内容长度非常大（大到内存都不可能全部容纳的极端情况），管理这些不连续的、碎片化、甚至超大块头数据，是由数据结构chunk_t来实现的。

chunk_t数据结构提供了各类功能接口，包括添加各种数据（内存块、文件名、文件描述符、文件指针等）、有序整理、统一输出、检索等访问接口，最主要的功能是该数据结构解决了不同类别数据整合在一起，模拟成为了一个大缓冲区，大大减少了数据读写拷贝产生的巨额性能开销，大大减少了内存消耗。使用该数据结构，只需将要发送的各种数据内容，通过chunk_t的各类数据追加接口，添加到该数据结构的实例对象中，最后通过tcp_writev或tcp_sendfile来实现数据高效、快速、零拷贝方式的传输发送。
 
基于以上逻辑，向客户端发送数据的主要工作是如何将待发送内容添加到源HTTPMsg中的res_body_chunk中：
 
（a）首先计算出res_body_chunk中累计存放的响应体数据总长度，加上源HTTP请求文件的起始位置（如果有Range取其起始位置，如果没有Range，缺省为0），得到当前要追加发送给客户端的数据在缓存文件中的位置偏移量。分别考虑两种响应体编码格式的处理情况；
 
（b）如果响应体是通过Content-Length来标识：

先用HTTP消息响应总长度减去chunk中的响应体总长度，就计算出剩余的有待添加的数据长度。通过CacheInfo的碎片数据管理接口，查询出当前Raw缓存文件中，以(a)中计算出的缓存文件偏移量位置，查出可用的数据长度有多少。
 
如果Raw缓存文件中存在可用数据，对比剩余数据长度，截取多余部分。将该Raw缓存文件名、文件偏移位置、截取处理过的可用数据长度等作为参数，调用chunk添加数据接口，添加到res_body_chunk中，如果跟chunk中之前存储且未发送出去的数据是接续的，合并处理。如果添加到chunk中的数据总长度达到或超过源请求HTTPMsg消息的响应总长度，则将res_body_chunk设置结束状态，启动TCP发送流程。
 
如果Raw缓存文件中不存在可用数据，则判断是否向Origin服务器发送HTTP代理请求：当前源HTTP请求中没有其他的代理请求存在、Raw缓存文件数据不完整、源HTTP请求的数据范围不在Raw缓存文件中，这三个条件都满足时，则需要向Origin服务器发送HTTP代理请求。这个代理请求是HTTP GET请求，可能跟源HTTP请求方法不一样，只是获取缓存数据的某一部分内容，其Range值是从源请求起始位置开始，去查找实际Raw缓存文件存储情况，得出的空缺处偏移位置。该HTTP代理请求，只负责下载数据存储到本地缓存文件，其响应头信息并不更新到缓存信息文件中。
 
（c）如果响应体的编码格式为Transfer-Encoding: chunked时：

通过CacheInfo的碎片数据管理接口，查询出当前Raw缓存文件中，以(a)中计算出的缓存文件偏移量位置，查出可用的数据长度有多少。
 
如果Raw缓存文件中存在可用数据，将可用数据长度截成最多50个1M大小的数据块，将Raw缓存文件名、1M数据块起始位置、长度作为参数添加到res_body_chunk中。如果添加到chunk中的数据总长度达到或超过源请求HTTPMsg消息的响应总长度，则将res_body_chunk设置结束状态，启动TCP发送流程。
 
如果Raw缓存文件中不存在可用数据，则与上述（b）流程类似。
 
（d）如果源HTTPMsg中统计发送给客户端的响应数据总长度小于res_body_chunk中的总长度，开始发送chunk中的数据。
 
**（6）发送响应给客户端的流程是标准通用的流程**
 
基于HTTP Proxy的缓存数据存储、发送、缓存信息管理维护等功能全部实现完成。这部分文档完稿于2020-11-7 15:17北屋，今天立冬，开始供应暖气。


### 4.21 HTTP Tunnel

HTTP Tunnel是在客户端和Origin服务器之间，通过Tunnel网关，建立传输隧道的通信方式，eJet服务器可以充当HTTP Tunnel网关，分别与客户端和Origin服务器之间建立两个TCP连接，并在这两个连接之间进行数据的实时转发。根据RFC 2616规范，HTTP CONNECT请求方法是建立HTTP Tunnel的基本方式。

HTTP Tunnel最常用的场景是HTTP Proxy正向代理服务器，代理转发客户端https的安全连接请求到Origin服务器，一般情况下，需要采用端到端的TLS/SSL连接，这时，客户端会尝试发送CONNECT方法的HTTP请求，建立一条通过Proxy服务器，到达Origin服务器的连接隧道，即两个TCP连接串联来实时转发数据，通过这个连接隧道，进行TLS/SSL的安全握手、认证、密钥交换、数据加密等，从而实现端到端的安全数据传输。

### 4.22 HTTP Cookie机制

HTTP Cookie是对事务型协议增加会话维持的机制。

eJet系统实现了Set-Cookie的解析和存储处理，每次向Origin发送HTTP请求时，根据request host和path，从Cookie本地存储中，读取相应的Cookie，并将Cookie添加到请求头中。
 
基于Domain和Path来管理不同域名下的Cookie，eJet系统根据Cookie设置不同的Domain域名，来管理Cookie；每个Domain下可以有多个Path，每个Path下可以有多个Cookie对象。由于Cookie有时效性，系统每隔3分钟就扫描一遍Cookie表，将过期的Cookie从系统中删除掉。
 
利用Set-Cookie中的Domain名构建AC Trie反向字典树，利用Domain中的Path构建AC Trie正向字典树。每当发送HTTP请求时，根据Host找到Domain对象，根据request path找到Path对象，将Path对象下处于有效期内的Cookie取出来，拼成Cookie请求头。
 
目前所有的Cookie都存储在一个文件中，理想的实现是建立一个独立的Cookie目录，对每个Domain建立一个Cookie存储文件，将该Domain下不同Path的Cookie以文本方式逐行存储起来。系统在加载期间也不用全部加载这些Cookie文件，而是当访问某个URL时，动态地访问是否存在该Host对应的Domain文件，如果存在Cookie，解析并添加有效Cookie对象。


五. eJet为什么高性能
------

eJet系统具备高性能特征，主要体现在能充分使用CPU运算能力、使用更小的内存开销完成更大数据处理、使用更少的I/O或读写效率更高效等方面。

  运用了哪些技术来支撑大并发访问
  降低单个HTTP请求的内存使用开销，采用chunk_t的不连续碎片管理技术
  大量运用Zero-Copy技术减少冗余拷贝
  采用writev和sendfile等系统调用，减少用户空间和内核空间的数据拷贝，提升发送效率

### 5.1 事件驱动多线程ePump框架


### 5.2 零拷贝Zero-Copy技术

 共采用了哪些Zero-Copy技术提升系统性能


### 5.3 使用writev和sendfile提升发送效率


### 5.4 内存池


### 5.5 超大文件上传
 客户端上传大文件时节省内存空间流程。当客户端上传文件过大时（一般超过128KB），上传内容自动保存到缓存文件里，

基于chunk_t数据结构，借助其文件存储管理功能，可以方便地实现HTTP超大文件的上传，内存数据转存到临时文件中，从而消耗内存会非常的低。可以采用零拷贝技术实现网络数据从内核模式和用户模式之间的交互，等等.


### 5.6 不连续碎片存储读写chunk_t


六. eJet Web服务应用案例
------

### 6.1 大型资源网站


### 6.2 承载PHP应用


### 6.3 充当代理服务器


### 6.4 Web Cache服务


### 6.5 作为CDN边缘分发
 运用反向代理、Cache存储实现CDN边缘分发

### 6.6 应用程序集成eJet
1. 将eJet作为动态库或静态库嵌入应用程序中，通过设置系统回调函数，来处理所有的客户端请求
2. 将eJet作为独立的Web服务器程序运行，应用程序作为动态库，嵌入到eJet中，通过动态库回调机制，执行动态库中编写的处理流程。



七. eJet相关的另外两个开源项目
------

### [adif 项目](https://github.com/kehengzhong/adif)
 
adif 是用标准 c 语言开发的常用数据结构和算法基础库，作为应用程序开发接口基础库，为编写高性能程序提供便利，可极大地缩短软件项目的开发周期，提升工程开发效率，并确保软件系统运行的可靠性、稳定性。adif 项目提供的数据结构和算法库，主要包括基础数据结构、特殊数据结构、常用数据处理算法，常用的字符串、字节流、字符集、日期时间等处理，内存和内存池的分配释放管理，配置文件、日志调试、文件访问、文件缓存、JSon、MIME等管理>，通信编程、文件锁、信号量、互斥锁、事件通知、共享内存等等。
 

### [ePump项目](https://github.com/kehengzhong/epump)
 
依赖于 adif 项目提供的基础数据结构和算法库，作者开发并开源了 ePump 项目。ePump 是一个基于I/O事件通知、非阻塞通信、多> 路复用、多线程等机制开发的事件驱动模型的 C 语言应用开发框架，利用该框架可以很容易地开发出高性能、大并发连接的服务器程 序。ePump 框架负责管理和监控处于非阻塞模式的文件描述符和定时器，根据其状态变化产生相应的事件，并调度派发到相应线程的事件队列中，这些线程通过调用该事件关联的回调函数（Callback）来处理事件。ePump 框架封装和提供了各种通信和应用接口，并融合了当今流行的通信和线程架构模型，是一个轻量级、高性能的 event-driven 开发架构，利用 ePump，入门级程序员也能轻易地开发出商业级的高性能服务器系统。 

***

八. 关于作者 老柯 (laoke)
------

有大量Linux等系统上的应用平台和通信系统开发经历，是资深程序员、工程师，发邮件kehengzhong@hotmail.com可以找到作者，或者通过QQ号码[571527](http://wpa.qq.com/msgrd?V=1&Uin=571527&Site=github.com&Menu=yes)或微信号[beijingkehz](http://wx.qq.com/)给作者留言。

eJet Web服务器项目是作者三个关联开源项目的第三个项目，以事件驱动模型、多线程、大并发连接等为特征的轻量级的高性能Web服务器，完全依赖于前两个项目的基础能力，在嵌入式Web服务器、Cache、CDN等应用领域，有广阔的前景。本项目源自于2003年作者开发完成的HTTP服务器项目，并应用于各类需要运用HTTP协议的通信系统中，这些功能的开发断断续续进行中，一直没有中断。

