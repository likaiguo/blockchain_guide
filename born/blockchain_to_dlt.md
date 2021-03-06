从区块链到分布式账本
===

<!--
报告对全球区块链和分布式账本领域的技术与平台进行了分析和评价。
区块链来源自分布式记账的需求，先后曾掀起三波研究和应用热潮。狭义上，区块链（Blockchain）是一种以区块为基本单位的链式数据结构，区块中利用数字摘要对之前的交易历史进行校验，适合分布式记账场景下防篡改的需求。广义上，指代基于区块链结构实现的分布式记账技术，还包括分布式共识、隐私与安全保护、点对点通信技术、网络协议、智能合约等。
类比互联网解决了传递“信息”的问题，区块链解决了传递“可信信息”的问题。区块链引发的记账科技的演进，将促使商业协作和组织形态发生变革。核心价值在于为多方协同提供可信基础。目前，区块链被认为是第四次工业革命的核心成果，将成为继互联网之后的重要基础设施。目前，虽然从技术上讲仍处于发展早期，但商业应用已经在加速落地。
以太坊（Ethereum）等开源项目面向公有链领域，可满足小规模、低性能场景的需求；全球科技金融领域巨头们合作推出的超级账本（Hyperledger）开源项目，作为面向商业应用的联盟链最新成果，已经成为未来商业网络的重要规范和实践标准，在性能、安全、智能合约、管理等指标上都取得了重要突破。
-->

随着最前沿的信息科技成果不断融入金融行业，以区块链（Blockchain）为基础的分布式账本科技（Distributed Ledger Technology，DLT）崭露头角，并在部分场景（如跨境支付）中得到探索和落地。从最早的简单账本到复式账本，再到数字化账本，以及目前正在探索的分布式账本，账本科技的每次突破都会引发金融领域的重要革新，也往往对社会生活的各个方面进一步产生阶段性的影响。

方兴未艾之际，理清区块链的来龙去脉和相关概念、研讨从区块链到分布式账本科技的演化规律，对金融领域正确看待和应用这一科技成果至关重要。


## 从分布式记账问题说起

分布式记账问题由来已久。

自电子计算机发明以来，数字化账本因为其便捷高效的特性很快就成为最主要的记账媒介。然而，数字化账本仍然是中心化的形式，这就意味着参与商业活动的多方首先要寻找一个能共同信任的第三方来记账，以确保交易记录的准确。

随着商业活动的规模越来越大，商业过程愈加动态和复杂（例如供应链领域动辄涉及来自多个行业的数百家参与企业），很多时候难以找到同时满足共同信任和隐私安全的第三方记账方，即便存在也往往需要较高的使用成本。这就需要探讨在多方分布式场景下进行协同记账的可能性。

实际上，从分布式记账问题出发可以很容易设计出一个简单、粗暴的分布式记账结构，如下图所示方案（一）。该方案中，账本为一个记录队列，由多方共同维护，多个参与方均对账本中记录拥有操作权限，并相互约定：一旦发生新的交易即追加到账本上，已发生的交易记录不得进行篡改。这种情况下，如果参与多方均诚实可靠依照约定执行，则该记账方案可以正常工作。但是一旦有参与方恶意操作，篡改已发生过的记录，则无法确保账本记录的正确性，并且他人无法获知篡改是否发生。

![方案（一）：简单分布式记账结构](dlt-01.png)

为了防止有参与者对交易记录进行篡改，需要引入一定的验证机制，核心为对已发生过的交易历史进行校验。很自然地，可以借鉴信息安全领域的数字摘要（Digital Digest）技术，从而改进为方案（二）：每次当有新的交易记录被追加到账本上时，参与各方可以使用约定的摘要算法对完整的交易历史计算数字摘要，获取当前交易历史的“指纹”。此后任意时刻，任何参与方都可以随时对交易历史重新计算摘要，一旦发现指纹不匹配，则说明交易记录被篡改过。同时，通过追踪指纹改变位置，还可以定位到被篡改的交易记录。

![方案（二）：带有数字摘要验证的分布式记账](dlt-02.png)

方案（二）可以解决账本记录防篡改的问题，然而在实际生产应用时，仍存在较大缺陷——不可扩展。由于每次校验需要从头对所有的历史数据计算数字摘要，当账本中存有大量历史交易时，数字摘要计算成本将变得很高。而且，随着新交易的发生，计算耗费将越来越大，该方案将无法支撑大规模的记账情形。

为了解决可扩展性的问题，需要进一步改进为方案（三）。注意到每次摘要时实际上已经确保了从头开始到摘要位置的历史的正确性，因此当新的交易发生后，实际上需要进行额外验证的只是新发生的若干交易，即增量部分。因此，计算摘要的过程可以改进为对旧的摘要值以及增量交易内容进行验证。这样就既解决了防篡改问题，又解决了可扩展性问题。

![方案（三）：带有数字摘要验证的可扩展的分布式记账](dlt-03.png)

实际上，读者可能已经注意到，方案（三）中的账本结构实际上就是一个区块链结构，如下图所示。 

![区块链结构](blockchain.png)

可见，**从分布式记账的基本问题出发，可以自然推导出区块链结构**。这也说明了在分布式记账问题的解决中，区块链结构是一个简洁有效的答案。

