# DS-CH-12-分布式Web

> 主题库：分布式系统　｜　源文件：DS-CH-12-分布式Web.pdf　｜　3,660 字符　｜　22 个片段　｜　更新于 2026-11-11


Distributed Systems
分布式系统
陈鹏飞
计算机学院
chenpf7@mail.sysu.edu.cn
办公室：学院楼310
主页：http://sdcs.sysu.edu.cn/node/3747

2
第十二讲
—
分布式
Web
系统

3
Distributed Web-Based Systems
ØEssence
The WWW is a huge client-server system with millions of servers; each server 
hosting thousands of hyperlinked documents:
Ø Documents are generally represented in text (plain text, HTML, XML)
ØAlternative types: images, audio, video, but also applications (PDF, PS)
ØDocuments contain scripts that are executed by the client-side software

4
Multi-tiered Architectures
ØObservation
Conventionally, web sites were organized into three tiers.

5
Web services
ØObservation
At a certain point, people started recognizing that it is was more than ju
st user  <->site interaction: sites could offer services to other sites => sta
ndardization is then badly needed.

6
Clients: Web browsers
ØObservation
browsers form the Web’s most important client-side software. They are 
used to be simple, but that is long ago.

7
Apache Web Server
ØObservation
More than 70% of all Web sites are based on Apache. The server is int
ernally organized more or less according to the steps needed to process 
an HTTP request:

8
Server Clusters (2/2)
ØQuestion
Why can content-aware distribution be so much better?

9
Communication (1/2)
ØEssence
Communication in the Web is generally based on HTTP; a relatively sim
ple client-server transfer protocol having the following request messages:

10
Communication (2/2)

11
SOAP
Simple Object Access Protocol: Based on XML, this is the standar
d protocol for communication between Web services.
ØSOAP is bound to an underlying protocol (i.e., it is not indepe
ndent from its carrier, HTTP、SMTP)
ØConversational exchange style: Send a document one way, get 
a filled-in response back.
ØRPC-style exchange: Used to invoke a Web service.

12
A Note on XML
ØObservation
XML has the advantage of allowing self describing documents. 

13
Naming: URL
ØURL
Uniform Resource Locator tells how and where to access a resource.

14
Web Proxy Caching
ØBasic Idea
Sites install a separate proxy server that handles all outgoing requests. 
Proxies subsequently cache incoming documents. Cache-consistency pro
tocols:

15
Web Proxy Caching

16
Replication in Web Hosting Systems
ØObservation
Sites install a separate proxy server that handles all outgoing requests. 
Proxies subsequently cache incoming documents. Cache-consistency pro
tocols:

17
Handling Flash Crowds
ØObservation
We need dynamic adjustment to balance resource usage. Flash crowd
s introduce a serious problem:

18
Server Replication
ØContent Delivery Network
CDNs act as Web hosting services to replicate documents across the I
nternet providing their customers guarantees on high availability
and performance (example: Akamai).

19
Replication of Web Apps. (1/3)
ØObservation
Replication becomes more difficult when dealing with databsesand suc
h. No single best solution.
Assumption: Updates are carried out at origin server, and propagated to 
edge servers.

20
Replication of Web Apps. (2/3)
ØFull replication: high read/write ratio, often in combination with co
mplex queries. Note: replication may possibly speed-down performa
nce when R/W ratio goes down.
ØPartial replication: high read/write ratio, but in combination with sim
ple queries

21
Replication of Web Apps. (3/3)
Ø Content-aware caching: Check for queries at local database, and 
subscribe for invalidations at the server. Works good with range qu
eries and complex queries.
ØContent-blind caching: Simply cache the result of previous querie
s. Works great with simple queries that address unique results (e.g., 
no range queries).

22
谢谢！
