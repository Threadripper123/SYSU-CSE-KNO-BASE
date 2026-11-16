# Lecture7-机器学习1-概述

> 主题库：人工智能　｜　源文件：Lecture7-机器学习1-概述.pdf　｜　10,200 字符　｜　96 个片段　｜　更新于 2026-11-11


人工智能
机器学习概述

W h a t  is M a c h i n eL e a r n i n g?

State of the Art in
Machine Learning Applications

Applications: Spam Detection
•Thisisabinaryclassificationtask:
Assignlabel(i.e.spam/not-spam)
totheinput(anemailmessage)
•Classification requires a model 
(aclassifier)todeterminewhich  
labeltoassigntoinput.
Email
Classifier
inputmodellabel

Applications: Document Classification
•Thisisamulti-classclassificationtask:
oAssignlabel(i.e.Politics,Sports,Finance,Arts)totheinput

Applications: Document Classification
•Thisideageneralizestomanytypesofdataandapplications
Labels
Politics,Sports, Finance
Positive,Negative  
Person,Location
Cat,Dog,Snake,Horse  
Re-admitsoon/Not
Data 
Documents  
Sentences  
Phrases  
Images
Medicalrecords
. ..

Applications: Object Recognition

Autonomous Cars
•Nevada was the first to legalize  
autonomous cars to drive on 
roads  (in June2011)
•As of 2019, 37 states have enacted  
legislation regarding autonomouscars
Penn’s Autonomous Car  (Ben Franklin RacingTeam)

Autonomous Cars Sensor

Autonomous Cars Technology
Laser TerrainMapping
Human
Machine 
Learning
AdaptiveVision
Learning from HumanDrivers
Path  
Planning
Images and movies taken from Sebastian Thrun's multimediawebsite.

Deep Learning in the Headlines

Deep Networks Learn Layered Representations

Deep Belief Net on Face Images
pixels
edges
object parts  
(combination  
ofedges)
objectmodels
Based onmaterials  
by AndrewNg
16

Learning of Object Parts

Training on Multiple Objects
Trained on 4 classes (cars, faces,  
motorbikes,airplanes).
Second layer: Shared-features  
and object-specificfeatures.
Third layer: More specific  
features.

Scene Labeling via Deep Learning
https://www.youtube.com/watch?v=ZJMtDRbqH40

Inference from Deep Learned Models
Zeng Yanhong et al., July 2019. Learning Pyramid-Context  Encoder Network for High-Quality 
Image Inpainting.  https://arxiv.org/pdf/1904.07475.pdf

Impact of Deep Learning on Speech

Impact of Deep Learning on Speech
A Typical Speech RecognitionSystem
ML used to predict of phone states from the soundspectrogram
Deep learning has state-of-the-artresults
# HiddenLayers12481012
Word Error Rate%16.012.811.410.911.011.1
Baseline GMM performance =15.4%
[Zeiler et al. “On rectified linear units for speech  
recognition” ICASSP2013]
21

Deepfake
https://thispersondoesnotexist.com/

Deepfake

Sample Applications

D e f i n i n g  M a c h i n eL e a r n i n g

Potential Definitions
Herbert A. Simon，中文名司马贺，美
国学者、计算机科学家和心理学家，研
究领域涉及认知心理学、计算机科学、
公共行政、经济学、管理学和哲学等多
个方向。为1975年图灵奖得主，1978
获得诺贝尔经济学奖。

Potential Definitions
Tom Michael Mitchell (born August 9, 1951) is an American computer scientist and E. Fredkin
University Professor at the Carnegie Mellon University (CMU). Mitchell is known for his contributions 
to the advancement of machine learning, artificial intelligence, and cognitive neuroscience and is the 
author of the textbook Machine Learning.

Potential Definitions
阿瑟·塞繆爾(December 5, 1901 –July 29, 1990) was an American pioneer in the field of computer 
gaming and artificial intelligence. He popularized the term "machine learning" in 1959. The Samuel 
Checkers-playing Program was among the world's first successful self-learning programs, and as such a 
very early demonstration of the fundamental concept of artificial intelligence (AI). He was also a senior 
member in the TeXcommunity in 1983.
“MachineLearning:Fieldofstudythatgivescomputerstheability
tolearnwithoutbeingexplicitlyprogrammed.”
-ArthurSamuel(1959)
Samuel’s  
CheckersPlayer



