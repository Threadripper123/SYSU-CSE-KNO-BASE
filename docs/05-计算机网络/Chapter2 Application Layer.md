# Chapter2 Application Layer

> 主题库：计算机网络　｜　源文件：Chapter2 Application Layer.pdf　｜　50,473 字符　｜　133 个片段　｜　更新于 2026-11-11


Computer Networking
谢逸
中山大学·计算机学院
2024. Fall

Chapter 2
Application Layer
2-2
A note on the use of these PowerPoint slides:
We’re making these slides freely available to all (faculty, students, 
readers). They’re in PowerPoint form so you see the animations; 
and can add, modify, and delete slides  (including this one) and 
slide content to suit your needs. They obviously represent a lotof 
work on our part. In return for use, we only ask the following:
If you use these slides (e.g., in a class) that you mention their 
source (after all, we’d like people to use our book!)
If you post any slides on a www site, that you note that they 
are adapted from (or perhaps identical to) our slides, and note 
our copyright of this material.
For a revision history, see the slide note for this page. 
Thanks and enjoy!  JFK/KWR
All material copyright 1996-2023
J.F Kurose and K.W. Ross, All Rights Reserved
Computer Networking: 
A Top-Down Approach 
8
th
edition 
Jim Kurose, Keith Ross
Pearson, 2020

Assignments :
Ch2 (ver8): 4,7,8,10,13,15,18,23,24,26,27
A group project
Key words: 
client-server architecture, Transport Services, HTTP, Cookies, 
Non-Persistent and Persistent Connections, Caching and proxy, 
FTP, SMTP, POP3, IMAP, DNS, P2P Architectures
3

2-4
Chapter 2: outline
2.1 principles of network 
applications
2.2 Web and HTTP
2.3 FTP 
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 socket programming with 
UDP and TCP

2-5
Chapter 2: application layer
our goals:
conceptual, implementation 
aspects of network application 
protocols
transport-layer service models
client-server paradigm
peer-to-peer paradigm
learn about protocols by 
examining popular application-
level protocols
HTTP
FTP
SMTP / POP3 / IMAP
DNS
creating network applications
socket API
Application
transport

2-6
Some network apps
e-mail
web
text messaging
remote login
P2P file sharing
multi-user network games
streaming stored video 
(YouTube, Hulu, Netflix) 
voice over IP (e.g., Skype)
real-time video 
conferencing
social networking
search
...

2-7
Creating a network app
write programs that:
run on (different) end systems
communicate over network
e.g., web server software communicates with 
browser software
no need to write software for network-core 
devices
network-core devices do not run user applications 
applications on end systems  allows for rapid app 
development, propagation
application
transport
network
data link
physical
application
transport
network
data link
physical
application
transport
network
data link
physical

2-8
Application architectures
possible structure of applications:
client-server (C/S)
peer-to-peer (P2P)

2-9
Client-server architecture
server: 
always-on host
permanent IP address
data centers for scaling
clients:
communicate with server
may be intermittently connected
may have dynamic IP addresses
do not communicate directly with each other
client/server

2-10
P2P architecture
noalways-on server
arbitrary end systems directly 
communicate
peers request service from other peers, 
provide service in return to other peers
self scalability– new peers bring 
new service capacity, as well as 
new service demands
peers are intermittently connected and 
change IP addresses
complex management
peer-peer

2-11
Processes communicating
process:program running 
within a host
within same host, two processes 
communicate using  inter-process 
communication(defined by OS)
processes in different hosts 
communicate by exchanging 
messages
client process:
process that initiates
communication
server process:
process that waits to 
be contacted
aside: applications with P2P 
architectures have client 
processes & server processes
clients, servers
Communication: Process-To-Process

2-12
Addressing processes
to receive messages, process  must 
have identifier
host device has unique 32-bit IP 
address
Q:does  IP address of host on which 
process runs suffice for identifying 
the process?
example port numbers:
HTTP server: 80
mail server: 25
to send HTTP message to 
gaia.cs.umass.edu web server:
IP address:128.119.245.12
port number:80
A:no, manyprocesses can be 
running on same host
identifier includes both IPaddress 
and Portnumbers associated with 
process on host.
IPaddress 
Portnumbers 

2-13
SocketsWhat is Socket?
process sends/receives messages to/from its socket
socket analogous to door
sending process shoves message out door
sending process relies on transport infrastructure on other side of 
door to deliver message to socket at receiving process
App-layer process 
sends  data
Data packaging 
identification
Sockets=
IP + Port
Internet
Data packaging 
identification
Sockets=
IP + Port
OS
OS
Host 1
Host 2
App-layer process 
gets  data

2-14
SocketsWhat is Socket?
The essence of a socket is a special buffer for a process to 
send/receive data;
The definition of a socket is related to its function
Sending: 
Process A
OS
buffer
Socket for A
Label: srcPort+dstIP+dstPort
IPs
Questions:
•What happens when a process sends 
data to multiple destination processes  
on different hosts at the same time?
•What happens when multiple 
processes on the same host send data 
to the same program on the same host 
at the same time?
Virtual Receiving 
Point/mirror point 
of receiving

2-15
SocketsWhat is Socket?
The essence of a socket is a special buffer for a process to 
send/receive data;
The definition of a socket is related to its function
Receiving   (I): 
Process B
OS
buffer
Socket for B
Label: dstPort
IPs
Receiving   (II): 
Process B
OS
buffer
Socket for B
IPs
Label: dstPort+srcIP+srcPort
What is the difference?
Virtual sending 
Point/Mirror 
Point of sending
Virtual sending 
Point/mirror 
point of sending

2-19
App-layer protocol defines
types of messages exchanged,
e.g., request, response 
message syntax:
what fields in messages & how 
fields are delineated
message semantics
meaning of information in fields
rulesfor when and how processes 
send & respond to messages
open protocols:
defined in RFCs
allows for interoperability
e.g., HTTP, SMTP
proprietary protocols:
e.g., Skype, QQ

2-20
What transport service does an app need?
data integrity
some apps (e.g., file transfer, web 
transactions) require 100% reliable 
data transfer
other apps (e.g., audio) can tolerate 
some loss
timing
some apps (e.g., Internet 
telephony, interactive games) 
require low delay to be 
“effective”
throughput
some apps (e.g., multimedia) require 
minimum amount of throughput to be 
“effective”
other apps (“elastic apps”) make 
use of whatever throughput they get 
security
encryption, data integrity, 
...
Application
transport

