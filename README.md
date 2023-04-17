# RefGPT: Reference-to-Dialogue by GPT and for GPT

[[English Version](README_EN.md)]

众所周知，事实正确性是ChatGPT的一大薄弱环节，也是所有试图复现ChatGPT的同行们所面临的重大挑战。想要提升事实正确性，可以标注大量的事实型对话数据（比如人物、科技、医疗、法律、艺术）用于微调GPT模型。为了避免人工标注的昂贵成本，我们提出一种自动生成事实型对话的方法，并公开我们的部分数据。我们公开的第一批数据（RefGPT-Dataset-V1-CN）共包含5万条中文多轮对话。



## 方法介绍


我们的目标是自动生成海量、高质量、事实型多轮对话，用于训练GPT，提升GPT的事实正确性。我们采用如下方法自动生成数据。

1. 采集优质的事实型文档，我们称之为reference，其来源可以是电子书、维基百科、优质垂类网站。文档需要涵盖尽量多的主题，包括但不限于人物、机构、科技、医疗、法律、人文、经济、家居、汽车、出行、美食、时尚、体育、教育、宠物。
2. 利用已有的LLM（例如付费API）生成多轮对话。输入是一篇reference，prompt类似“请根据这篇文章生成多轮问答”。API会输出一段多轮对话（dialogue）。这种方法将原本只适合预训练的文档转化成可供微调的多轮对话。
3. 第2步收集到大量的reference-dialogue二元组。将reference和prompt作为输入，dialogue作为目标，微调一个GPT模型（可以基于LLaMA或BLOOM的预训练基座）。我们将微调出的模型称作*Reference-Enlightened-Dialogue GPT*，缩写*RefGPT*。有了RefGPT，即可基于reference生成多轮对话，获得海量的数据。


外界复现我们的方法时，需要关注2个要点。
1. Reference的质量、广度。Reference内容质量必须高，比如医疗等优质垂类网站的页面、维基百科上非生僻的词条，且需要对网页做清洗。Reference的广度应当大，不能限制在单个垂类或单个网站。
2. 调用已有LLM时需要写prompt，需要仔细尝试各种prompt，使得LLM生成的多轮对话符合预期。


## 数据公开

我们使用上述步骤1和2获得大量多轮对话。我们公开其中部分数据，称之为“RefGPT-Dataset-V1-CN”，包含5万对中文reference-dialogue二元组，其中dialog是基于reference生成的多轮对话。

### 例1：

- Reference：1863年征兵暴动。美国南北战争期间，纽约市工人因对征兵制度不满而举行的四天暴动。法律允许交纳三百美元即可免役，这笔金额仅极少数人能负担。1863年7月11日纽约市点名入伍一开始，大批白人（多为工人）涌上街头，攻打征兵总部，烧毁房屋。在纽约市，白人对于解放后南方来的黑人进入劳工市场，特别是许多雇主雇用黑人破坏罢工，又有嫉恨，于是暴动者时常砸毁无辜的黑人家庭和商店。约一百人死亡（多为暴民）。

- Dialogue：
	- Human: 你知道1863年的征兵暴动吗？
	- Assistant: 是的，那是在美国南北战争期间，纽约市的工人因不满征兵制度而举行的四天暴动。
	- Human: 那么，为什么工人会暴动呢？
	- Assistant: 因为法律允许交纳三百美元即可免役，这笔金额仅极少数人能负担，大多数工人无法承受这样的费用。
	- Human: 那么这场暴动导致了多少损失？
	- Assistant: 暴动者攻打征兵总部，并烧毁了房屋，造成了巨大的财产损失。此外，暴动者还残酷地攻打了一些无辜的黑人家庭和商店，共有约一百人死亡。
	- Human: 为什么暴动者会攻击黑人家庭和商店呢？
	- Assistant: 在纽约市，白人对于解放后南方来的黑人进入劳工市场，特别是许多雇主雇用黑人破坏罢工，有嫉恨，这导致了暴动者时常砸毁无辜的黑人家庭和商店。


### 例2：

- Reference：巴泽雷克冢墓。南西伯利亚早期铁器时代的墓地。位于俄罗斯戈尔诺阿尔泰省的巴泽雷克盆地 。主要包括自南向北联成一线的5 座冢墓。冢墓的发掘为研究阿尔泰早期畜牧部落的体质类型和物质文化提供了珍贵资料。 巴泽雷克冢墓于1927 ～ 1949年间全部发掘 。墓上均有圆形石冢，地下以双层原木构成墓穴。墓室南墙附近放置厚木棺具，棺具表面有禽兽图。室内北面随葬墓主生前遗物 ，墓室外葬有马具和马尸。阿尔泰地区高山酷寒，流入墓中的积水常年冰冻不化，使随葬品中许多有机物得以保存完好 。随葬品有来自中国和西亚的丝织和毛织品，上面饰有极具特色的阿尔泰狮身人面像。还有武器、木器、金银装饰品、乐器和吸食大麻的烟具等。 巴泽雷克冢墓出土的马饰 巴泽雷克冢墓主人为部落首领或贵族，有文身习俗。其妻妾同时殉葬。所有遗体均经过防腐处理。墓主兼具欧罗巴人种和蒙古人种的特征。多数学者认为墓葬的年代在公元前3世纪～前2世纪间。当时处于氏族解体阶段。

