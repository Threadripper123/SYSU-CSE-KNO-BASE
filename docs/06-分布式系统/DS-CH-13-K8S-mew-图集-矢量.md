# DS-CH-13-K8S-mew-图集-矢量

> 主题库：分布式系统　｜　源文件：DS-CH-13-K8S-mew-图集-矢量.pdf　｜　4,628 字符　｜　53 个片段　｜　更新于 2026-11-13
app app libs libs  app libs libs kernel

% Cat Dockerfile FROM node:4.4 EXPOSE8080 COPY server.js. CMD node server.js % dockerbuild-t gcr.io/mohr-dev/hello-node:vl. % docker run -d -p 8080:8080 --name hello_tutorial gcr.io/mohr-dev/hello-node:v1 % curlhttp://localhost:8080/ Hello World!

Container API Cluster

API  CL kubelet ca scheduler  cnannrollers kubelet

—

172.16.1.1   172.16.1.1  172.16.1.2

A: 172.16.1.1 9376 SNAT 8000 80  SNAT 11878 B: 172.16.1.2

cYantent Manager Consumers    🌸 File  や Web     Puller Server [Yalume Pod Google Cloud Platform

Support many / types of volume plugins ● EmptyC dir(and tmpfs) C · vSphere  Hostpath · GlusterFS  Git repository • Ceph File and RBD . GCEPersistentDisk . Cinder AWS  Elastic Block Store · FibreChannel . AzureFile Storage . Secret,ConfigMap,   isCSl DownwardAPl  Flocker Flex (exec a binary)

arbitrarymetadata o Attached to any APl object  Generally represent identity Queryable by selectors 0  think SQLselect .. where.. 1  ä α The only grouping mechanism pods under a ReplicaSet caadsinaService caapabilitiesofanode (constraints

App: MyApp Phase: prod Role:FE

App: MyApp Phase: test Role:FE

App: MyApp Phase: prod Role: BE

App: MyApp Phase: test Role: BE

App: MyApp  O   App: MyApp Phase: test 0 ©     O O  0 Phase: test   Role: FE    Role: BE App= MyApp,Phase = test   1

How Start1 How many? more many? 3 K APl Server

A group of pods that work together ( grouped by a selector Defines access policy  "load balanced”or"headless  VirtualIP Can have a stable virtual lP and port .  alsoaDNSname  VIP is managed by kube-proxy updates iptables when backends change - O oo    O default implementation - can be replaced! Hides complexity

NodeX kube-proxy iptables

services& NodeX kube-proxy watch apiserver iptables

NodeX kube-proxy  apiserver iptables

kubectl run …. iserver

kuberproxy apiserver schedule  iptables ① a9 ö 0 a a0 ö

cYa

kubectl expose apiserver  -

cYa

NodeX kube-proxy  apiserver iptables

ReplicaSet name: : my-app-vl replicas:3 2 + selector: Yersion: l

name:  my-app-v1 replicas: 0 + Yersion: l

Updates-as-a-service | Rolling update is im perative, client-side  Deployment manages replica changes for you updates are configurable, done server-side kubectl edit or kubectl  . i apply  Aggregates stats Can have multiple updates in flight Status:BETA in Kubernetes v1.2

Run-to-completion, as opposed to run-forever • Express parallelism vs. required completions Workflow: restart on failure E Bu ild/test: don''trestartonfailure Aggregates success/failure counts Built for batch and big-data work Status: GA in Kubernetes v1.2

Claim

Cluster Admin

PersistentVolumes Provision Cluster Admen

User

PVClaim areate User Google Cloud Platform

PersistentVolumes

Binder

Cluster 4 Admin 回 PVClaim Create 0 Pod User Google Cloud Platform

PersistentVolumes 回 Gaogle Cloud Platform

Peaersistenteolum es Cluster Admin PVCIaim Delete User GeogleClud Platfom

Recycler

Goal: enable clustered software on Kubernetes mysqlredis,zookeeper,. Clustered ε apps need "identity” and sequencing guarantees stable hostname.availablein DNS ● stable storage: linked to the ordinal & hostnam e cdiscoveryofpeersforquorum anartup/teardownerdering Status: ALPHA in Kubernetes v1.3

API Pod Config Map [Ynade orm

c

Defined in terms of Request and Limit Guaranteed:highestprotection  request > 0 && limit == request    O Burstable: medium protection O  request > 0 && limit > request oo Best Effort: lowest protection O request==e What does"protection"mean? Oomscore CPU scheduling

addnodeswhenneeded  . 1 t therearepending pods somepending pods would fit ifweaddanode Remove nodes when not needed • after removal, all pods must fit remaining nodes    Status: Works on GCE, GKE and AWS

SLO metat <2000 nodeS,<60000 pods Pod startuplatency 3 50th • 99% ofpodsstartin < 5 seconds percentile  2,5 percentile Coming Soon ggth · protobufs in APl storage (already enabled on percentile 2 cYaawRe 5000 nodes 1,5

~5k ( Commits Top 0.01% of 250  W 800 Unique in 1.4 over 3 all Github Pro COntributOrS months Projects ciTRiX WeaVeWorks HUAWEI CoreS intell Companies Contributing Fujinsu redhat. V m W are H iT A C HI Engine Yard

Companies TTPAN Using ① Pearson El

Goldman Sachs SAMsuNG SA italOcean WiKIMEDiA FOUNDATION

R REDAPT 重克

ONCUR eby D Digi SOUNDCLOUD

1X Target Traffic 5X Worst Case estimate 50X Actual Traffic Original LaunchTarget EstimatedWorstCase Actual Traffic Google Cloud "Niantic chose GKE for its ability to orchestrate their container cluster at planetary-scale, freeing its team to focus on deploying livechangesfortheirplayers. Niantic