2-21
Transport service requirements: common apps
application
file transfer
e-mail
Web documents
real-time audio/video
stored audio/video
interactive games
text messaging
data loss
no loss
no loss
no loss
loss-tolerant
loss-tolerant
loss-tolerant
no loss
throughput
elastic
elastic
elastic
audio: 5kbps-1Mbps
video:10kbps-5Mbps
same as above 
few kbps up
elastic
time sensitive
no
no
no
yes, 100’s msec
yes, few secs
yes, 100’s msec
yes and no

2-22
Internet transport protocols services
TCP service:
reliable transportbetween sending 
and receiving process
flow control:sender won’t 
overwhelm receiver 
congestion control:throttle sender 
when network overloaded
connection-oriented:setup 
required between client and server 
processes
DOES NOT provide:timing, 
minimum throughput guarantee, 
security
UDP service:
unreliable data transferbetween 
sending and receiving process
does not provide:reliability, flow 
control, congestion control, 
timing, throughput guarantee, 
security, or connection setup, 
Q:why bother?  Why is there a UDP?

2-23
Internet apps:  application, transport protocols
application
e-mail
remote terminal access
Web 
file transfer
streaming multimedia
Internet telephony
application
layer protocol
SMTP [RFC 2821]
Telnet [RFC 854]
HTTP [RFC 2616]
FTP [RFC 959]
HTTP (e.g., YouTube), 
RTP [RFC 1889]
SIP, RTP, proprietary
(e.g., Skype)
underlying
transport protocol
TCP
TCP
TCP
TCP
TCP or UDP
TCP or UDP

Securing TCP
TCP & UDP 
no encryption
Clear-text passwords sent 
into socket traverse Internet  
in clear-text
SSL
provides encrypted TCP 
connection
data integrity
end-point authentication
SSL is at app layer
Apps use SSL libraries, which 
“talk”to TCP
SSL socket API
Clear-text passwords sent into 
socket traverse Internet  
encrypted 
See Chapter 7
2-24
应用层
SSL
TCP

2-25
Chapter 2: outline
2.1 principles of network 
applications
app architectures
app requirements
2.2 Web and HTTP
2.3 FTP 
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 socket programming 
with UDP and TCP

2-26
Web and HTTP
First, a review...
web pageconsists of objects
object can be HTML file, JPEG image, 
Java applet, audio file,...
web page consists of base HTML-file
which includes several referenced 
objects
each object is addressable by a URL, 
e.g.,
www.someschool.edu/someDept/pic.gif
host name
path name

2-27
HTTP overview
HTTP: hypertext transfer protocol
Web’s application layer protocol
client/server model
client:browser that requests, receives, 
(using HTTP protocol) and “displays”
Web objects 
server:Web server sends (using HTTP 
protocol) objects in response to requests
PC running
Firefox browser
server 
running
Apache Web
server
iphone running
Safari browser
http/1.1 (rfc2616)

2-28
HTTP overview (continued)
uses TCP:
client initiates TCP connection (creates 
socket) to server,  port 80
server accepts TCP connection from 
client
HTTP messages (application-layer 
protocol messages) exchanged between 
browser (HTTP client) and Web server 
(HTTP server)
TCP connection closed
HTTP is “stateless”
server maintains no 
information about past client 
requests
protocols that maintain 
“state”are complex!
past history (state) must be maintained
if server/client crashes, their views of 
“state”may be inconsistent, must be 
reconciled
aside

2-29
HTTP connections
non-persistent HTTP
at most one object sent over 
one TCP connection
connection then closed
downloading multiple objects 
required multiple 
connections
persistent HTTP
multiple objects can be 
sent over single TCP 
connection between 
client, server

2-30
Non-persistent HTTP
suppose user enters URL:
1a.HTTP client initiates TCP 
connection to HTTP server 
(process) at 
www.someSchool.edu on 
port 80
2.HTTP client sends HTTP request 
message(containing URL) into 
TCP connection socket. 
Message indicates that client 
wants object 
someDepartment/home.index
1b.HTTP server at host 
www.someSchool.edu waiting 
for TCP connection at port 80.  
“accepts”connection, 
notifying client
3.HTTP server receives request 
message, forms response 
messagecontaining requested 
object, and sends message into 
its socket
time
(contains text,
references to 10 
jpeg images)
www.someSchool.edu/someDepartment/home.index

2-31
Non-persistent HTTP (cont.)
5.HTTP client receives response 
message containing html file, 
displays html.  Parsing html 
file, finds 10 referenced jpeg  
objects
6.Steps 1-5 repeated for eachof 
10 jpeg objects
4.HTTP server closes TCP 
connection. 
time

2-32
Non-persistent HTTP: response time
RTT (definition):time for a small packet to 
travel from client to server and back
HTTP response time:
one RTT to initiate TCP connection
one RTT for HTTP request and first few 
bytes of HTTP response to return
file transmission time
non-persistent HTTP response time =   
2RTT+ file transmission  time
time to 
transmit 
file
initiate TCP
connection
RTT
request
file
RTT
file
received
time
time

2-33
Persistent HTTP
non-persistent HTTP issues:
requires 2 RTTs per object
OS overhead for eachTCP 
connection
browsers often open parallel TCP 
connections to fetch referenced 
objects
persistent  HTTP:
server leaves connection open after 
sending response
subsequent HTTP messages  
between same client/server sent 
over open connection
client sends requests as soon as it 
encounters a referenced object
as little as one RTT for all the 
referenced objects

36
non-persistent HTTP 

37
会出现阻塞
因为TCP导致排头阻塞，
HTTP3 采用UDP

2-38
HTTP request message
two types of HTTP messages: request, response
HTTP request message:
ASCII (human-readable format)
request line
(GET, POST, 
HEAD commands)
header
lines
carriage return, 
line feed at start
of line indicates
end of header lines
GET /index.html HTTP/1.1\r\n
Host: www-net.cs.umass.edu\r\n
User-Agent: Firefox/3.6.10\r\n
Accept: text/html,application/xhtml+xml\r\n
Accept-Language: en-us,en;q=0.5\r\n
Accept-Encoding: gzip,deflate\r\n
Accept-Charset: ISO-8859-1,utf-8;q=0.7\r\n
Keep-Alive: 115\r\n
Connection: keep-alive\r\n
\r\n
carriage return character
line-feed character

2-39
HTTP request message: general format
request
line
header
lines
body
method
sp
sp
cr
lf
version
URL
cr
lf
value
header field name
cr
lf
value
header field name
~
~
~
~
cr
lf
entity body
~
~
~
~