- Dialogue：
	- Human: 巴泽雷克冢墓位于哪里？其发掘提供了何种珍贵资料？
	- Assistant: 巴泽雷克冢墓位于俄罗斯戈尔诺阿尔泰省的巴泽雷克盆地。发掘巴泽雷克冢墓为研究阿尔泰早期畜牧部落的体质类型和物质文化提供了珍贵资料。该冢墓发掘于1927~1949年间。
	- Human: 巴泽雷克冢墓的建筑特点是什么？
	- Assistant: 巴泽雷克冢墓上均有圆形石冢，地下以双层原木构成墓穴。墓室南墙附近放置厚木棺具，棺具表面有禽兽图。室内北面随葬墓主生前遗物，室外葬有马具和马尸。
	- Human: 巴泽雷克冢墓出土了哪些随葬品？
	- Assistant: 巴泽雷克冢墓出土了来自中国和西亚的丝织和毛织品，上面饰有极具特色的阿尔泰狮身人面像。还有武器、木器、金银装饰品、乐器和吸食大麻的烟具等。
	- Human: 巴泽雷克冢墓主人的身份是什么？墓葬的年代是什么时候？
	- Assistant: 巴泽雷克冢墓主人为部落首领或贵族，其妻妾同时殉葬。所有遗体均经过防腐处理。墓主兼具欧罗巴人种和蒙古人种的特征。多数学者认为墓葬的年代在公元前3世纪～前2世纪间，那时正处于氏族解体阶段。


### 例3：

- Reference：花露水是主要用作遮盖气味、杀菌、消毒、去污、赋香等方面。花露水一般香气纯净无杂味，在-5℃的低温也不能产生浑浊和沉淀。正因如此，花露水受到越来越多的人喜爱，是人们夏季驱蚊、止痒、杀菌、祛体臭的卫生用品。花露水以乙醇、香精、蒸馏水为主体,辅以少量螯合剂、抗氧剂和耐晒的水溶性颜料,颜色以淡湖蓝、绿、黄为宜。香精用量一般在2%~5%之间，酒精浓度为70%~75%。习惯上香精以清香的薰衣草油为主体,有的产品采用东方香水香型(如玫瑰麝香型),以加强保香能力，称为花露香水。药品名称：花露水。外文名：Florida Water。是否处方药：否。主要用药禁忌：勿口服，避火使用，严重皮炎及皮肤破损者请遵医嘱。剂型：水剂。运动员慎用：否。是否纳入医保：否。花露水基本信息：花露水与香水、古龙水同属于水剂类化妆品。据记载，最开始是有人在医用的消毒酒精（其浓度为75%）里面滴加了一点薰衣草油，就发明了我们所说的“花露水”。在欧美国家，花露水一般是带有薰衣草香味的，然而在我国，由于当时花露水传到我国时，国内还没有薰衣草油，于是调香师配制了比较容易制备的玫瑰麝香香精加入到75%的酒精中，制备成花露水，这种方法配置的花露水在我国盛行了近一个世纪。花露水历史沿革：1905年，香港建厂的广生行开始生产国人第一款花露水“双妹牌”。“花露”二字取自宋朝文学家欧阳修之词《阮郎归·南园春半踏青时》中的“花露重，草烟低，人家帘幕垂”。1908年（清光绪34年）中国大陆花露水最早源起于明星花露水，最早的诞生地号称十里洋场的上海。当时颇具盛名的上海中西大药房，研发出一种盛装在绿色玻璃瓶的花露水。并将这瓶透明绿的香水取名为「明星花露水」，并且将Logo设计为一个拉著舞衣裙摆款款答礼的女孩，主攻女性市场。随即花露水席卷上海，一跃而成知名的国产香水。负责生产明星花露水的化妆品部门也因此从中西药房中独立出来，不久上海中西大药房的经营权转移，同时挂牌上市。上海中西大药房采用明星化工经营，并且推出香皂、香粉、发油等周边商品。1946年，明星化工着眼于南部地区气候炎热，更能接受兼具消毒功能的花露水，开始在广州、台湾陆续设立办事处。然而，战乱改变了时局。1950年，明星花露水的商标被上海家庭化学取得，上海中西大药房老板周文玑在紊乱时局下匆促带着几名员工逃到台湾。

