# DS-云原生环境中的DevOps

> 主题库：分布式系统　｜　源文件：DS-云原生环境中的DevOps.pdf　｜　1,903 字符　｜　25 个片段　｜　更新于 2026-11-11


Distributed Systems
分布式系统
陈鹏飞
计算机学院
chenpf7@mail.sysu.edu.cn
办公室：学院楼310
主页：http://sdcs.sysu.edu.cn/node/3747

2
补充内容
—
云原生系统中的
DevOps
实
践

3
背景知识
1

4
IaaS
SaaS
PaaS
1
、软、硬件资源的按需使用；
2
、软、硬件资源的服务化；
云计算
云计算

5
https://www.cncf.io/
云原生系统
云原生技术帮助公司和机构在公有云、私有云和混合云等新型动态环境中，构建和
运行可弹性扩展的应用。云原生的代表技术包括容器、服务网格、微服务、不可变
基础设施和声明式
API
。

6
云原生系统的能力

7
微服务
•什么是微服务
微服务架构风格是指通过开发一组微型服务来组成一个单一应用的方法。这每一
个小的服务都自成一体，运行在自己的进程里，彼此之间通过轻量级的机制，通
常是HTTP资源API的方式，来进行通信。这些小服务一般基于既定的业务能力范
围来进行构建，通过完全自动化部署的机制来进行独立部署。
In short, the microservice architectural style is an approach to developing a 
single application as a suite of smallservices, each running in its own proces
s and communicating with lightweight mechanisms, often an HTTP resource 
API. These services are built around business capabilities and independently 
deployablebyfully automated deployment machinery. 
—Martin Fowler & James Lewis.
https://martinfowler.com/articles/microservices.html

8
微服务

9
容器

10
连续集成和部署
GitLab 是一个用于仓库管理系统的开源项目，使用Git作为代码
管理工具，并在此基础上搭建起来的web服务。安装方法是参
考GitLab在GitHub上的Wiki页面。

11
传统开发过程

12
现代开发过程

13

14
DevOps
DevOps是一组过程、方法与系统的统称，用于促进开发（应用程序/
软件工程）、技术运营和质量保障（QA）部门之间的沟通、协作与整
合。

15
DevOps原则
Ø
基础设施即代码（
Infrastructure as Code
）
Ø
持续交付（
Continuous Delivery
）
Ø
协同工作（
Culture of Collaboration
）

16
参考资料
phttps://devops.com/is-noops-the-future-of-cloud-networking/
phttps://pivotal.io/cn/devops/resources
phttps://puppet.com/resources/whitepaper

17
背景知识
IDE
代码库
/CI/CD
云平台

18
Kubernetes
Kubernetesis an open-source system for automating deployme
nt, scaling, and management of containerized applications.

19
DevOps流程
1.gitlab-runner;
2.Container repository 
开发/部署/运行
环境搭建
代码编写
代码编写
CI/CD工作流
程序测试
Gitlab-runner
镜像创建/存储
应用部署
服务就绪

20
系统部署-1
连续集成

21
系统部署-2
开发终端
连续集成

22
系统部署-3
开发终端
连续集成

23
系统部署-4
开发终端
连续集成

24
系统部署
Master
Worker
222.200.180.196
kubectl
Gitlab-runner
222.200.180.179
Gitlab服务器
Container Repository
开发终端

25
Demo