2-40
Uploading form input
POST method:
web page often includes form input
input is uploaded to server in entity body
Create new items
URL method:
uses GET method
input is uploaded in 
URL field of request 
line:
www.somesite.com/animalsearch?monkeys&banana

2-41
Method types
HTTP/1.0:
GET
POST
HEAD
asks server to check requested 
object without responding it.
HTTP/1.1:
GET, POST, HEAD
PUT
uploads file in entity body to 
path specified in URL field
Modify (idempotent)
DELETE
deletes file specified in the URL 
field

2-42
HTTP response message
status line
(protocol
status code
status phrase)
header
lines
data, e.g., 
requested
HTML file
HTTP/1.1 200 OK\r\n
Date: Sun, 26 Sep 2010 20:09:20 GMT\r\n
Server: Apache/2.0.52 (CentOS)\r\n
Last-Modified: Tue, 30 Oct 2007 17:00:02 
GMT\r\n
ETag: "17dc6-a5c-bf716880"\r\n
Accept-Ranges: bytes\r\n
Content-Length: 2652\r\n
Keep-Alive: timeout=10, max=100\r\n
Connection: Keep-Alive\r\n
Content-Type: text/html; charset=ISO-8859-
1\r\n
\r\n
data data data data data ... 

2-43
HTTP response status codes
200 OK
request succeeded, requested object later in this msg
301 Moved Permanently
requested object moved, new location specified later in this 
msg (Location:)
400 Bad Request
request msg not understood by server
404 Not Found
requested document not found on this server
505 HTTP Version Not Supported
status code appears in 1st line in server-to-
client response message.
some sample codes:

2-44
Trying out HTTP (client side) for yourself
1. Telnet to your favorite Web server:
opens TCP connection to port 80
(default HTTP server port) at cis.poly.edu.
anything typed in sent 
to port 80 at cis.poly.edu
telnet cis.poly.edu 80
2. type in a GET HTTP request:
GET /~ross/ HTTP/1.1
Host: cis.poly.edu
by typing this in (hit carriage
return twice), you send
this minimal (but complete) 
GET request to HTTP server
3. look at response message sent by HTTP server!
(or use Wireshark to look at captured HTTP request/response)

2-45
User-server state: cookies
many Web sites use cookies
four components:
1) cookie header line of HTTP 
responsemessage
2) cookie header line in next HTTP 
requestmessage
3) cookie file kept on user’s host, 
managed by user’s browser
4) back-end database at Web site
example:
Susan always access Internet from 
PC
visits specific e-commerce site for 
first time
when initial HTTP requests arrives 
at site, site creates: 
unique ID
entry in backend database for ID

2-46
Cookies: keeping “state”(cont.)
client
server
usual http response msg
usual http response msg
cookie file
one week later:
usual http request msg
cookie: 1678
cookie-
specific
action
access
ebay 8734
usual http request msg
Amazon server
creates ID
1678 for user
create
entry
usual http response 
set-cookie: 1678
ebay 8734
amazon 1678
usual http request msg
cookie: 1678
cookie-
specific
action
access
ebay 8734
amazon 1678
backend
database
2023.9.11

2-47
Cookies (continued)
what cookies can be used for:
authorization
shopping carts
recommendations
user session state (Web e-mail)
cookies and privacy:
cookies permit sites to 
learn a lot about you
you may supply name 
and e-mail to sites
aside
how to keep “state”:
protocol endpoints: maintain state at sender/receiver 
over multiple transactions
cookies: http messages carry state

2-48
Web caches (proxy server)
user sets browser: Web 
accesses via  cache
browser sends all HTTP 
requests to cache
object in cache: cache 
returns object 
else cache requests 
object from origin 
server, then returns 
object to client
goal:satisfy client request without 
involving origin server (就近访问)
Internet
Internet

2-49
More about Web caching
cache acts as both client 
and server
server for original requesting client
client to origin server
typically cache is installed 
by ISP (university, 
company, residential ISP)
why Web caching?
reduce response time for client 
request
reduce traffic on an institution’s 
access link
Internet dense with caches: 
enables “poor”content 
providers to effectively deliver 
content (so too does P2P file 
sharing)

2-50
Caching example: 
assumptions:
avg object size: 1M bits
avg request rate from browsers to origin servers:15/sec
avg data rate to browsers: 15 Mbps
RTT from institutional router to any origin server: 2 sec
access link rate: 15 Mbps
consequences:
LAN utilization: 15%
access link utilization = 99%
total delay   = Internet delay + access delay + LAN delay
=  2 sec+ minutes+ usecs
problem!
瓶颈在哪里？
(15 requests/sec)  (1 Mbits/request)/(100 Mbps) = 0.15
(15 requests/sec)  (1 Mbits/request)/(15 Mbps) = 1

2-51
assumptions:
avg object size: 1M bits
avg request rate from browsers to origin servers:15/sec
avg data rate to browsers: 15 Mbps
RTT from institutional router to any origin server: 2 sec
access link rate: 15 Mbps
consequences:
LAN utilization: 15%
access link utilization = 99%
total delay   = Internet delay + access delay + LAN delay
=  2 sec + minutes + usecs
Caching example: fatter access link 
100Mbps
100 Mbps
msecs
Cost:increased access link 
speed (not cheap!)
15%

2-52
Caching example: install local cache 
assumptions:
avg object size: 1M bits
avg request rate from browsers to origin 
servers:15/sec
avg data rate to browsers: 15 Mbps
RTT from institutional router to any origin server: 2 
sec
access link rate: 15 Mbps
consequences:
LAN utilization: 15%
access link utilization = ?
total delay   = Internet delay + access delay + LAN 
delay
=  2 sec + minutes + usecs
?
?
How to compute link 
utilization, delay?
Cost:web cache (cheap!)

2-53
Caching example: install local cache 
Calculating access link utilization, delay 
with cache:
suppose cache hit rate is 0.4
40% requests satisfied at cache, 60% requests satisfied 
at origin 
access link utilization: 
60% of requests use access link 
data rate to browsers over access link = 0.6*15 Mbps = 9 Mbps 
utilization = 9/15 = 0.6
total delay
= 0.6 * (delay from origin servers) +0.4 * (delay when 
satisfied at cache)
= 0.6 (2+0.01) + 0.4 (~msecs~0.01s) 
= ~ 1.2 secs
less than with 100Mbps link (and cheaper too!)

