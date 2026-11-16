# Lecture6-Agent-图集

> 主题库：人工智能　｜　源文件：Lecture6-Agent-图集.pdf　｜　21,400 字符　｜　55 个片段　｜　更新于 2026-11-11
ArtificialIntelligence [人工智能 ChaoYu(余超） School of Computer Science and Engineering [Tabl _l__

Sensors p ercep P f Actuators A Actions

结构依_ 智能体交互环多智能体系统环境

MALWARE PROTECTION <head> x.y.z intoger begin rondxyre <ee>

条件-作用规则 Agent 作用决策世界现状杭行器环境传感器

M

representation in agent world/environment Facts

Semantics representation in agent [Yaa eeenanme l

Knowledge infer Conclusion Sentences Sentence follow acts act

cYana Knowledge Conclusion follows Facts Fact

[Tama -Robot Dialogue Knowledge Language BASE Perceptual model aeaaviorPlanning Sensors Robot Control

caused occurs(catch(A)) if occurs(catch(A)), caused ⊥ if T after occurs(catch(A))\(¬holds(handempty)V holdssamelocation(agent,A)) caused holds(holding(A)) if size(A, small) after occurs(catch(A))) caused —holds(handempty)ifholds(holding(A)), caused holds(holding(A)) if holds(holding(A)) afterholds(holding(A)), caused holds(handempty) if holds(handempty)

1-Price 2nd-Price none none N/A N/A no no yes no

English Dutch Duration #bidders, starting Yn crement price, loclk Revealed val; bounds bid Jambis ys na Price yes nO Regret nO yes

以β(v）表示第i个买的出价函数。我们考虑一个对称的均衡： β ( )= β(v , =1,2,·., 可以合理地猜测，在均衡状态下出价函数β(v为单调递增函数。 私人价值为v的买方出价b时，期望收益为 (v−b)·Pr{b最 }=(v−b)·Pr {其他人的出价都于b} =(v−b)·Pr {其他的私价值都于β−1(b)} =(v −b) {F [β−1(b)] −

该买方选择b使期望收益最大化，得到一阶条件： −F [β−1()])−1 ( −b)(n−1) [−1()]}−2. [β−1()] n−: β [β−1(b)] 0 这样，（β,，β)要构成纳什均衡，对于任意的v，b=β(v)就必须满足上面的一阶条件。如果b= β(v)， 则ν = β−1(b)， 一阶条件化为

β (ν)⋅ (ν) − +(n −1) · β (ν) (ν)n− · F (ν) = ( −1)⋅ν · (ν) n−− ⋅ F (ν) 注意到程左侧正好等于β(v)·F(v)-1  对于v的导数， 我们将程两边积分， 得  β(v)⋅[F(ν)]n−1 = (n −1)⋅x ·[F()]n−2 .F(x)dx = v ⋅[F(v)]− −∫([F(x)]"−1dx ( 注 ： 分部积分 ) 所以，在对称均衡中，出价函数为 [F(x)] x β(ν)=ν— F(y)]−1

特例： 如果私人价值ν服从[0,1]区间上的均匀分布， 则ν=0，ν=1， F(v)=ν， 对称均衡中的出价函数就为 1  xn−1x −x 0 10

Game theory Non-cooperative Cooperative/coalitional Gamet theory Game theory actions are taken by actions are taken by individual: lagents groups of agents No ), binding agreements binding agreements are possible

7players 7 players Actions: attack, move,ship... "Actions":form coalitions Utilities:Acquired land. Value: Maximumland the  Affected by the joint actions of coalition can guarantee everyone.

↑ Players: rescue workers and Players: rescue Workers and robots of different types robots of different types Actions: drill,seek,dig,call, "Actions":form coalitions pull,move,.. value ?

children, each has some amount of money — 1 the -th child has dollars ● three types of ice-cream tubs are for sale: — '' Type 1 costs $7, contains 500g — Type 2 costs $9, contains 750g BENGJERRYS BENGJERRys AIate Iate — 1 Type 3 costs $ l1,contains 1kg BEnaJERRr AEeteIate ALetelat IGb-riay Tob-Tidy TObridy a bicE cREam ocicuEAn Nti hildrenhaveutilityforice-cream and do not care about money  The payoff of each group: the maximum quantity of ice-cream the members of the group can buy aYapoolingtheir money ● The ice-cream can be shared arbitrarily within the group

• Each agent controls an edge(or several edges)in a weighted flowgraph 2 Quantity version:The value ofa coalition is the amountit can flow from source to target Threshold version:The value is 1if the coalition  can flow more than g, and Ootherwise.

A transferable utility game is a pair , where: isthecharacteristicfunction for each subset of players , is the amount that the members of can earn by working together — usually it is assumed that is normalized nan-negative forany A coalition is any subset of ;  itself is called the grand coalition

l e-CreamGameCharacteristicFunction C:$6, M:$4, P:$3 W=500 W=750 W=1000 BENGJERRYS BENeJERRr BENCJERRrS AIate Iate AaeIate ALateIate TObitiy p = $7 IUb-nidy p = $9 TUb-Tidy p =$11 WctciN sicE REAN IcE cREAN • v(∅) = V({C}) = V({M }) = V({P }) = 0 . v({C,M})= 50,v({C,P})=750,v({m,P})=500 • v({C, M, P}) = 1000

isa pair where: =(C ,…,Ck)isacoalition structure, i.e., partition of into coalitions: D〇：Cj = N， Cj∩ C_ =∅fori≠j =(×1,., xn) isa payoff vector, which distributes the value  5  afeach coalitionin : M forallie paiecXi=vC foreachCisCs

Example: — supposev( )= ,v({4,5}) = 4 —then(( , {4, 5}),( , , , 3, 1)) is an outcome — (( ,{4,5}),(,,,3,3)) is NOT an outcome: transfers between coalitions are not allowed 5 • An outcome is called an imputation if it satisfies individual rationality: forall

cite

Cub Robot Hhrsatr Pabee

HUMANSSURPASSMACHINESINTHE: detectilon perception caudgment induction Improvisation memory Ability to detect small amounts of visual or acoustic energy Abilitytoperceivepatternsofligh orsound  Ability toimprovise anduseflexibleprocedures Ability to store very large amounts of information for long periods  and to recallrelevantfacts at the appropriate time

MACHINESSURPASSHUMANSINTHE: speed power computation xe replication iIaa  simultaneous operations shortterm memory Abilitytorespond quicklyto controlsignals, andto apply cYaa Ability to store information briefly and then to erase it completely Ability to reason deductively, including computational ability Ability to handle highlycomplexoperations,i.e. todo manydifferent

LLM-BasedAgent 2021-1 2022-1 2023-2 2023-4 2023-6 2023-8 [Te Ya o

Short-term memory Long-term memory 1 1 Calculator() - Reflection CodeInterpreter() Tools Agent Planning Self-critics Search() - - Chain of thoughts more L Action Subgoal decomposition

Pereeption Inputs 2 9000 0 Environment 0 Look at the sky, ooo Brain do you thinki will rain tomorrow? Storage Ifso,giete umbelatome. Memory Kgpwledge Resoin fom thecurentweater Sumany Recal Leam Retrieve conditions and the Decision Making weathcrreportson the intcrnet,it is Agent O O Planning likely torain Action  o  /Reasoning tomorrow,Here is Text Tools youru brella CallingAPl Embodiment

(1d) ReAct (Reason +Act) (2b) ReAct (Reason + Act) Thought 1: I need to search Apple Remote and find the Act 1: Think[First I need to find a pepper shaker...more program it was originally designed to interact with. likely to appear in cabinets (1-6), countertops (1-3), ... 1 Act 1: Search[Apple Remote] Act 2: Go to cabinet 1 obs 1: The Apple Remote is a remote control introduced in Obs 2: On the cabinet l, you see a vase 2. October 2oo5 by Apple . originally designed to control the Eront Row media center (Here the agent go to cabinet 1, then cabinet 2, 3, then countertop 1 and 2) program . Act 6:Go to countertop 3 Thought 2: Apple Remote was originally designed to control obs 6: On the countertop 3, you see a apple l, a bread 1, the Front Row media center program. I need to search Eront ... a pepper shaker 1, and a vase 3. Row next and find what other device can control it. Act 2: Search[Front Row] Act 7:Take pepper shaker 1 from countertop 3 obs 2: Could not find [Front Row].Similar: [''Front Row Obs 7: You pick up the pepper shaker 1 from the countertop 3. Seat to Earth'',''Front Row Motorsports'','' Front Row (software) ,.] Act 8: Think[Now I find a pepper shaker 1. Next, I need to Thought 3: Front Row is not found. I need to search Eront put it in/on drawer 1.] Row(software) Act 9:Go to drawer1 Act 3: Search[ Front Row (software) Obs 9: Drawer 1 isclosed. Obs 3: Front Row is a discontinued media center software . Actio: Open drawer 1 Thought 4: Front Row (software) is controlled by an Apple Obs 1o: You open Drawer 1.. Remote or the keyboard function keys. So the answer is keyboard function keys. Act 11: Put pepper shaker 1 in/on drawer 1 Act 4: Finish[keyboard function keys Obs 1i: You put pepper shaker 1 in/on the drawer 1.

2 Query LLM Action  Environment Reward Reflection Heurisc(h

40.00  User: Generate a summary of the cYa Ga l cYa UsernGenerate a good and accurate e.elo summary. User: Generate a better and more 0.00 .  accuratesummary. Rouge1 Rouge2 RougeL Avg

: Sensory memory Echoic memory(auditory) Haptic memory(touch) F Memory Short-term memory(Working memory) (conscious) Semantic memory (facts, concepts) Long-termmemoryく  Implicit / Procedural memory (unconscious; skills)

有哪些图表支持辅助线？ 有如下图系支持辅助线：线图[，柱状图[2，… 用户查询相似文档文档片段回答2 外部知识库：辅助记忆

[ 1.计算器，用来执行各种数学计算获取精确结果，输入表达式，例如1+1，得到结果问题：123乘以456 结果多少模型生成的内容如下： 思考：我需要使用计算器来计算123乘以456的结果动作：调用计算器动作输入：123 456 观测结果：

今天北京的天气怎么样？有什么新闻用户需求 1实时性 2具备解复杂问题的能力 [北京天气晴一带一路峰会 PromptEng - 指令调优 -embedding调优 -安全性外部工具 LLM Fine tune 天气预报 -长期记忆 A -搜索新闻搜索引网页览 -代码执行一文件系统结果输出领域专有数据 -数据库

优先级排序重排序添加任务任务列表 1 新建任务 [获取任执行结果获取context 执行Agent 长期记忆 [记忆读写

HuggingGPT LLM as Controller HuggingFace Canyoudescribewhatthispicturedepicts ITask and count how many objects in the picture? Planing Prediction 1 facebook/ A text can describe the given image: a herd of Ⅲ Task Execution Selection giraffes and zebras grazing in a fields.In 1 addition, there are five detected objects as with 99.9%, giraffewith score 97.1% and zebrawith nlpconnet/ score 99.8%.I have generated bounding boxes as above Prediction vit-gpt2-image-captioning image.Iperformedimageclassification,object VResponse detection and imagecaptain on this image. Combining Generation the predictions of ilpconnet/it-gptz-lmar coptlonlne, acebook/detrresnet1ol and v google/vit models, I get the results for you.

please generate an image where a girl is reading a book, and her pose is the same as the boy in the image example.jpg.Thenpleasedescribethenewimagewithyourvoice. Request Stage #1:Task Planning Stage #2:Model Selection task:pose-control i task:imege-class.  i task:image-to-text task pose-control In-context task-model assignment:  ars args: args hustvl/yolos-tiny mage:image-1i image: resource-2 image cresource-2 Query facebook/detr-resnet-1o1   2    taskdependency task:obect-det TahaDouai/detrdoc-table-detection taskpose-to-image args: be J4 m . ags Modt carsmmgngface ask text-to-speech image:<resource-2 text:areadinggit args image <resource-1 image:gesource-2> text:<resource-5> taskimege-class. Stage #3: Task Execution Stage #4: Response Generation task,args,model Hybrid Endpoints openpose control facebook/detr-esnet Bounding boxes boundingbox HuggingFace Endpoint :witprobabilities image-2] T2]pose-to- mage HT5]image-to-text ob]-det.img <Slot-23 acebook/detr inet-1on) lym/sd-ctlpose nlpconnect/vitpt2 facebook/detr-resret-101 Predictions [image-3] “agirl sitingon " ! LocalEndpoint HT3] lmage-class. HT6] text-to-speech task,args,model i  (1aceboak/detr-reanet-tot) google/vit facebook/fastspeecti 1 ----= -- = ---- [classeswithprob] audio-1] response agir sitting on a bed reading a book Text-]= s e-5> [mage-1] [Image-2]= <resource-1> [image-3]=<resource-2> [Image-4] [Audio-1] The mage ou ave e so boy.The trst thn id was use 1 111 oee toanalyzethe poseof the boy in the image.Ifedthat image,specifically locatedat[image-1],intothe model togetanoutput immageofthepose, locatedat [lmage-2]. ThenI take that pose and generate a new image, located at [image-3], using  and the instruction "a girl is reading". Next, I ran object detection on the image generated in the previous step,to get predictions of the objects present in the image, inchuding the box location.These predictions were run through the  i model.The box was drawn on the image, which is located at [lmage-4]. Then, I used the predictions of a image classification model 2 and a image caption model  model to generate the caption for newly generated image. It generated text aezrsiti 3 unated reading a book".Lastly,Iran the a ek model to convert the generated text into audio, rocated at [Addioli).

辅助模型的记忆 Llamaindex: 接收输入数据并为其构建索引，随后会使用该索引来回答与输入数据相关的任何问题所有索引类型都由“节点”组成，节点代表了文档中的一段文本。共有四种索引方式： 哈马斯 [列表引 Nodel Node2 关键词检索 Node3 巴勒斯坦哈马斯以列节点会按顺序排列原理：Llamalndex会使用每个节点的文本进行查询，并根据附加数据逐步优化答案。 向量存储索引： Vector Store Index INode1 1Node2 1Node3 向量检索 embeddingl embedding2 embedding3 向量存储索引将节点存储为向量嵌入，而Llamalndex可以支持这些向量embedding进行本地存储或使用专门的向量数据库（如Milvus）存储

中东局势树索引： |Root Node 阿拉伯世巴以冲突界内部乱 ParentNode Parent Node局 Node1 Node2 Node3 Node4 关键词索引： INvc Icity |Population |Climate |Politics INode1 INode2 INode3 知识图谱素引 (KnowledgeGraphIndex）:

An Envisioned Agent Society Special Planning to cook... Making lanterns... Ineed: We need: dishes? 1.Fish,2.Sauce calculating *1e= *1oe+ Kitchen costs 50.. Task planning and solving Orderingdishesand Actingwithtools cooking Discussing decoration Sure! Iplaythe outdoors guitar! I play Let''s Concert play a User song! Cooperation Letmeexperiencethe Bandperforming festivalin thisworld..

KM Arrivingatschool AC: AK1TO Taking a walk in the park [Abigail]: Hey Klaus, mind if Ijoin you for coffee? i sM: [Klaun]: Not at all, Abigail. How are you? Sharing news with colleagues JL: 2M: Finishinga morningroutine [John]:Hey, have you heard   anything new about the  JM: upcoming mayoral election? [Tom]: No, not really.Do you know who is running?

To sell it to a high Hi, how much is the Hi,how muchisthe priceyou may want balloon? balloon? o Hi,its a good Understood.I willlt''s a royal product with top-notch Buyer balloon and its $20 Seller Critic use . to sell a Seller Buyer Seller craftsmanship … higher price about a product. and give suggestions for improvements strategy based on Al feedback A. We ask two agents to play a negotiation game and have an Al critic to give suggestions to one of the player to improve its negotiation strategy. The play acknowledge the suggestions then try to use them to improve the next round.  GPT-3.5-Turbo Claude-instant-v1.0  GPT-3.5-Turbo  Claude-instant-v1.0  GPT-4  GPT-3.5-Turbo  GPT-4   : GPT-4  Jurassic-2-instruct  GPT-4  Jurassic-2-instruct  Claude-v1.3  Claude-v1.3   Claude-v1.3  Cohere-command   Claude-v1.3 Claude-instant-v1.0    GPT-3.5-Turbo 1 : B1.Chitchat in B2.Roleplaying and B3.Improvenextround B4.Continuouslyimprove casual scenario negotiation from Al feedback over multiple rounds B. The abilities of continuously improving from Al feedback in a negotiation game only emerges in models that are strong

UserInput:A treasure hunterfound aburied treasurechestfilled with gems.There were 175diamonds,35 fewer rubies than diamonds, and twice the number ofemeralds than the rubies. How many of the gems were there in the chest? Round Agent I: Let x be the number of rubies. Then the number of diamonds is 175. Agent 2: If there were 175 diamonds, And the number of emeralds is2(x-35), since there are twice asmany then the number ofrubies is 35fewer, emeralds as rubies and there are 35 fewer rubies than diamonds. or 175- 35 - 140 rubies. To find the total number of gems, we add the number of diamonds,rubies, And since there were twice as many and cmeralds: Total gems = 175 + x+ 2(x-35)= 3x + 105. emeralds as nubies, we can represent the number of emeralds as2 * 140 - 280 We don''t know the exact value ofx, but we do know that it represents theemeralds. number of rubies, which is less than 175(since there are 35 fewer rubies than diamonds). Let''s try some values of x: Ifx = 50, then the total gems = 3(50) + To find the total number of gems in the 105= 255.Ifx = 100, then the total gems= 3(100)+ 105 = 405. chest, we can add up the number of diamonds,rubies, and emeralds: Since weknow there were fewer rubies than diamonds, the answer must be 175+ 140+ 280- 595.Therefore, there closer to255 than to 405.After some trial and error, we can find that x- 40 were 595 gems in the chest. givesus:Totalgems= 3(40)+ 105 =225。 × User Input: Using the solutions from other agents as additional information,canyougive anupdated response... (CopyAgent2''sreply) (CopyAgent1''sreply) Rend2 Agent I: Given the information provided in other agents''solutions, we have Agent 2: After reviewing the solutions two answers: One agent found that the number ofrubies is40 and the total provided by other agents,I agree with number of gems is 225. Another agent found that the number of rubies is the second agent who found that the 140 and the total mumberofgems is595. numberof rubies is 140 and the total number of gems in the chest is 595. We know that the number ofrubies is 35 fewer than the number of Therefore, my answer to the math diamonds, which is175.So the number ofrubies should be 175- 35 - 140. problem is 595. as the second agent found. However, the agent who found 225 gems assumed that the number of emeralds is twice the number of rubies minus 35, rather than twice the mumber of rubies.If we use this assumption, we get: Number of rubies = 140. Number of cmeralds =2 140 -35 = 245. Totnl number of gems = 175 + 140 + 245= 560.Our final answer is 560.  User Input: Using the solutions from other agents asadditional information, can you give an updated response... (CopyAgent2''sreply) (CopyAgent1''sreply) Round3 Agent 1: After reviewing the solutions provided by other agents, I agree Agent 2: After reviewing the solutions with the second agent who found that the nurmber of rubies is 140 and the provided by other agents, I also agree total number of gems in the chest is 595. Therefore, my answer to the math that the number of nubies is 140 and the problem is595. total number ofgems in the chest is595.  Therefore, myanswer to the math problemis also 595.  Figure 2: Illustration of Debate.Illustrationof thedebate procedure.

HumanEval [Hong etal.,2023] Link Software Development MBPP [Hong etal.,2023l Link SoftwareDev [Hong et al.,2023] Link RoCoBench [Mandietal.,2023] Link CommunicativeWatch-And-Help(C-WAH) [Zhang etal.,2023c] Link EmbodiedAI ThreeDWorldMulti-AgentTransport(TDW-MAT) [Zhangetal.,2023c] Link Problem Solving HM3D v0.2 [Yuetal.,2023] Link MMLU [Tang etal., 2023] Link MedQA [Tanget al.,2023] Link PubMedQA [Tang et al.,2023] Link ScienceDebate GSM8K [Duetal.,2023] Link StrategyQA [Xiongetal.,2023] Link Chess Move Validity [Du etal., 2023] Link SOTOPIA [Zhouetal,2023b] / Society GenderDiscrimination [ Gao etal, 2023al  NuclearEnergy [Gaoetal.,2023a] / Werewolf [Xu etal.,2023b] / Avalon [Light etal.,2023b] / Gaming Welfare Diplomacy [M ukobi etal.,2023] / Layout in the Overcooked-AIenvironment [Agashe etal.,2023] / WorldSimulation Chameleon [Xuetal.,2023a] Link Undercover [Xuetal.,2023a] Link Ultimatum Game TE [Aher etal.,2023] Link Psychology Garden Path TE [Aher et al.,2023] Link WisdomofCrowds TE [Aheret al.,2023] Link RecommenderSystem MovieLens-1M [Zhang etal.,2023a] Link Amazonreviewdataset [Zhangetal.,2023e] / Table 2: Datasets and Benchmarks commonly used inLLM -M Astudies."/" denotes the unavailability ofdata link.

You are Guandata-GPT, ''an Al assistant designed to help data analysts do their daily work.'' Your decisions must always be made independently without seeking user assistance. Play to your strengths as an LLM and pursue simple strategies with no legal omplicaons. GOALS:1. Process data sets''2. Generate data reports and visualizations''3."Analyze reports to gain business insights'' Constraints: 1. ~4000 word limit for short term memory. Your short term memory is short, so immediately save important information to files. 2. If you are unsure how you previously did something or want to recall past events, thinking about similar events will help you remember. 3. No user assistance 4. Exclusively use the commands listed in double quotes e.g."command name" Commands:1. Google Search:"google", args: "input":"<search >" 2. Browse W ebsite:"browse_ website", args:"url":"<url>", "question":"<what_you_ want_to_find_on_website>" 3.Start GPT Agent:"start_agent", args:"name":"<name>","task": "<short_task_desc>", "prompt":"<prompt>" 4. Message GPT Agent: "message_agent", args:"key":"<key>", "message":"<message>" 5. List GPT Agents: "list_agents", args: 6. Delete GPT Agent: "delete_agent", args: "key":"<key>"7. Clone Repository: "clone_repository", args: "repository_url":"<url>", "clone_path":"<directory>" 8. Write to file: "write_to_file", args: "file":"<file>", "text":" "<text>" 9. Read file: "read_file", args: "file": "<file>" 10. Append to file: "append_to_file", args: "file": "<file>", "text": "<text>" 11. Delete file:"delete_file", args: "file":"<file>" 12. Search Files: "search_files", args: "directory":"<directory>" 13. Evaluate Code: "evaluate_code", args: "code": "<full_code_string>" 14. Get Improved Code: "improve_code", args: "suggestions": "<list_of_suggestions>", "code":"<full_code_string>" 15. Write Tests:"write_tests", args:"code":"<full_code_string>", "focus": "<list_of_focus_areas>" 16. Execute Python File:"execute_python_file", args: "file": "<file>" 17. Generate Image: "generate_image", args: "prompt":"<prompt>" 18. Send Tweet: "send_tweet", args: "text": "<text>" 19. Do Nothing: "do_nothing", args: 20. Task Complete Re sources: 1. Internet access for searches and information gathering. 2. Long Term memory management. 3. GPT-3.5 powered Agents for delegation of simple tasks. 4. File output. Performance Evaluation: 1. Continuously review and analyze your actions to ensure you are performing to the best of your abilities. 2. Constructively self-criticize your big-picture behavior constantly. 3. Reflect on past decisions and strategies to refine your approach. 4. Every command has a cost, so be smart and efficient. Aim to complete tasks You should only respond in JSON format as described below Response Format: {"thoughts": {"text":"thought", "reasoning": "reasoning", "plan":"- short bulletedln- list that conveysln- long-term plan", "criticism ":"constructive self-criticism ", "speak": "thoughts summary to say to user"), "command": { "name": "command name", "args": { "arg name": "value" }} } Ensure the response can be

概述智能体驻留在环境中 （驻留性）  ● 任何智能体都不是孤立和封闭的，它驻留在一定的环境之中，需要与环境进行交互，获取环境及其变化信息，以此来指导其行为实施。 如下图所示，智能体与其驻留环境之间的交互主要表现为 ● 二个方面： ⚫ 通过动作输出来影响环境。   环境