When We Need Machine Learning

Example Task: Digit Recognition
Whatisa‘2’?
Imagefrom:Rayaretal.2018.CNNtrainingwithgraph-
based sample preselection: application to handwritten  
character recognition.https://arxiv.org/abs/1712.02122

Example Task: Digit Recognition
Imagefrom:Rayaretal.2018.CNNtrainingwithgraph-
based sample preselection: application to handwritten  
character recognition.https://arxiv.org/abs/1712.02122
Whatisa‘4’?

Example Task: Digit Recognition
Imagefrom:Rayaretal.2018.CNNtrainingwithgraph-
based sample preselection: application to handwritten  
character recognition.https://arxiv.org/abs/1712.02122
Whatisa‘6’?

T y p e   o fL e a r n i n g  P r o b l e m

Types of Learning

Supervised Learning: Regression

Supervised Learning: Regression
•Given (x
1
, y
1
), (x
2
, y
2
), ..., (x
n
,y
n
)
•Learnafunctionf(x)topredictygivenx
yisnumeric==regression

Supervised Learning: Classification
•Given (x
1
, y
1
), (x
2
, y
2
), ..., (x
n
,y
n
)
•Learnafunctionf(x)topredictygivenx
yiscategorical==classification
TumorSize
Malignant
Benign
PredictBenign
PredictMalignant
OcularTumor(Malignant/Benign)
f(x)

Supervised Learning: Classification
•x can be multi-dimensional
Each dimension corresponds to an attribute
TumorSize
Age
-Patientage
-Clumpthickness
-TumorColor
-Distance from opticnerve
-Cell type
-...
Cell type is the most telling feature, but it’s risky to do a biopsy of theeye
•ML can help determine when a feature isneeded

Unsupervised Learning
•Givenx
1
, x
2
, ...,x
n
(withoutlabels)
•Outputhiddenstructurebehindthex’s
oE.g., clustering

Unsupervised Learning
Genomicsapplication:
group genes inneuroblastoma
•Rowsarethegenes
•Columnsarepatientsatriskfor  
neuroblastoma and 
theirstages/outcomes
Imagefrom:Fuscoetal.Chromosomeinstabilityin  
neuroblastoma.OncologyLetters16(6),2018.

Unsupervised Learning Applications
Organize computingclusters
Social networkanalysis
Image credit: NASA/JPL-Caltech/E. Churchwell (Univ. of Wisconsin,Madison)
Astronomical dataanalysis
Marketsegmentation
Slide credit: Andrew Ng

Unsupervised Learning
Independent component analysis –separate a  combined signal into its original sources

Unsupervised Learning
Independent component analysis –separate a  combined signal into its original sources

Reinforcement Learning
Given a sequence of states and actions with  (delayed) rewards, 
output a policy
oPolicy is a mapping from states→actions. It  tells you what 
to do in a given state
Examples:
oCredit assignment problem
oGame playing
oRobotgraspinganobject
oBalanceapoleonyourforehead
Environment
Agent

Reinforcement Learning
https://www.youtube.com/watch?v=4cgWya-wjgY

Reinforcement Learning
https://www.youtube.com/watch?v=iaF43Ze1oeI

H o w   t o   F r a m e   a
L e a r n i n g  P r o b l e m

Learning as Search
•MLusesexperience(data)toapproximateachosentargetfunction
•TherearethousandsofMLalgorithms(moreeveryyear!)

Components of Every ML Application
Optimization
•Howisthemodeltrainedonthedata?
Evaluation
•Howareweassessingwhetherthe  
model issuccessful?
•Whatistheperformancemeasure?
Representation
•Howisthedataspecified?
•Whatistheformofthemodel?

Representations

Optimization and Search Techniques