2-54
Conditional GET 
Goal:don’t send object if cache 
has up-to-date cached version
no object transmission delay
lower link utilization
cache:specify date of cached copy 
in HTTP request
If-modified-since: <date>
server:response contains no 
object if cached copy is up-to-date: 
HTTP/1.0 304 Not Modified
HTTP request msg
If-modified-since: <date>
HTTP response
HTTP/1.0 
304 Not Modified
object 
not 
modified
before
<date>
HTTP request msg
If-modified-since: <date>
HTTP response
HTTP/1.0 200 OK
<data>
object 
modified
after 
<date>
client
server

HTTP/2
Key goal: decreased delay in multi‐object HTTP requests
HTTP1.1:introduced multiple, pipelined GETsover single TCP 
connection
server responds in‐order(FCFS: first‐come‐first‐served scheduling) to 
GET requests
with FCFS, small object may have to wait for transmission  (head‐of‐
line (HOL) blocking) behind large object(s)
loss recovery (retransmitting lost TCP segments) stalls object 
transmission
Application Layer: 2-55

HTTP/2
HTTP/2:[RFC 7540, 2015] increased flexibility at serverin sending objects 
to client:
methods, status codes, most header fields unchanged from HTTP 1.1
transmission order of requested objects based on client‐specified object 
priority 
(not necessarily FCFS)
pushunrequested objects to client
divide objects into frames, schedule frames to mitigate HOL blocking
Key goal: decreased delay in multi‐object HTTP requests
Application Layer: 2-56

HTTP/2: mitigating HOL blocking
HTTP 1.1: client requests 1 large object (e.g., video file) and 3 smaller objects
client
server
GET O
1
GET O
2
GET O
3
GET O
4
O
1
O
2
O
3
O
4
object data requested
O
1
O
2
O
3
O
4
objects delivered in order requested: O
2
, O
3
, O
4
wait behind O
1
Application Layer: 2-57

HTTP/2: mitigating HOL blocking
HTTP/2: objects divided into frames, frame transmission interleaved
client
server
GET O
1
GET O
2
GET O
3
GET O
4
O
2
O
4
object data requested
O
1
O
2
O
3
O
4
O
2
, O
3
, O
4
delivered quickly, O
1 
slightly delayed
O
3
O
1
Application Layer: 2-58

HTTP/2 to HTTP/3
HTTP/2 over single TCP connection means:
recovery from packet loss still stalls all object transmissions
•as in HTTP 1.1, browsers have incentive to open multiple parallel 
TCP connections to reduce stalling, increase overall throughput
no security over vanilla TCP connection
HTTP/3: adds security, per object error‐and congestion‐
control (more pipelining) over UDP
•more on HTTP/3 in transport layer
Application Layer: 2-59

2-60
Chapter 2: outline
2.1 principles of 
network applications
app architectures
app requirements
2.2 Web and HTTP
2.3 FTP
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 socket 
programming 
with UDP and 
TCP

2-61
FTP: the file transfer protocol
file transfer
FTP
server
FTP
user
interface
FTP
client
local file
system
remote file
system
user 
at host
transfer file to/from remote host
client/server model
client:side that initiates transfer (either to/from remote)
server:remote host
ftp: RFC 959
ftp server: port 21

2-62
FTP: separate control, data connections
FTP client contacts FTP server at port 
21, using TCP 
client authorized over control 
connection
client browses remote directory, sends 
commands over control connection
when server receives file transfer 
command, serveropens 2
nd
TCP data 
connection (for file) to client
after transferring one file, server closes 
data connection
FTP
client
FTP
server
TCP control connection,
server port 21
TCP data connection,
server port 20
server opens another TCP data 
connection to transfer another file
control connection: “out of 
band”
FTP server maintains “state”: 
current directory, earlier 
authentication

2-63
FTP commands, responses
sample commands:
sent as ASCII text over control 
channel
USER username
PASS password
LISTreturn list of file in current 
directory
RETR filenameretrieves (gets) 
file
STOR filenamestores (puts) 
file onto remote host
sample return codes
status code and phrase (as in 
HTTP)
331 Username OK, password 
required
125 data connection already 
open; transfer starting
425 Can’t open data 
connection
452 Error writing file

2-64
Chapter 2: outline
2.1 principles of network 
applications
app architectures
app requirements
2.2 Web and HTTP
2.3 FTP 
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 socket 
programming 
with UDP and 
TCP

2-65
Electronic mail
Three major components:
user agents 
mail servers 
simple mail transfer protocol: SMTP
User Agent
a.k.a. “mail reader”
composing, editing, reading mail 
messages
e.g., Outlook, Thunderbird, iPhone
mail client
outgoing, incoming messages 
stored on server
user mailbox
outgoing 
message queue
mail
server
mail
server
mail
server
SMTP
SMTP
SMTP
user
agent
user
agent
user
agent
user
agent
user
agent
user
agent

2-66
Electronic mail: mail servers
mail servers:
mailboxcontains incoming messages for 
user
message queueof outgoing (to be sent) 
mail messages
SMTP protocolbetween mail servers to 
send email messages
client: sending mail server
“server”: receiving mail server
mail
server
mail
server
mail
server
SMTP
SMTP
SMTP
user
agent
user
agent
user
agent
user
agent
user
agent
user
agent

SMTP RFC (5321)
uses TCP to reliably transfer email message 
from client (mail server initiating 
connection) to server, port 25
direct transfer: sending server (acting like client) 
to receiving server
three phases of transfer
•SMTP handshaking (greeting)
•SMTP transfer of messages
•SMTP closure
command/response interaction (like HTTP)
•commands:ASCII text
•response:status code and phrase
initiate TCP
connection
RTT
time
220
250 Hello
HELO
SMTP 
handshaking
TCP connection
initiated
“client”
SMTP server
“server”
SMTP server
SMTP 
transfers
Application Layer: 2-67

2-68
user
agent
Scenario: Alice sends message to Bob
1) Alice uses UA to compose message 
“to”bob@someschool.edu
2) Alice’s UA sends message to her 
mail server; message placed in 
message queue
3) client side of SMTP opens TCP 
connection with Bob’s mail server
4) SMTP client sends Alice’s message 
over the TCP connection
5) Bob’s mail server places the message 
in Bob’s mailbox
6) Bob invokes his user agent to read 
message
mail
server
mail
server
1
2
3
4
5
6
Alice’s mail server
Bob’s mail server
user
agent
SMTP
SMTP