当然，区块链结构并非唯一能满足分布式记账需求的结构。另一方面，为了满足实际应用场景的需求，还需要额外考虑如何维护可扩展的账本结构、如何对使用多方进行身份验证、如何进行权限管理、如何保护隐私……等诸多系统性的问题。


## 区块链概念三次热潮

狭义上，区块链是一种以区块为基本单位的链式数据结构，区块中利用数字摘要记录历史区块数据而进行校验，适合分布式记账场景下防篡改和可扩展性的需求。

广义上，区块链还指代基于区块链结构实现的分布式记账技术，还包括分布式共识、隐私与安全保护、点对点通信技术、网络协议、智能合约等。

从比特币项目诞生之日（2009 年 1 月）算起，区块链已在全世界掀起了三次关注热潮。

![区块链的三次热潮](3-hops.png)

第一波热潮出现在 2013 年左右。比特币项目上线后，很长一段时间里并未获得太多关注。直到比特币价格在 2013 年发生暴涨，各种加密货币项目纷纷出现，隐藏在其后的区块链结构才首次引发大家的兴趣。2014 年起，“区块链”术语开始频繁出现，但更多集中在加密货币相关领域；

第二波热潮出现在 2016 年前后。以区块链结构为基础的分布式账本技术被证实在众多商业领域存在应用潜力。尤其 2015 年 10 月《经济学人》刊物的封面文章《信任机器》中正式指出，区块链为基础的账本平台有潜力改变人们和企业之间互相协作的方式。更多实验性应用出现，集中在金融、供应链、贸易等场景。下半年更是出现了“初始代币发行（Initial Coin Offering，ICO）”等新型众筹募集形式。这一时期，区块链技术自身也有了突破，首个大规模公有智能合约引擎——以太坊项目正式上线；首个面向企业应用的联盟分布式账本——超级账本项目，在众多企业巨头的支持下也正式成立。

随着更多商业项目开始落地，从 2017 年开始至今，众多互联网领域的资本开始关注到区块链和分布式账本领域。北京、新加坡、硅谷等地持续涌现创业团队，传统企业也开始着手研发分布式账本方案，人才缺口持续加大。区块链相关科技俨然已经成为继人工智能之后的又一资本热点。

分析这三次热潮可以看出，每次热潮的出现都与金融行业对区块链记账科技的深化应用和密切关注有关。这也表明金融行业对最前沿的科技成果始终保持了较高的敏感度和接纳度。


## 分布式记账的重要性

分布式记账问题为何重要？笔者认为，可以类比互联网出现后对社会带来的重大影响。

互联网是人类历史上最大的分布式互联（inter-connect）系统。作为信息社会的基础设施，互联网很好地解决了传递信息的问题。然而，由于早期设计上的缺陷，互联网无法确保所传递信息的可靠性，这大大制约了人们利用互联网进行大规模协作的能力。而以区块链为基础的分布式账本科技则可能解决传递可信信息的问题，在互联基础上提供可信保障。这意味着基于分布式账本科技的未来商业网络，将成为继互联网之后的新一代基础设施——大规模协作（collaboration）系统。

笔者相信，分布式账本科技的核心价值，在于为大规模多方协作网络提供可信基础。而区块链引发的记账科技的演进，将促使商业协作和组织形态发生变革。世界经济论坛执行主席 Klaus Schwab 甚至认为 “区块链是（继蒸汽机、电气化、计算机之后）第四次工业革命的核心（Blockchains are at the heart of the Fourth Industrial Revolution）”。

## 分布式账本的现状与未来

从科技发展的一般规律而言，笔者认为，分布式账本科技仍处于发展早期，但商业应用已经在加速落地。同样作为基础设施，可以类比互联网的发展过程，如下表所示。

时期 | 互联网 | 区块链 | 时期
-- | -- | -- | --
1974~1983 | ARPANet 内部试验网络 | 比特币试验网络 | 2009~2014
1984~1993 | TCP/IP 基础协议确立，基础架构完成 | 探索基础协议和框架，出现超级账本、以太坊等开源项目 | 2014~2019?
1990s~2000s | HTTP 应用协议出现；互联网正式进入商用领域 | 商业应用的加速落地，但仍未出现杀手级应用 | 2018~？
2000s~？ | 商业互联网普及 | 商业协作网络普及 | ？

互联网在其发展过程中，先后经历了试验网络、基础架构和协议、商业应用、大规模普及等四个时期，每个阶段都花费了10年左右的时间。其中第二个时期尤为关键，在此期间，TCP/IP 网络传输控制协议最终从已有的众多网络控制协议中胜出，成为核心协议，这为互联网后来扩展到全球规模奠定了扎实的技术基础。

作为前所未有的大规模协作网络，分布式账本网络的发展很大可能也要经历这四个阶段的演化。当然，站在前人肩膀上，相信无论是演化速度还是路线选择上，都会有不小的优势。

客观来看，虽然超级账本、以太坊等开源项目在基础协议和框架方面已经进行了诸多探索，并取得了重要成果。但在可扩展性、多账本系统互联、与已有业务平台的互操作性等方面还存在不足，同时，商业应用的广度和深度仍需实践的考验。

但毫无疑问，分布式账本科技已经成为金融科技领域的核心创新成果，所带来的协作效率提升将为金融行业创造新的发展机遇。继互联网之后，未来商业协作网络必将在全球范围内产生深远的影响。