Evaluation
•Accuracy
•Squarederror
•Precision/ recall
•F-measure
•Utility(expectedcost)
•Likelihood
•Posteriorprobability
•Margin
•Informationgain/Entropy
•KLdivergence

Designing a Learning System
•Choose the trainingexperience
•Choose exactly what is to belearned
–i.e. the targetfunction
•Choose how to represent the targetfunction
•Choose a learning algorithm to infer the target  
function from theexperience
Environment/  
Experience
Learner
Knowledge
Performance  
Element
Trainingdata
Testingdata

Training vs. Test Distribution
•We generally assume that the training and  
test examples are independently drawn from  
the same overall distribution ofdata
–We call this “i.i.d” which stands for “independent  
and identicallydistributed”
•If examples are not independent,requires
collective classification
•If test distribution is different,requires
transferlearning

WORKFLOW OF ML
ML inP r a c t i c e

CASE STUDY IN ML:  ONLINE DATING

CASE STUDY IN ML:  ONLINE DATING

CASE STUDY IN ML:  ONLINE DATING

From a Profile to Features

From a Profile to Features

Obtaining User Feedback

Getting Labeled Data
–
DifferencevectorLabel

Getting Labeled Data
Append featurevectorsLabel

Getting Labeled Data
...
...

Forming the Training Data

Forming the Training Data
...
...

Forming the Training Data
datamatrix
X
labels
y
...
...
labeledtrainingdata

Training the Model

Applying the Model



Some Profiles are Harder than Others

Framing as a Recommendation Problem
Seem to prefer  
similarpeople
SimilarUsers

机器学习vs 知识工程

流行机器学习模型的演变

流行机器学习模型的演变

神经网络的三起两落

人工智能>机器学习>深度学习

传统机器学习：人工设计特征

传统机器学习vs 深度学习

History of MachineLearning
•1950s
–Samuel’s checkerplayer
–Selfridge’s Pandemonium
•1960s:
–Neural networks:Perceptron
–Patternrecognition
–Learning in the limittheory
–Minsky and Papert prove limitations ofPerceptron
•1970s:
–Symbolic conceptinduction
–Winston’s archlearner
–Expert systems and the knowledge acquisitionbottleneck
–Quinlan’sID3
–Michalski’s AQ and soybeandiagnosis
–Scientific discovery withBACON
–Mathematical discovery withAM
51

History of Machine Learning(cont.)
•1980s:
–Advanced decision tree and rulelearning
–Explanation-based Learning(EBL)
–Learning and planning and problemsolving
–Utility problem
–Analogy
–Cognitivearchitectures
–Resurgence of neural networks (connectionism,backpropagation)
–Valiant’s PAC LearningTheory
–Focus on experimentalmethodology
•1990s
–Datamining
–Adaptive software agents and webapplications
–Textlearning
–Reinforcement learning(RL)
–Inductive Logic Programming(ILP)
–Ensembles: Bagging, Boosting, andStacking
–Bayes Netlearning
52

History of Machine Learning(cont.)
•2000s
–Support vector machines & kernelmethods
–Graphicalmodels
–Statistical relationallearning
–Transferlearning
–Sequence labeling
–Collective classification and structuredoutputs
–Computer Systems Applications (Compilers, Debugging, Graphics,Security)
–E-mail management
–Personalized assistants thatlearn
–Learning in robotics andvision
•2010s
–Deep learningsystems
–Learning for big data
–Bayesian methods
–Multi-task & lifelonglearning
–Applications to vision, speech, social networks, learning to read,etc.

一切的开始：感知机

一切的开始：感知机

传统神经网络

传统神经网络

传统神经网络

深度学习发展

深度学习发展

深度学习发展

深度学习发展

深度学习发展

深度学习发展

深度学习发展

深度学习发展
ACM（国际计算机学会）宣布，有“深度学习三巨头”之称
的YoshuaBengio、YannLeCun、GeoffreyHinton共同获
得了2018年的图灵奖。

神经网络结构的发展

神经网络结构的发展

Thanks
ThanksthesupportfromProf.EricEaton,Prof.Dan,andProfCongfortherefertotheirslides.
