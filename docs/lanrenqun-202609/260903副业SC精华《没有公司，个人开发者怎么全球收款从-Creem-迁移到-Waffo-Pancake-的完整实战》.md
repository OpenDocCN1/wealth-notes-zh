


# 没有公司，个人开发者怎么全球收款？从 Creem 迁移到 Waffo Pancake 的完整实战

260903 副业 SC 精华
整理：公众号懒人搜索，懒人专属群精选
懒人微信：lazyhelper1

![](img/67c37faeb967b1478183a189236a069f_0_0.png)

> 小懒判断 | 被 Creem 封号卡住收款的出海个人开发者直接照着走，网站还没上线的新手先收藏，注册审核要看你的真实产品站。最值钱的是那张费率表：国际卡 3.9%加 0.5 刀，2.99 美元低价包手续费占比约 20%，定价前必须先算这笔账。坑在 AIGC 审核，法律页写得太泛、AUP 缺了六类禁令或举报邮箱，第一次基本会被打回。

大家好，我是独孤风。生财 6 年圈友。

第二期小排老师 AI 编程深海圈学员，深海圈千刀户认证高手。

我在生财参加很多项目，从去年开始全力做 AI 产品网站出海，由于主业工作限制，我一直没有注册公司，也没有开通港卡，所以做付费 AI 产品还是有很多的限制。