2-69
Sample SMTP interaction
S: 220 hamburger.edu 
C: HELOcrepes.fr 
S: 250  Hello crepes.fr, pleased to meet you 
C: MAIL FROM: <alice@crepes.fr> 
S: 250 alice@crepes.fr... Sender ok 
C: RCPT TO: <bob@hamburger.edu> 
S: 250 bob@hamburger.edu ... Recipient ok 
C: DATA
S: 354 Enter mail, end with "." on a line by itself 
C: Do you like ketchup? 
C: How about pickles? 
C: .
S: 250 Message accepted for delivery 
C: QUIT
S: 221 hamburger.edu closing connection

2-70
Try SMTP interaction for yourself:
telnet servername 25
see 220 reply from server
enter HELO, MAIL FROM, RCPT TO, DATA, QUIT commands
above lets you send email without using email client (reader)

2-71
SMTP: final words
SMTP uses persistent connections
SMTP requires message (header & 
body) to be in 7-bit ASCII
SMTP server uses CRLF.CRLFto 
determine end of message
comparison with HTTP:
HTTP: pull
SMTP: push
both have ASCII command/response 
interaction, status codes
HTTP: each object encapsulated in 
its own response msg
SMTP: multiple objects sent in 
multipart msg

2-72
Mail message format
SMTP: protocol for exchanging 
email msgs
RFC 822: standard for text 
message format:
header lines, e.g.,
To:
From:
Subject:
differentfrom SMTP MAIL 
FROM, RCPT TO:
commands!
Body: the “message”
ASCII characters only
header
body
blank
line

2-73
Mail access protocols
SMTP:delivery/storage to receiver’s server
mail access protocol: retrieval from server
POP:Post Office Protocol [RFC 1939]: authorization, download 
IMAP:Internet Mail Access Protocol [RFC 1730]: more features, including 
manipulation of stored msgs on server
HTTP:gmail, Hotmail, Yahoo! Mail, etc.
Sender’s mail 
server
SMTP
SMTP
mail access
protocol
Receiver’s mail 
server
(e.g., POP, 
IMAP
)
user
agent
user
agent

2-74
POP3 protocol
authorization phase
client commands: 
user:declare username
pass:password
server responses
+OK
-ERR
transaction phase,client:
list:list message numbers, size
retr:retrieve message by number
dele:delete
quit
C: list 
S: 1 498 
S: 2 912 
S: . 
C: retr 1 
S: <message 1 contents>
S: . 
C: dele 1 
C: retr 2 
S: <message 1 contents>
S: . 
C: dele 2 
C: quit 
S: +OK 
POP3 server signing off
S: +OK POP3 server ready 
C: user bob 
S: +OK 
C: pass hungry 
S: +OKuser successfully logged on

2-75
POP3 (more) and IMAP
more about POP3
previous example uses POP3 
“download and delete”mode
Bob cannot re-read e-mail if he 
changes client
POP3 “download-and-keep”: 
copies of messages on different 
clients
POP3 is stateless across sessions
IMAP
keeps all messages in one place: at 
server
allows user to organize messages 
in folders
keeps user state across sessions:
names of folders and mappings 
between message IDs and folder 
name

2-76
Chapter 2: outline
2.1 principles of network 
applications
app architectures
app requirements
2.2 Web and HTTP
2.3 FTP 
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 socket 
programming 
with UDP and 
TCP

2-77
DNS: domain name system
people:many identifiers:
SSN, name, passport #
Internet hosts, routers:
IP address (32 bit) - used for 
addressing datagrams
“name”, e.g., www.yahoo.com -
used by humans
Q:how to map between IP address 
and name, and vice versa ?
Domain Name System:
distributed databaseimplemented in 
hierarchy of many name servers
application-layer protocol:hosts, 
name servers communicate to 
resolvenames (address/name 
translation)
note: core Internet function, 
implemented as application-layer 
protocol
complexity at network’s “edge”

2-78
DNS: services, structure 
why not centralize DNS?
single point of failure
traffic volume
distant centralized database
maintenance
DNS services
hostname to IP address translation
host aliasing
canonical, alias names
mail server aliasing
load distribution
replicated Web servers: many IP 
addresses correspond to one name
A: doesn’t scale!

Thinking about the DNS
humongous distributed database:
~ billion records, each simple
handles many trillionsof queries/day:
manymore reads than writes
performance matters:  almost every 
Internet transaction interacts with 
DNS ‐msecs count!
organizationally, physically decentralized:
millions of different organizations 
responsible for their records
“bulletproof”: reliability, security
Application Layer: 2-79

2-80
Root DNS Servers
com DNS servers
org DNS serversedu DNS servers
poly.edu
DNS servers
umass.edu
DNS servers
yahoo.com
DNS servers
amazon.com
DNS servers
pbs.org
DNS servers
DNS: a distributed, hierarchical database
client wants IP for www.amazon.com; 1
st
approx:
client queries root server to find com DNS server
client queries .com DNS server to get amazon.com DNS server
client queries amazon.com DNS server to get  IP address for 
www.amazon.com
...
...

2-81
DNS: root name servers
contacted by local name server that can not resolve name
root name server:
contacts authoritative name server if name mapping not known
gets mapping
returns mapping to local name server
13 root name “servers”worldwide
a. Verisign, Los Angeles CA
(5 other sites)
b. USC-ISI Marina del Rey, CA
l. ICANN Los Angeles, CA
(41 other sites)
e. NASA Mt View, CA
f. Internet Software C.
Palo Alto, CA (and 48 other   
sites)
i. Netnod, Stockholm (37 other sites)
k. RIPE London (17 other sites)
m. WIDE Tokyo
(5 other sites)
c. Cogent, Herndon, VA (5 other sites)
d. U Maryland College Park, MD
h. ARL Aberdeen, MD
j. Verisign, Dulles VA (69 other sites )
g. US DoD Columbus, 
OH (5 other sites)

82
IPv4根服务器分布表
名称 地位主服务器运营者
[1]
主服务器位置
[1]
IP
[1]
ADM and Root serverINTERNI.NET
美国弗吉尼亚州
198.41.0.4
BRoot server
美国信息科学研究所美国加利弗尼亚州
128.9.0.107
CRoot serverPSINet
公司美国弗吉尼亚州
192.33.4.12
DRoot server
马里兰大学美国马里兰州
128.8.10.90
ERoot server
美国航空航天管理局美国加利弗尼亚州
192.203.230.10
FRoot server
因特网软件联盟美国加利弗尼亚州
192.5.5.241
GRoot server
美国国防部网络信息中心   美国弗吉尼亚州
192.112.36.4
HRoot server
美国陆军研究所美国马里兰州
128.63.2.53
IRoot serverAutonomica
公司瑞典斯德哥尔摩
192.36.148.17
JRoot serverVerSign
公司美国弗吉尼亚州
192.58.128.30
KRoot serverRIPE NCC
英国伦敦
192.0.14.129
LRoot serverIANA
美国弗吉尼亚州
198.32.64.12
MRoot serverWIDE Project
日本东京
202.12.27.33
全世界只有13台IPv4根域名服务器。1个为主根服务器在美国。其余12个均为辅根
服务器，其中9台在美国，欧洲2个，位于英国和瑞典，亚洲1个位于日本。