- Dialogue：
	- Human: 请问花露水都有哪些主要用途？
	- Assistant: 花露水主要用于遮盖气味、杀菌、消毒、去污和赋香等方面。由于其香气纯净和具有一定的杀菌消毒功能，花露水在夏季可以用于驱蚊、止痒、杀菌和祛除体臭。可以说其功效非常实用且受到很多人的喜爱。
	- Human: 花露水是如何制作的？主要成分是什么？
	- Assistant: 花露水的制作主要以乙醇、香精和蒸馏水为主体，辅以少量螯合剂、抗氧剂及耐晒的水溶性颜料。调整颜色，以淡湖蓝、绿、黄为宜。香精用量一般在2％～5％之间，酒精浓度为70％～75％。在制作花露水的过程中，香精通常以清香的薰衣草油为主体，有些产品也会采用东方香水香型（如玫瑰麝香型）来加强保香能力，称之为花露香水。
	- Human: 中国的花露水与欧美有什么区别？
	- Assistant: 中国的花露水与欧美的主要区别在于香型。欧美的花露水通常以薰衣草香味为主，而在中国花露水流行起初，由于国内尚未有薰衣草油，调香师便使用较易制备的玫瑰麝香香精，将其加入到75%的酒精中，制备成具有特色的花露水。如此制作出的花露水在中国盛行了近一个世纪，成为国人所熟知的、具有特色的香型。
	- Human: 能否介绍一下花露水的发展史？
	- Assistant: 当然可以。1905年，香港的广生行开始生产国人第一款花露水——“双妹牌”。花露水传入中国大陆的最早时间应为1908年，源起于上海制作的明星花露水。当时，著名的上海中西大药房研发出一款装在绿色玻璃瓶里的花露水，并将其命名为“明星花露水”，以一个拉着舞衣裙摆款款答礼的女孩作为Logo，主打女性市场。之后，花露水在上海迅速流行，成为著名的国产香水。负责生产明星花露水的化妆品部门从中西药房中独立出来，并上市。1946年，明星化工着眼南部地区炎热气候，发现更能接受具有消毒功能的花露水，所以开始在广州、台湾设立办事处。但是1950年，战乱改变了局势，明星花露水商标被上海家庭化学取得。
	- Human: 关于使用花露水有什么注意事项？
	- Assistant: 在使用花露水时，请注意以下事项：
		- 1. 花露水不得口服，因为其中含有酒精和香精成分，口服可能会导致身体不适。
		- 2. 避免火源接触，因为花露水含有高浓度酒精，具有一定的火灾隐患。
		- 3. 对于严重皮炎和皮肤破损者，请在使用前遵从医生建议，以免可能对皮肤造成刺激和不良反应。
		- 4. 请注意保存环境，避免高温、潮湿，以及避免阳光直射，以免影响花露水的质量及使用效果。




### **注：**

1. 本次公开的数据（V1-CN）只包含中文。对于其他语言，可以采用类似的方法用reference生成dialog，也可以将我们公开的数据翻译成其他语言。我们计划在未来公布英文数据。
2. 本次公开的数据（V1-CN）未经人工校验，无法严格保证正确性、安全性。使用者需要对数据产生的结果负责。



## RefGPT模型

我们使用某些已有的LLM生成了大量的多轮对话（公开了其中5万段）。我们用所有的reference-dialogue二元组微调GPT，将得到的模型称为RefGPT，RefGPT是单任务模型，专门根据reference生成dialogue。我们可能会在未来公开RefGPT，届时同行可以用RefGPT生成多轮对话，最后将多轮对话用于复现ChatGPT。


## FAQ

- 问1：你们爬取了哪些网站或书籍？你们用的是GPT-3.5或GPT-4的API吗？
- 答1：由于潜在的法务风险，我们不便回答这些细节问题。

- 问2：你们的数据V1-CN包含5万条样本，只是你们数据的一部分对吗？可以公开全部吗？
- 答2：我们获取了大量reference-dialogue二元组用于训练RefGPT。我们公开的是全部数据的一个子集，以后会考虑公开全部。

- 问3：有了RefGPT，可以基于网页、电子书生成海量的多轮对话，对吗？你们是否打算公开RefGPT生成的数据？
- 答3：有了RefGPT，任何人都可以低成本生成海量的多轮对话，但前提是爬取和清洗网页、电子书。我们不会公开生成的数据，原因是爬取数据涉及法务风险。

- 问4：你们本次公布的数据只有中文，之后会公开其他语言的数据吗？
- 答4：我们计划公布英文数据，量级也是5万，请大家耐心等待。



## 主要贡献者

1. 杨东杰, djyang.tony@sjtu.edu.cn
2. [袁瑞峰](https://scholar.google.com.hk/citations?user=zPj0R-8AAAAJ&hl=zh-CN), ruifeng.yuan@connect.polyu.hk
3. 范元涛, yuantaofan@bupt.edu.cn
4. 杨逸飞, yifeiyang@sjtu.edu.cn
5. [汪自力](https://commencement.github.io/), ziliwang.do@gmail.com
6. [王树森](http://wangshusen.github.io/)，wssatzju@gmail.com



## 引用

如果你使用我们的方法或数据，请引用本GitHub Repo。

```bibtex
@misc{refgpt,
  author = {Dongjie Yang and Ruifeng Yuan and Yuantao Fan and and Yifei Yang and Zili Wang and Shusen Wang},
  title = {RefGPT: Reference-to-Dialogue by GPT and for GPT},
  year = {2023},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/ziliwangnlp/RefGPT}},
}
```




