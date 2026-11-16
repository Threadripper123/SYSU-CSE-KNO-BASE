# DS-CH-12-分布式Web-图集

> 主题库：分布式系统　｜　源文件：DS-CH-12-分布式Web-图集.pdf　｜　4,574 字符　｜　22 个片段　｜　更新于 2026-11-11
天河天天二号天号

80

中算机院院 S SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

Help

Client machine Server machine document from local fi le [Taa 1. Get document request (HTTP)

3. Start process to fetch document 1. Get request HTTP CGI 4. Database interaction GaReturnresul tande [aaHaMldocumen ccnYeated Web server CGlprocess Database server

CYlient machine Server machine LOOkUP a service Client Server Publish service capplication application Stub Stub Communication SOAP Communication subsystem subsystem cGenerate stub Generate stub description description Service description (WSDL)

cYa mnmnam l CYlient-side Network script HTML/XML comm. interpreter parser

Module Module Module [Functaon Link between function and hook Hook Hook Hook Hook aache cere Functions called per hook Request Response

6. Server responses WEB 5.Forward server 3.Hand of other 2 TCP connection messages Distributor Other messages Dis- Client Switch 4.Inform patcher Setup request switch 1. Pass setup request Distributor 2. Dispatcher selects WEB SeRver

Yperation Head Request to return the header of a document Get Request to return a document to the client Put Request to storea document Post Provide data that are to be added to a docu- Yment(collection Delete Request to delete a document

C/S Header Contents Accept C The type of documents the client can handle Accept-Charset C The character sets are acceptable for the client Accept- C The document encodings the client can handle Accept- C The natural language the client can handle Language Authorization C A list of the client''s credentials WwW- s Security challenge the client should respond to authenticate Date C+S Date and time the message was sent ETag S The tags associated with the returned document Expires S The time for how long the response remains valid From C The client''s e-mail address Host C The TCP address of the document''s server If-Match C The tags the document should have If-None-Match C The tags the document should not have If-Modified- C Tells the server to return a document only if it has Since been modified since the specified time If-Unmodified- C Tells the server to return a document only if it has Since not been modified since the specified time Last-Modified S The time the returned document was last modified Location S A document reference to which the client should redirect its request Referer C Refers to client''s most recently requested document Upgrade C+s  The application protocol sender wants to switch to Warning C+S Information about status of the data in the message

env:Envelope xmlns:env="http://www.w3.org/2003/05/soap-envelope"> <env:Header> <n:alertcontrol xmlns:n="http://example.org/alertcontrol"> <n:expires>2001-06-22T14:00:00-05:00</n:expires> </env:Header> eenv:Bodyo <m:alert xmlns:m="http://example.org/alert"> <m:msg>Pick up Mary at school at 2pm</m:msg> </m:alert> </env:Envelope>

Scheme Host name Pathname http :// www.cs.vu.nl  /home/steen/mbox (a) Scheme Host name Port Pathname http :// www.cs.vu.nl : 80 /home/steen/mbox Scheme Host name Port Pathname http :// 130.37.24.11 : 80 /home/steen/mbox

http HTTP http://www.cs.Vu.nl:80/globe mailto Mail mailto:steen @ cs.vu.nl ftp FTP ftp://ftp.cS.Vu.nl/pub/minix/README file Local file file:/edu/book/work/chp/11/11 data Inline data data:text/plain;charset=iso-8859-7, %e1%e2%e3 telnet Remote login telnet://flits.cs.vu.nl

Cooperative caching, by which you first check your  ( neighborsonacachemiss

Web server to Web server 1.Look  local cache Web 2. Ask neighboring proxy caches Web  Cache proxy  proxy Cache [Client Client Client Client Client Client Web TTP Getrequest proxy Cache Clien Clien Clien

Uncontrollable parameters (disturbance / noise) Initial configuration Corrections Observed output +1- +1- Replica Consistency Request Metric placement enforcem ent routing estimation Analysis

2 days · 2 days (a) (b) 6 days· - 2.5 days (c) (d)

canGetembeddeddocument CDN (if not already cached) Cache server 5.Get embedded Yacamen Retum IP address 7. Embedded documents cYa a.Get base document CDN DNS 4 Client Origin server server 2. Docum ent with refs DNS lookups 3 to embedded documents

Edge-server side Origin-server side cache copy full/partial data replication Content-aware full schema replication/ Authoritative Cache Schema query templates Schema database

mege-server side rigen-serve side CYient Server query Server response Content-blind Database cache copy full/partial data replication Content-aware full schema replication/ Authoritative cache Schema query templates Schema database