83

2-84
TLD, authoritative servers
top-level domain (TLD) servers:
responsible for com, org, net, edu, aero, jobs, museums, 
and all top-level country domains, e.g.: uk, fr, ca, jp
Network Solutions maintains servers for .com TLD
Educause for .edu TLD
authoritative DNS servers:
Organization’s own DNS server(s), providing 
authoritative hostname to IP mappings for organization’s 
named hosts 
can be maintained by organization or service provider
amazon.com DNS server
.com DNS server

2-85
Local DNS name server
does not strictly belong to hierarchy
each ISP (residential ISP, company, university) has one
also called “default name server”
when host makes DNS query, query is sent to its local DNS 
server
has local cache of recent name-to-address translation pairs (but 
may be out of date!)
acts as proxy, forwards query into hierarchy

DNS name resolution: iterated query
Example: host at engineering.nyu.edu
wants IP address for gaia.cs.umass.edu
Iterated query:
contacted server replies 
with name of server to 
contact
“I don’t know this name, 
but ask this server”
requesting host at
engineering.nyu.edu
gaia.cs.umass.edu
root DNS server
local DNS server
dns.nyu.edu
1
2
3
4
5
6
authoritative DNS server
dns.cs.umass.edu
7
8
TLD DNS server
Application Layer: 2-86
2023.9.13

DNS name resolution: recursive query
requesting host at
engineering.nyu.edu
gaia.cs.umass.edu
root DNS server
local DNS server
dns.nyu.edu
1
2
3
4
5
6
authoritative DNS server
dns.cs.umass.edu
7
8
TLD DNS server
Recursive query:
puts burden of name 
resolution on 
contacted name 
server
heavy load at upper 
levels of hierarchy
?
Example: host at engineering.nyu.edu
wants IP address for gaia.cs.umass.edu
Application Layer: 2-87

2-88
DNS: caching, updating records
once (any) name server learns mapping, it cachesmapping
cache entries timeout (disappear) after some time (TTL)
TLD servers typically cached in local name servers
thus root name servers not often visited
cached entries may be out-of-date(best effort name-to-address 
translation!)
if name host changes IP address, may not be known Internet-wide until all 
TTLs expire
update/notify mechanisms proposed IETF standard
RFC 2136

2-89
DNS records
DNS:distributed db storing resource records (RR)
type=NS
nameis domain (e.g., foo.com)
valueis hostnameof authoritative 
Name Server for this domain
RR format:(name, value, type, ttl)
type=A
nameis hostname
valueis IP 
Address
type=CNAME
nameis alias name for some “canonical”(the real) 
name
e.g., www.ibm.comis really
servereast.backup2.ibm.com
valueis 
Canonical NAME
type=MX
valueis name of Mailserver
associated withname