之前一直用 Creem 做付费接入，今年的 5 月份开始，Creem 开始大面积风控，我的账号也难逃被封的命运，申诉了几次都没有成功，这个期间也损失了很多收入。具体的情况小排老师前一阵做过分析，大家可以看 [Creem 之后，AI 产品收款用这两家-刘小排](#)。

我目前已经完成了从 Creem 向 Waffo Pancake 的迁移工作。

我完成的主要工作如下，如果大家也和我面临同样的问题，希望这篇文章能对你有所帮助。

- 我没有海外公司，也没有营业执照；
- 用中国大陆身份证完成了个人身份认证；
- 通过了 Waffo Pancake 的生产审核；
- 一次性积分包和月付、年付订阅都已接入，并成功收款；
- 绑定了个人支付宝和国内银行卡，可以提现；

本文写于 2026 年 8 月 31 日。支付费率、支持国家、提款方式和审核政策都可能变化，文中的数字是我申请时的官方文档、后台页面和工作人员回复口径，实际申请前请再核对一次官网。

我目前也在不断的进化中，如果文中有任何纰漏，希望大家及时指正，不要误导他人。

## 一、Waffo Pancake 到底是什么？

接入之前大家先分清 Waffo 和 Waffo Pancake。

这是我踩的第一个坑，很多人听说 Waffo 可以接入，直接就去申请，发现自己没有公司就放弃了。

Waffo 市场经理在邮件里给我解释了 Waffo 的两条路线：

| 路线 | 适合谁 | 主体要求 | 典型用途 |
| :--- | :--- | :--- | :--- |
| Waffo 主通道 | 已公司化运营的商户 | 香港、新加坡、日本、美国等公司主体 | 企业收单、多支付方式、对公结算 |
| Waffo Pancake | 独立开发者、Startup、AI/SaaS 小团队 | 可以没有海外公司，以个人身份申请 | MoR 全球收款、订阅、积分包、个人提款 |

所以，没有公司的个人开发者，正确入口不是企业 Sandbox，而是：

https://pancake.waffo.ai

官方文档是：

https://docs.waffo.ai

Waffo 团队给我的口径是：Pancake 就是面向 AI 个人开发者和 Startup 的产品，不刚需公司主体；如果品类不在禁止清单内，可以直接提交店铺审核，由风控进一步判断。AIGC 会审得更细，耗时也可能更长。

![](img/67c37faeb967b1478183a189236a069f_2_0.png)

### 那么 Waffo Pancake 到底是什么？

先交代一下背景。Waffo 是一家总部位于香港的全球支付与商业化平台，Waffo Pancake 是其面向开发者和数字产品的 MoR 产品。按照 Waffo 2026 年 1 月的公告，公司累计融资 3,000 万美元，投资方包括 Illuminate Financial、高榕资本、HSBC 和 BAI Capital；其官方资料同时披露了香港 MSO 牌照和 PCI DSS 认证。

Waffo Pancake 不是一个只帮你刷卡的普通支付网关，而是 Merchant of Record，简称 MoR，中文一般叫“记录商户”或“名义商户”。

普通支付处理商只处理钱。税务、发票、退款、拒付以及不同国家的销售税，法律责任仍然主要在这里。

MoR 的链路是：

客户向 Waffo Pancake 购买 → Waffo 作为法律意义上的销售方收款并处理税务 → 扣除费用后把收入结算给你。

按照 Waffo 官方说明，它会处理全球税费计算与缴纳、发票、支付、退款、拒付和争议。你仍然要对产品本身、内容合规、用户交付和客户服务负责，但不用一个人研究每个国家的 VAT、GST 和 Sales Tax。

### 关于费率、退款、拒付和提款成本

这是大家最关心的部分。我按 2026 年 8 月 30 日的 Waffo 官方费率页整理如下。

## 1. 成功交易

| 支付方式 | 费用 |
| :--- | :--- |
| 国际卡、数字钱包 | 3.9% + $0.50/笔 |
| 微信支付 | 3.9%，没有 $0.50 固定费 |
| 月费、接入费 | 0 |

可以看出，固定的 $0.50 对低客单价影响很大。例如 $2.99 的 Trial Pack，如果用户刷卡，手续费约为：

$2.99 × 3.9% + $0.50 = $0.6166，交易手续费占订单金额约 20.6%。如果走微信支付，则只有约 $0.1166 的比例费。

这个大家定价的时候要考虑进去，不要最后赔钱了。要把支付费、模型 API 成本、退款和获客成本一起算进定价。

## 2. 失败交易

如果卡支付进入 3DS 验证后失败，部分失败类型会收取 $0.30/次。用户没有进入 3DS 就离开，则不收费。

这意味着高风险流量不仅转化差，还可能产生失败交易成本，不能只看成功订单手续费。

## 3. 退款

- 每笔退款处理费：$1；
- 原成功交易的 3.9% + $0.50 不退；
- 全额退款和部分退款都一样。

## 4. 提款

- 最低提款金额：$20；
- 提款手续费：1%，每次最低 $10；
- 销售后约 10 个工作日进入可提款余额；
- 发起提款后通常 1 至 3 个工作日到账；
- 目前中国大陆以人民币结算，可提到银行卡或支付宝。

因为每次提款最低收 $10，小额商户不要频繁提现。比如只提 $500，1% 本来是 $5，但仍按最低 $10 收；提 $2,000 才按 1% 收 $20。

Waffo 工作人员当时还给了我以下额度口径：

- 支付宝：单笔不超过 5 万元人民币，每人每年累计不超过 30 万元；
- 银行卡：单笔/单日 50 万元，月累计 200 万元；
- 美元提款计划在 9 月上线，方向包括 Wise 和香港汇丰。

这是当时回我邮件的截图，他们也在不断的更新中，大家还是要问清楚自己当时的情况。

5. 个人开发者需要准备的：目前公司和个人在审核上没有渠道差异，后面需要对公提款时才会收集公司信息。两点建议提前准备好：①申请时填写产品真正的域名——我们审核的是您的产品网站，审核通过后不支持任意更改；②支持邮箱最好用与网站同域名后缀的。审核时长一般 1-3 个工作日，可以先提交。

6. 国内银行卡提现：目前提款支持人民币转到国内银行卡和支付宝。结算周期是 T+10 个工作日（以香港节假日为准），结算后发起提现，到账 1-3 个工作日。提现手续费是 1%，最低 $10。限额方面：支付宝单笔 5 万人民币，年限额 30 万；银行卡限额/单笔 50 万人民币、月累计 200 万。另外美元提款九月份上线（支持 Wise 与香港汇丰银行），上线后我们会在群里同步。

## 二、个人开发者申请与通过审核攻略

Pancake 的个人身份认证需要：

- First name：名的拼音/拉丁字符；
- Last name：姓的拼音/拉丁字符；
- Legal full name：身份证上的中文法定姓名；
- Country/Region：CN；
- ID Type：大陆居民身份证或护照；
- ID Number：18 位身份证号，或符合规则的护照号；
- 可以收验证码的客服邮箱；
- 产品域名的控制权；
- 与法定姓名一致的银行卡或支付宝账户。

注意，英文名字段填拼音，法定姓名字段填身份证上的中文姓名。法定姓名以后会成为提款户名，空格、标点或顺序不一致，都可能导致打款失败。

Waffo 团队建议使用和网站同域名的邮箱，这样更容易证明产品归属，也能减少“网站邮箱与审核邮箱不一致”的问题。

### 如何保证审核通过？

Waffo 对于 AIGC 的审查还是挺严格的。

Waffo 的禁止与限制品类里，AIGC 属于“限制品类”，不是默认禁止品类。官方要求至少包括：

- 不能侵犯第三方知识产权；
- 要清楚披露内容由 AI 生成；
- 使用自己的品牌，不能冒充某个模型的官方产品；
- 如实披露实际使用的底层模型；
- 对提示词和生成结果做内容审核；
- 发布公开的 AUP（可接受使用政策）；
- 提供真实的违规内容举报渠道；
- 保存必要的审核和处置记录。

Waffo 的 AIGC 合规文档还要求，AUP 至少明确禁止六类内容：

- 色情和 NSFW；
- 暴力、血腥；
- 仇恨言论；
- 儿童不安全内容；
- Deepfake、未经授权冒充他人；
- 侵犯版权或商标。

平台会检查：有没有逐项列出、有没有举报邮箱、收到举报后怎么处理、账号是否会被限制或封禁，以及你的代码里是否真的有审核。

我第一次提交后，Waffo 没有永久拒绝，而是给了 needs_info / awaiting_information，并明确点名了几个功能不合规。

合规团队认为，这些功能在 Visa/Mastercard 的口径下涉及 Deepfake 或高风险人物内容，给了我两个选择：

### 方案一：下线高风险功能

整改完成后，重新申请国际卡、钱包和本地支付方式的全渠道审核。

### 方案二：保留高风险功能

只能申请微信支付，不能开国际卡渠道。

我的目标用户在海外，所以选择了方案一。

这里我做了一个很重要的取舍：下线四个具体高风险工具，但保留其他核心产品和 SEO 资产。

合规整改不等于把整个站的 SEO 全砍掉。应该下线的是具体高风险能力，不是把正常产品方向一起删掉。

提交前一定要做好自查，Waffo 官方的店铺审核清单很实用。我结合自己两次网站自检，把最低要求整理成一张表：

| 项目 | 最低要求 |
| :--- | :--- |
| 产品 | 公开可访问，不能只是 Coming Soon，也不能必须登录后才能理解产品 |
| 定价 | 用户进入收银台前就能看到价格、周期和获得的权益 |
| Terms | 写清服务、计费、取消、退款、积分有效期、禁止用途 |
| Privacy | 写清收集的数据、用途、第三方服务商、跨境处理、保留和删除 |
| AUP | 对 AIGC 六类禁止内容、审核、举报和处置逐项说明 |
| 客服 | 全站页脚直接展示有效邮箱，最好与域名同后缀 |
| 退款 | 和收银台、Terms 里的规则保持一致 |
| 品牌 | 不能冒充模型官方，也不要蹭他人商标 |
| 营销信息 | 不要写虚假用户数、虚假评价和无法证明的能力 |
| 模型披露 | 如实列出使用的底层模型或在生成界面展示模型选项 |

我的网站第一次自检失败，就是法律页写得太泛：

- 没写清个人经营主体和 MoR 关系；
- 没写一次性积分包、月订阅和年订阅的差别；
- 没写积分有效期、取消和退款；
- 没列出真实的数据处理服务商；
- AUP 没有用户举报渠道；
- 页脚没有直接显示客服邮箱。

整改后，我在 AUP 中增加了：用户可以向客服邮箱发送 Content Report，附上链接、截图、账号或订单信息；平台会做初步分级、自动审核和人工复核，必要时删除内容、限制功能或封禁账号。

同时把支持邮箱直接放到全站页脚，让用户不用进入 Contact 页面也能联系。

## 三、个人开发者申请的完整步骤

下面按我的真实顺序写，大家可以照着走。

### 第 1 步：先做品类自查

先看官方禁止与限制品类：

https://docs.waffo.ai/zh/mor/prohibited-products

如果涉及 Deepfake、未经授权的人脸/声音克隆、NSFW、侵权、赌博、破解软件、刷量或其他高风险业务，不要抱着“先提交试试”的心态。先改产品，再申请。

### 第 2 步：注册 Pancake

打开：

https://pancake.waffo.ai

我的选择是：

- **身份**：个人；
- **国家/地区**：中国；
- **产品类目**：SaaS；
- **计费模式**：订阅制；

![](img/67c37faeb967b1478183a189236a069f_10_0.png)

建议大家按真实的情况填写。Waffo 团队明确提醒，审核的是这个产品网站，通过后不支持随意换成完全不同的网站。

### 第 3 步：创建商店和第一个商品

商店名正常填写。

一开始不用把所有套餐都建完。我先建了一个最低价 Trial Pack：

- 一次性购买；
- $2.99；

![](img/67c37faeb967b1478183a189236a069f_11_0.png)

先用一个低价真实商品把测试支付、Webhook 和积分发放跑通，再补其他套餐，排错会简单很多。

![](img/67c37faeb967b1478183a189236a069f_12_0.png)

这里直接进入 Dashboard 就可以，配置 API 密钥和代码接入，其他的设置，进去以后都可以做。

### 第 4 步：按步骤完成设置

这就是 Waffo 的 Dashboard 了，右下角有新手指南，可以按步骤完成设置。

![](img/67c37faeb967b1478183a189236a069f_12_1.png)

一共有 9 个步骤，大家要善用 AI，我基本每个步骤都要“command + command”截图到 codex 里，让 AI 核实网站的具体情况。

再做回答。

![](img/67c37faeb967b1478183a189236a069f_13_0.png)

我这里提交审核那部分简单列一下我的填写，大家结合自己的情况核对。

![](img/67c37faeb967b1478183a189236a069f_13_1.png)

业务类型肯定是个人。

![](img/67c37faeb967b1478183a189236a069f_13_2.png)

地区选中国，协议肯定都同意。

![](img/67c37faeb967b1478183a189236a069f_14_0.png)

业务详情我选的已经准备好。

![](img/67c37faeb967b1478183a189236a069f_14_1.png)

这个如实填写，有付费用户就选有。

![](img/67c37faeb967b1478183a189236a069f_14_2.png)

这里要写真实姓名的全拼，比如张三，就写 Zhang San。

![](img/67c37faeb967b1478183a189236a069f_14_3.png)

这里写店铺名，之前写过默认就会在这里，这个固定一个名字，不要乱改。

![](img/67c37faeb967b1478183a189236a069f_15_0.png)

联系邮箱写网站域名的域名邮箱。这个验证的，一定要准确。

![](img/67c37faeb967b1478183a189236a069f_15_1.png)

这个不用说了，完整域名。

![](img/67c37faeb967b1478183a189236a069f_15_2.png)

![](img/67c37faeb967b1478183a189236a069f_16_0.png)

Pancake 会问现有客户、商家名称、业务介绍、产品详情、目标客户、定价和网站链接等问题。

这部分可以让 AI 结合自己的产品写，这段介绍的重点不是写得多漂亮，而是让审核员快速看懂五件事：

1. 你是谁；
2. 产品做什么；
3. 用户如何使用；
4. 怎么收费；
5. 你如何控制风险。

![](img/67c37faeb967b1478183a189236a069f_16_1.png)

合规性检查，肯定都同意。

这里把产品域名、Terms、Privacy 填好后运行网站自检。AIGC 产品还要核对 AUP、客服邮箱、模型披露和内容审核。

https://docs.waffo.ai/zh/api-reference/endpoints/content-safety/scan-prompt 这是 AIGC 产品的审核接口，可以让 AI 跑一下。

自检通过不等于人工审核一定通过，这是基本前提，自检不过就让 AI 去阅读官方文档，不断排查问题。

![](img/67c37faeb967b1478183a189236a069f_17_0.png)

最后让你确定下手续费，就可以提交审核了。

![](img/67c37faeb967b1478183a189236a069f_17_1.png)

审核的很快，我基本上提交完，半天就得到回复了，大家一定要看好邮箱，第一次申请打回来的概率很大。赶紧修改继续申请。

## 四、审核通过后，完成代码接入

别忘了，我们的网站还没有接入完整的支付。

Waffo 是绝对 AI 友好的，接入只需要对 AI 说一句话。

```
Read https://docs.waffo.ai/llms-full.txt, load the official Waffo Pancake skill from https://docs.waffo.ai/zh/integrate/skill, and integrate Waffo Pancake payments into the current project.
```

![](img/67c37faeb967b1478183a189236a069f_18_0.png)

很快就接入成功了。

### 1. 创建并映射商品

把网站的付费商品加入。

包括一次性付费和订阅的。

![](img/67c37faeb967b1478183a189236a069f_18_1.png)

一个个耐心的设置。每个商品都有独立 Product ID。代码里不要用价格猜商品，也不要把 Product ID 散落在前端，而要用自己网站的套餐 ID 映射到 Waffo Product ID。

## 2. 服务端创建 Checkout

Waffo 提供 TypeScript SDK。服务端使用 Merchant ID 和 Private Key 创建 Checkout，再把托管收银台 URL 返回给浏览器。

Private Key 只能放服务端 Secret，不能：



## 3. 配置生产 Webhook

在 Pancake Dashboard 的设置 → Webhooks → Production 添加，格式选择 Raw。

一次性积分包至少监听：

- order.completed

订阅至少监听：

- subscription.activated
- subscription.payment_succeeded
- subscription.updated
- subscription.canceling
- subscription.uncanceled
- subscription.canceled
- subscription.past_due

退款还要监听：

## 五、设置提款账户，验证，上线

别忘了做提款账户的设置。

![](img/67c37faeb967b1478183a189236a069f_20_0.png)

包括，姓名的拼音，中文名，身份证号，国家地区。

![](img/67c37faeb967b1478183a189236a069f_20_1.png)

提款方式可以选择支付宝或者银行卡。

随后，就可以做支付的验证了。可以先验证自己微信支付一下，国外的银行卡，Waffo 给我的批准邮件写的是“160+ countries”；当前官方 MoR 文档显示支持国家数为 173。这个数字会随着平台能力变化，不必纠结某个固定数量，申请时以 Dashboard 实际开放的支付国家和方式为准。

上线之前，把右上角切换到生产模式。

![](img/67c37faeb967b1478183a189236a069f_20_2.png)

然后去 API 与开发里创建生产的 key。key不知道怎么配置也可以交给 AI，都有官方的提示词建议。

至此全部流程都完事了，希望这次从Creem 被关，到 Waffo 真实收款跑通的完整过程，能帮大家少踩几个坑。

祝大家都审核通过，早日月入万刀！ 我是独孤风，大家有问题欢迎留言，需要联系我请联系鱼丸。

![](img/67c37faeb967b1478183a189236a069f_21_0.png)

## 附：Waffo Pancake 官方入口与文档索引

下面只收录 Waffo 官方入口。文档和产品能力可能更新，遇到本文与后台不一致时，以你申请当天的 Dashboard、官方文档和客服书面回复为准。

### 1. 官网、注册与帮助

- Waffo Pancake 官网：https://waffo.ai
- Waffo 开发者产品站：https://waffo.dev
- Waffo 公司官网：https://waffo.com
- Pancake 注册与登录：https://pancake.waffo.ai
- 中文文档首页：https://docs.waffo.ai/zh
- 快速开始：https://docs.waffo.ai/zh/quickstart
- 功能总览：https://docs.waffo.ai/zh/feature-list
- 官方支持邮箱：support@waffo.ai

### 2. MoR、费率与支持范围

- 什么是 MoR：https://docs.waffo.ai/zh/mor/what-is-mor
- 最新交易费率：https://docs.waffo.ai/zh/mor/fees
- 支持国家与支付方式：https://docs.waffo.ai/zh/mor/supported-countries
- 禁止与限制品类：https://docs.waffo.ai/zh/mor/prohibited-products

### 3. 身份、域名与店铺审核

- 账户审核总清单：https://docs.waffo.ai/zh/mor/account-reviews
- Business Details / KYB：https://docs.waffo.ai/zh/settings/business-details
- 个人身份认证 KYC：https://docs.waffo.ai/zh/merchant/identity-kyc
- 支持邮箱验证：https://docs.waffo.ai/zh/settings/email-verification
- 产品域名验证：https://docs.waffo.ai/zh/settings/domain-verification
- 店铺设置：https://docs.waffo.ai/zh/settings/store-settings
- 测试模式说明：https://docs.waffo.ai/zh/features/test-mode

### 4. AIGC 合规与网站法律页

- AIGC 产品审核要求：https://docs.waffo.ai/zh/mor/account-reviews/aigc-compliance
- 服务条款模板与检查清单：https://docs.waffo.ai/zh/mor/account-reviews/tos
- 隐私政策模板与检查清单：https://docs.waffo.ai/zh/mor/account-reviews/privacy-policy
- 可接受使用政策 AUP 模板：https://docs.waffo.ai/zh/mor/account-reviews/aup
- 内容安全接口总览：https://docs.waffo.ai/zh/api-reference/endpoints/content-safety/overview
- Prompt Sift 提示词安全扫描：https://docs.waffo.ai/zh/api-reference/endpoints/content-safety/scan-prompt

### 5. 商品、定价、收银台与订阅

- 商品功能说明：https://docs.waffo.ai/zh/features/products
- AI 产品与 API 计费：https://docs.waffo.ai/zh/guides/ai-billing
- 一次性商品：https://docs.waffo.ai/zh/guides/one-time-payments
- SaaS 订阅：https://docs.waffo.ai/zh/guides/subscriptions
- 订阅功能说明：https://docs.waffo.ai/zh/features/subscriptions
- Checkout 功能说明：https://docs.waffo.ai/zh/features/checkout
- Checkout 完整流程：https://docs.waffo.ai/zh/checkout/checkout-flow
- 自定义收银台：https://docs.waffo.ai/zh/checkout/customization
- 客户自助门户：https://docs.waffo.ai/zh/features/customer-portal

### 6. SDK、API 与 Webhook 接入

- 官方 AI 集成 Skill：https://docs.waffo.ai/zh/integrate/skill
- API 身份认证：https://docs.waffo.ai/zh/api-reference/authentication
- 创建 Checkout Session：https://docs.waffo.ai/zh/api-reference/endpoints/orders/create-checkout-session
- Webhook 配置指南：https://docs.waffo.ai/zh/guides/webhooks
- Webhook 验签与事件结构：https://docs.waffo.ai/zh/api-reference/webhooks
- 取消订阅接口：https://docs.waffo.ai/zh/api-reference/endpoints/subscriptions/cancel-subscription
- 退款处理指南：https://docs.waffo.ai/zh/guides/refunds
- 支付错误码：https://docs.waffo.ai/zh/api-reference/payment-error-codes

### 7. 收入、提款与迁移

- 财务与提款概览：https://docs.waffo.ai/zh/features/finance
- 商户财务账户：https://docs.waffo.ai/zh/merchant/finance
- 设置提款账户：https://docs.waffo.ai/zh/merchant/payout-accounts
- 提款、周期与到账：https://docs.waffo.ai/zh/merchant/payout-flow
- 从 Stripe/Creem 迁移：https://docs.waffo.ai/zh/integrate/migrate

### 8. 给 AI 编程工具使用的完整资料

- 文档目录索引：https://docs.waffo.ai/llms.txt
- 全量 AI 可读文档：https://docs.waffo.ai/llms-full.txt

---

看完这篇 SOP，你觉得你懂了。但 90% 的人一到实操就会卡在第一步。

工具和教程只是最低级的壁垒，真正的壁垒是“立刻动手”和“圈层信息差”。

如果你想看全网最新的变现玩法，并且和一群执行力极强的极客一起下场拿结果，欢迎围观我的核心私域：【懒人专属群】。

我们不聊虚的，只做商业闭环的拆解与 AI 效率的压榨。系统内已沉淀 1000+ 跑通的套利实战局。

- 查阅圈子完整数字资产与上车门槛：https://lazysol.com/insider/

认同价值、准备好下场干活的，扫码加我微信（lazyhelper1）

备注：【实操上车】，不闲聊

![](img/67c37faeb967b1478183a189236a069f_27_0.png)

![](img/67c37faeb967b1478183a189236a069f_27_1.png)

微信：lazyhelper1