2-90
DNS protocol, messages
queryand replymessages, both with same 
message format
msg header
identification:16 bit # for 
query, reply to query uses 
same #
flags:
query or reply
recursion desired 
recursion available
reply is authoritative
identificationflags
#questions
questions (variable # of questions)
#additional RRs
#authority RRs
#answer RRs
answers (variable # of RRs)
authority (variable # of RRs)
additional info (variable # of RRs)
2 bytes2 bytes

92

2-93
name, type fields
for a query
RRs in response
to query
records for
authoritative servers
additional “helpful”
info that may be used
identificationflags
# questions
questions (variable # of questions)
# additional RRs
# authority RRs
# answer RRs
answers (variable # of RRs)
authority (variable # of RRs)
additional info (variable # of RRs)
DNS protocol, messages
2 bytes2 bytes

2-94
Inserting records into DNS
example: new startup “Network Utopia”
register name networkuptopia.com at DNS registrar(e.g., 
Network Solutions)
provide names, IP addresses of authoritative name server 
(primary and secondary)
registrar inserts two RRs into .com TLD server:
(networkutopia.com, dns1.networkutopia.com, NS)
(dns1.networkutopia.com, 212.212.212.1, A)
create authoritative server type A record for 
www.networkuptopia.com; type MX record for 
networkutopia.com

Attacking DNS
DDoS attacks
Bombard rootservers with 
traffic
Not successful to date
Traffic Filtering
Local DNS servers cache IPs of 
TLD servers, allowing root server 
bypass
Bombard TLDservers
Potentially more dangerous
Redirect attacks
Man-in-middle
Intercept queries
DNS poisoning
Send bogus relies to DNS server, 
which caches
Exploit DNS for DDoS
Send queries with spoofed 
source address: target IP
Requires amplification
2-95

96
DNS诞生于1983年，在几次修订后最终于1987年定型。整个查询过程以TCP协
议或UDP协议在53端口上进行，并且以明文进行查询。整个过程完全没有验证
、没有隐私可言，这意味着任何人都可以对DNS查询过程进行攻击与篡改。

97

98
DoT(DNS over TLS)标准正式发布于2016年，而
DoH (DNS over HTTPS)标准正式成文于2018年10月

2-99
Chapter 2: outline
2.1 principles of network 
applications
app architectures
app requirements
2.2 Web and HTTP
2.3 FTP 
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 socket 
programming 
with UDP and 
TCP

2-100
Pure P2P architecture
noalways-on server
arbitrary end systems directly communicate
peers are intermittently connected and change 
IP addresses
examples:
file distribution (BitTorrent)
Streaming (KanKan)
VoIP (Skype) 

2-101
File distribution: client-server vs P2P
Question:how much time to distribute file (size F) from one 
server to N  peers?
peer upload/download capacity is limited resource
u
s
u
N
d
N
server
network (with abundant
bandwidth)
file, size F
u
s
:server upload capacity
u
i
:peer i upload 
capacity
d
i
:peer i download 
capacity
u
2
d
2
u
1
d
1
d
i
u
i

2-102
File distribution time: client-server
server transmission: must
sequentially send (upload) N file
copies
:
time to send one copy: F/u
s 
time to send N copies: NF/u
s
increases linearly in N
time to  distribute F 
to N clients using 
client-server approach
D
c-s
> max{NF/u
s,
,F/d
min
}
client: each client must download file copy
d
min
= min client download rate
min client download time: F/d
min
u
s
network
d
i
u
i
F

2-103
File distribution time: P2P
server transmission: must upload 
at least one copy
time to send one copy: F/u
s 
time to  distribute F 
to N clients using 
P2P approach
D
P2P
> max{F/u
s,
,F/d
min,
,NF/(u
s
+ u
i
)}
client: each client must download file copy
min client download time: F/d
min
clients: as aggregate must download NFbits
max upload rate (limiting max download rate) is u
s
+ u
i
... but so does this, as each peer brings service capacity
increases linearly in N...
u
s
network
d
i
u
i
F
File:
Minimum Distribution Time

1042-104
0
0.5
1
1.5
2
2.5
3
3.5
05101520253035
N
Minimum Distribution Time
P2P
Client-Server
Client-server vs. P2P: example
client upload rate =u,  F/u = 1 hour,  u
s
= 10u,  d
min
≥ u
s

2-105
P2P file distribution: BitTorrent
tracker:tracks peers 
participating in torrent
torrent:group of peers 
exchanging  chunks of a file
Alice arrives  ...
file divided into 256Kb chunks
peers in torrent send/receive file chunks
... obtains list
of peers from tracker
... and begins exchanging 
file chunks with peers in torrent

2-106
peer joining torrent: 
has no chunks, but will accumulate 
them over time from other peers
registers with tracker to get list of 
peers, connects to subset of peers 
(“neighbors”)
P2P file distribution: BitTorrent
while downloading, peer uploadschunks to other peers
peer may change peers with whom it exchanges chunks
churn:peers may come and go
once peer has entire file, it may (selfishly) leave or (altruistically) remain in 
torrent

2-107
BitTorrent: requesting, sending file chunks
requesting chunks:
at any given time, different peers 
have different subsets of file 
chunks
periodically, Alice asks each 
peer for list of chunks that they 
have
Alice requests missing chunks 
from peers, 
rarest first
sending chunks: tit-for-tat
Alice sends chunks to those four 
peers currently sending her chunks 
at highest rate
other peers are choked by Alice (do not 
receive chunks from her)
re-evaluate top 4every10 secs

every 30 secs: randomly select 
another peer, starts sending chunks
“optimistically unchoke”this peer
newly chosen peer may join top 4
让稀缺资源变多

2-108
BitTorrent: tit-for-tat
(1) Alice “optimistically unchokes”Bob
(2) Alice becomes one of Bob’s top-four providers; Bob reciprocates
(3) Bob becomes one of Alice’s top-four providers
higher upload rate:find better 
trading partners, get file faster !

Distributed Hash Table (DHT)
Hash table
DHT paradigm
Circular DHT and overlay networks
Peer churn 
109

Distributed Hash Table (DHT)
Background
How to distribute
the chunks to peers and 
manage the (peer, chunk)
110
Data Fragmentation

Original KeyValue
John Washington132-54-3570
Diana Louise Jones761-55-3791
Xiaoming Liu385-41-0902
Rakesh Gopal441-89-1956
Linda Cohen217-66-5609
................
Lisa Kobayashi177-23-0199
Simple database with(key, value) pairs: 
• key: human name; value: social security #
Simple Database
key: movie title; 
value: movie data.
111

Original KeyHash KeyValue
John Washington8962458132-54-3570
Diana Louise Jones7800356761-55-3791
Xiaoming Liu1567109385-41-0902
Rakesh Gopal2360012441-89-1956
Linda Cohen5430938217-66-5609
................
Lisa Kobayashi9290124177-23-0199
• More convenient to store and search on 
numerical representation of key
• key = hash(original key)
Hash Table
key: hash(movie title); 
value: movie data.
112

Distribute (key, value) pairs over millions of peers
pairs are evenly distributed over peers
Any peer can query database with a key
database returns value for the key
To resolve query, small number of messages exchanged among 
peers
Each peer only knows about a small number of other peers
Robust to peers coming and going (churn)
Distributed Hash Table (DHT)
113

Assign key-value pairs to peers
rule: assign (key, value) pair to the peer that has the 
closestID.
convention: closest is the immediate successor of the key.
e.g., ID space of peers is {0,1,2,3,...,63}
suppose 8 peers whose IDs are: 1,12,13,25,32,40,48,60
If key = 51, then assigned to peer 60
If key = 60, then assigned to peer 60
If key = 61, then assigned to peer 1
HASH the 
data’s name 
Peer ID
114
Data ID vs. peer ID  ?

1
12
13
25
32
40
48
60
Circular DHT
each peer onlyaware of 
immediate successor and 
predecessor.
“overlay network”
115

1
12
13
25
32
40
48
60
What is the value
associated with key 53 ?
value
O(N) messages
on avgerage to resolve
query, when there
are Npeers
Resolving a query
116

Circular DHT with shortcuts
•each peer keeps track of IP addresses of predecessor, successor, short 
cuts.
•reduced from 6 to 3 messages.
•possible to design shortcuts with O(log N) neighbors, O(log N) messages 
in query
1
12
13
25
32
40
48
60
What is the value for
key 53
value
117

Peer churn
example: peer 5 abruptly leaves
1
3
4
5
8
10
12
15
handling peer churn:
peers may come and go (churn)
each peer knows address of its two 
successors 
each peer periodically pings its 
two successors to check aliveness
if immediate successor leaves, choose next 
successor as new immediate successor
118

Peer churn
example: peer 5 abruptly leaves
peer 4 detects peer 5’s departure; makes 8 its immediate successor
4 asks 8 who its immediate successor is; makes 8’s immediate 
successor its second successor.
1
3
4
8
10
12
15
handling peer churn:
peers may come and go (churn)
each peer knows address of its two 
successors 
each peer periodically pings its 
two successors to check aliveness
if immediate successor leaves, choose 
next successor as new immediate 
successor
119

2-120
Chapter 2: outline
2.1 principles of 
network applications
app architectures
app requirements
2.2 Web and HTTP
2.3 FTP (cancel)
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 video streaming 
and content 
distribution 
networks (CDNs)
2.8 socket 
programming with 
UDP and TCP

2-121
Video Streaming and CDNs: context
•Netflix, YouTube: 37%, 16% of downstream residential ISP 
traffic
•~1B YouTube users, ~75M Netflix users
challenge:  scale - how to reach ~1B users?
•single mega-video server won’t work (why?)
challenge: heterogeneity
different users have different capabilities (e.g., wired versus 
mobile; bandwidth rich versus bandwidth poor)
solution: distributed, application-level infrastructure
video traffic: major consumer of Internet bandwidth

video: sequence of images displayed 
at constant rate
e.g., 24 images/sec
digital image: array of pixels
each pixel represented by bits
coding: use redundancy withinand 
betweenimages to decrease # bits 
used to encode image
spatial (within image)
temporal (from one image to next)
Multimedia: video
..........................
spatial coding example: instead 
of sendingN values of same 
color (all purple), send only two 
values: color  value (purple)  and 
number of repeated values (N)
..........................
framei
framei+1
temporal coding example: 
instead of sending 
complete frame at i+1, 
send only differences from 
frame i
2-122

Multimedia: video
CBR: (constant bit rate): video encoding rate fixed
VBR:  (variable bit rate): video encoding rate changes 
as amount of spatial, temporal coding changes 
examples:
•MPEG 1 (CD-ROM) 1.5 Mbps
•MPEG2 (DVD) 3-6 Mbps
•MPEG4 (often used in Internet, < 1 Mbps)
..........................
spatial coding example: instead 
of sendingN values of same 
color (all purple), send only two 
values: color  value (purple)  and 
number of repeated values (N)
..........................
framei
framei+1
temporal coding example: 
instead of sending 
complete frame at i+1, 
send only differences from 
frame i
2-123

Streaming stored video: 
simple scenario:
video server
(stored video)
client
Internet
2-124
Main challenges: 
server-to-client bandwidth will varyover time, with changing 
network congestion levels (in house, access network, 
network core, video server)
packet loss, delay due to congestion will delay playout, or 
result in poor video quality

Streaming stored video
1. video
recorded 
(e.g., 30 
frames/sec)
2. video
sent
streaming: at this time, client  playing out 
early part of video, while server still sending 
later part of video
time
3. video received, played out at client
(30 frames/sec)
network delay
(fixedin this 
example)
Application Layer: 2-125

Streaming stored video: challenges
continuous playout constraint: during client 
video playout, playout timing must match 
original timing 
•... but network delays are variable (jitter), so will 
need client‐side buffer to match continuous playout 
constraint
other challenges:
•client interactivity: pause, fast‐forward, rewind, 
jump through video
•video packets may be lost, retransmitted
Application Layer: 2-126

Streaming stored video: playout buffering
constant bit 
rate video
transmission
time
variable
network
delay
client video
reception
constant bit 
rate video
playout at client
client playout
delay
buffered
video
client‐side buffering and playout delay: compensate for 
network‐added delay, delay jitter
Application Layer: 2-127

buffered
video
Streaming multimedia: DASH
server:
divides video file into multiple chunks
each chunk encoded at multiple different rates
different rate encodings stored in different files
files replicated in various CDN nodes
manifest file: provides URLs for different chunks
client
?
client:
periodically estimates server‐to‐client bandwidth
consulting manifest, requests one chunk at a time 
•chooses maximum coding rate sustainable given current bandwidth
•can choose different coding rates at different points in time (depending 
on available bandwidth at time), and from different servers
...
...
...
Dynamic, Adaptive 
Streaming over HTTP
Application Layer: 2-128

...
...
...
Streaming multimedia: DASH
“intelligence” at client: client 
determines
•whento request chunk (so that buffer 
starvation, or overflow does not occur)
•what encoding rate to request (higher 
quality when more bandwidth 
available) 
•whereto request chunk (can request 
from URL server that is “close” to 
client or has high available 
bandwidth) 
Streaming video = encoding +DASH + playout buffering
client
?
Application Layer: 2-129

Content distribution networks
challenge: how to stream content (selected from millions of 
videos) to hundreds of thousands of simultaneoususers?
option 1: single, large “mega-server”
single point of failure
point of network congestion
long path to distant clients
multiple copies of video sent over outgoing link
....quite simply: this solution doesn’t scale
2-130

Content distribution networks
challenge: how to stream content (selected from millions of 
videos) to hundreds of thousands of simultaneous users?
option 2: store/serve multiple copies of videos at multiple 
geographically distributed sites (CDN)
enter deep: push CDN servers deep into many access networks 
close to users
used by Akamai, 1700 locations
bring home: smaller number (10’s) of larger clusters in POPs near 
(but not within) access networks
used by Limelight
2-131

Content Distribution Networks (CDNs)
subscriber requests content from CDN
CDN: stores copies of content at CDN nodes 
•e.g. Netflix stores copies of MadMen
where’s Madmen?
manifest file
•directed to nearby copy, retrieves content
•may choose different copy if network path 
congested
2-132
2023.9.18

Content Distribution Networks (CDNs)
Internet host-host communication as a service
OTT challenges: coping with a congested Internet
from which CDN node to retrieve content?
viewer behavior in presence of congestion?
what content to place in which CDN node?
“over the top”
more .. in chapter 7
133

CDN content access: a closer look
Bob (client) requests video http://netcinema.com/6Y7B23V
video stored in CDN at http://KingCDN.com/NetC6y&B23V
netcinema.com
KingCDN.com
1
1. Bob gets URL for video 
http://netcinema.com/6Y7B23V
from netcinema.com web page
2
2. resolve http://netcinema.com/6Y7B23V
via Bob’s local DNS
netcinema’s
authoratative DNS
3
3. netcinema’s DNS returns URL 
http://KingCDN.com/NetC6y&B
23V
4
4&5. Resolve 
http://KingCDN.com/NetC6y&B23
via KingCDN’s authoritative DNS, 
which returns IP address of KingCDN 
server  with video
5
6. request video from
KINGCDN server,
streamed via HTTP
KingCDN
authoritative DNS
Bob’s 
local DNS
server
2-134

Case study: Netflix
1
1. Bob manages      
Netflix account
Netflix registration,
accounting servers
Amazon cloud
CDN
server 
2
2. Bob browses
Netflix video
3
3. Manifest file
returned for 
requested video
4. DASH 
streaming
upload copies of 
multiple versions of 
video to CDN servers
CDN
server 
CDN
server 
2-135

2-136
Chapter 2: outline
2.1 principles of network 
applications
2.2 Web and HTTP
2.3 electronic mail
•SMTP, POP3, IMAP
2.4 DNS
2.5 P2P applications
2.6 video streaming and 
content distribution 
networks
2.7 socket programming 
with UDP and TCP (DIY)

Email: xieyi5@mail.sysu.edu.cn
https://cse.sysu.edu.cn/content/2462
Thanks
Q & A
