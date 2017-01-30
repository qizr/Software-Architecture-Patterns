#Software Architecture Patterns翻译

最近再看阮一峰的一篇[博客](http://www.ruanyifeng.com/blog/2016/09/software-architecture.html)提到了一本书[《Software Architecture Patterns》](http://www.oreilly.com/programming/free/software-architecture-patterns.csp)（[PDF](http://www.oreilly.com/programming/free/files/software-architecture-patterns.pdf)）
,写的不错,虽然英语很屎,试着翻(gu)译(ge)一下。以下为翻译的内容，祝我能够翻译完，并借此机会能够加深对架构的认识。

![Software Architecture Patterns 封面](http://upload-images.jianshu.io/upload_images/1650675-8bcf0ffe6f6bfc71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ps:建议看英文原著。
ps:[  ]中的话是我说的。

参考:
[阮一峰的博客](http://www.ruanyifeng.com/blog/2016/09/software-architecture.html)
[鸟窝的博客](http://colobu.com/2015/04/08/software-architecture-patterns/)

-----
目录
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [介绍](#%E4%BB%8B%E7%BB%8D)
- [1.分层架构(Layered Architecture)](#1%E5%88%86%E5%B1%82%E6%9E%B6%E6%9E%84layered-architecture)
  - [模式描述(Pattern Description)](#%E6%A8%A1%E5%BC%8F%E6%8F%8F%E8%BF%B0pattern-description)
  - [关键概念(Pattern Description)](#%E5%85%B3%E9%94%AE%E6%A6%82%E5%BF%B5pattern-description)
  - [架构例子(Pattern Example)](#%E6%9E%B6%E6%9E%84%E4%BE%8B%E5%AD%90pattern-example)
  - [注意事项(Considerations)](#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9considerations)
  - [模式分析(Pattern Analysis)](#%E6%A8%A1%E5%BC%8F%E5%88%86%E6%9E%90pattern-analysis)
- [2.事件驱动架构(Event-Driven Architecture)](#2%E4%BA%8B%E4%BB%B6%E9%A9%B1%E5%8A%A8%E6%9E%B6%E6%9E%84event-driven-architecture)
  - [Mediator拓扑(Mediator Topology)](#mediator%E6%8B%93%E6%89%91mediator-topology)
  - [Broker拓扑架构[broker就是代理的意思]](#broker%E6%8B%93%E6%89%91%E6%9E%B6%E6%9E%84broker%E5%B0%B1%E6%98%AF%E4%BB%A3%E7%90%86%E7%9A%84%E6%84%8F%E6%80%9D)
  - [注意事项(Considerations)](#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9considerations-1)
  - [模式分析(Pattern Analysis)](#%E6%A8%A1%E5%BC%8F%E5%88%86%E6%9E%90pattern-analysis-1)
- [3.微内核架构 (Microkernel Architecture)](#3%E5%BE%AE%E5%86%85%E6%A0%B8%E6%9E%B6%E6%9E%84-microkernel-architecture)
  - [模式描述(Pattern Description)](#%E6%A8%A1%E5%BC%8F%E6%8F%8F%E8%BF%B0pattern-description-1)
  - [模式例子(Pattern Examples)](#%E6%A8%A1%E5%BC%8F%E4%BE%8B%E5%AD%90pattern-examples)
  - [结论(Considerations)](#%E7%BB%93%E8%AE%BAconsiderations)
  - [模式分析(Pattern Analysis)](#%E6%A8%A1%E5%BC%8F%E5%88%86%E6%9E%90pattern-analysis-2)
- [4.微服务架构(Microservices Architecture Pattern)](#4%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84microservices-architecture-pattern)
  - [模式拓扑(Pattern Topologies)](#%E6%A8%A1%E5%BC%8F%E6%8B%93%E6%89%91pattern-topologies)
  - [避免依赖和编排(Avoid Dependencies and Orchestration)](#%E9%81%BF%E5%85%8D%E4%BE%9D%E8%B5%96%E5%92%8C%E7%BC%96%E6%8E%92avoid-dependencies-and-orchestration)
  - [注意事项(Considerations)](#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9considerations-2)
  - [模式分析(Pattern Analysis)](#%E6%A8%A1%E5%BC%8F%E5%88%86%E6%9E%90pattern-analysis-3)
- [5.云架构(Space-Based Architecture)](#5%E4%BA%91%E6%9E%B6%E6%9E%84space-based-architecture)
  - [模式描述(Pattern Description)](#%E6%A8%A1%E5%BC%8F%E6%8F%8F%E8%BF%B0pattern-description-2)
  - [Pattern Dynamics](#pattern-dynamics)
  - [注意事项(Considerations)](#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9considerations-3)
  - [模式分析(Pattern Analysis)](#%E6%A8%A1%E5%BC%8F%E5%88%86%E6%9E%90pattern-analysis-4)
- [附录A](#%E9%99%84%E5%BD%95a)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
# 介绍
开发人员在没有合适架构的情况下开始编写程序是非常普遍的情况. 在这种情况下,大多数开发人员和架构师会采用传统分层架构模式（也称为n层架构),通过将源码模块分成各种包来表示抽象层。不幸的是，这种做法经常导致的是一个混乱的源码结构，它们缺乏明确的角色，职责和彼此之间的关系。 这通常被称为反模式的大泥球。

没有通过架构设计的应用程序通常紧密耦合，脆弱，难以改变，没有清晰结构。如果没有完全理解系统中的每个组件和模块的内部工作原理的情况下，确定程序的架构特性会是非常困难的事情。关于程序部署和维护的基本问题也会很难回答：架构是否具有可扩展性？应用程序的性能如何？ 程序是否能够应对变化？程序如何部署？架构的响应能力如何？

架构模式有助于定义应用程序的基本特征和行为。 例如，一些架构模式非常适合需要高可扩展性的应用程序，一些架构模式则适用于非常灵活的应用程序。 只有了解各种架构模式的优势和弱点，才能够选择出满足自己业务特点和目标的架构模式。

作为一个架构师，你必须证明你的架构决策，特别是当涉及到选择一个特定的架构模式或方法。 这篇文正的目的是为您提供足够的信息，以证明你的选择是OK的。
# 1.分层架构(Layered Architecture)
分层架构是最常见的架构，也是javaee应用程序的标准模式，并被多数开发人员了解。这种架构适合于传统IT通信和组织结构，成为大多数业务应用程序开发工作的自然选择。
## 模式描述(Pattern Description)
该架构中每一层都包含多个组件并且拥有具体的职责(例如显示逻辑、业务逻辑)。该架构并不严格规定层的类型和层数，多数分层架构包含视图层、业务层、持久化层和数据库层(图1-1)。有时，业务层和持久层被合并为单个业务层，特别是当持久性逻辑（例如，生成SQL或HSQL）嵌入到业务层组件中。因此，较小的应用可以仅具有三个层，而大的应用可以包含五个或更多个层[对于多数应用四层足以]。
![图1-1 分层架构](http://upload-images.jianshu.io/upload_images/1650675-c9a5a3ac1fcc05d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
分层架构模式的每一层在应用程序中都有特定的角色和职责。例如，表示层将负责处理所有的用户界面逻辑，而业务层负责响应表示层的请求并执行相应的业务逻辑。架构中的每一层都围绕需要完成的工作形成抽象，以满足特定的业务请求。例如，表示层不需要知道或担心如何获得客户数据;它只需要在特定格式的屏幕上显示该信息。类似地，业务层不需要关心如何格式化客户数据以在屏幕上显示或者甚至在客户数据来自哪里;它只需要从持久层获得数据，对数据执行业务逻辑（例如，计算值或聚合数据），并将该信息传递到表示层。

分层架构模式的一个强大的功能是组件之间的关注点分离(separation of concerns)。 特定层中的组件仅处理与该层相关的逻辑。 例如，表示层中的组件只处理表示逻辑，而业务层的组件只处理业务逻辑。 这种组件分类方式使您可以轻松地在您的架构中构建有用的模型，并且由于定义明确的组件接口和有限的组件范围，还可以轻松地使用此架构模式开发，测试，管理和维护应用程序。

## 关键概念(Pattern Description)
在图1-2中，架构中的每个层都标记为封闭。这是分层架构模式中非常重要的概念。 关闭意味着当请求从一个层移动到另一个层时，它必须经过它的下层，才能到达目的层。 例如，一个请求从表示层要到数据库层就必须经过业务层和持久层。

![图1-2 关闭层和请求访问](http://upload-images.jianshu.io/upload_images/1650675-0595dbab9f3cdcb7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么为什么不允许表示层直接访问持久层或数据库层？ 毕竟，从表示层的直接数据库访问比通过一堆不必要的层来检索或保存数据库信息要快得多。 这个问题的答案在于一个被称为隔离层(layers of isolation)的关键概念。

隔离层意味着在一个层的变化通常不会影响到其它层的组件。 如果你允许表示层直接访问持久层，那么持久层的变化将会同时影响业务层和表示层， 从而产生了非常紧密耦合和组件之间的依赖性。当需求发生变化时，这种架构将会付出较大的代价。

隔离概念的层还意味着每个层独立于其他层，并且不需要知道其它层是如何工作的。 为了理解这个概念的重要性，考虑一个大规模的重构，将展示框架从JSP（Java服务器页面）修改为JSF（Java服务器面。假设在表示层和业务层之间使用的契约(contracts)（例如，模型）保持相同，则业务层不受重构的影响并且完全和表示层保持独立。

虽然封闭的层有助于层次之间的隔离，从而提高了架构的低耦合设计，但有时候某些层被打开是有意义的。 例如，假设您要增加一个共享服务层向业务层的组件提供服务（例如，数据和字符串实用程序类或审计和日志记录类）。 在这种情况下创建服务层通常是一个好主意，因为在体系结构上，它将对共享服务的访问限制为业务层（而不是表示层）。 没有单独的层，在架构上没有限制表示层访问这些公共服务，使得难以管理该访问限制[这句话没看懂，回头再看]。

在该示例中，新服务层可能驻留在业务层之下，同时表明该服务层中的组件不能被表示层访问。 然而，这衍生出一个新的问题，业务层需要经过服务层以到达持久层，但这根本没有意义。 这是分层架构的一个古老的问题，需要通过在架构内创建开放层来解决。

如图1-3所示，在这种情况下，服务层被标记为打开，意味着改层可以被绕过。 在以下示例中，由于服务层是打开的，因此业务层现在允许绕过它，并直接转到持久层，这是完全合理的。

![图1-3 打开层和请求流](http://upload-images.jianshu.io/upload_images/1650675-65d0d2cabfbfd94e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
利用开放层和封闭层的概念有助于定义层次和请求流之间的关系，并且向设计者和开发者提供理解架构内的各种层访问限制的必要信息。如果不能确定架构中的哪些层是开放和封闭的（以及为什么）通常会导致架构难以测试，维护和部署的紧密耦合和脆弱。
## 架构例子(Pattern Example)
为了说明分层架构的工作原理，考虑一个来自用户检索客户信息的请求，如图1-4所示。 黑色箭头显示请求流向数据库以检索客户数据，红色箭头显示响应返回到屏幕以显示数据。 在该示例中，客户信息包括客户数据和订单数据（由客户下达的订单)。

![图1-4 分层架构例子](http://upload-images.jianshu.io/upload_images/1650675-c1a90d1c817c7705.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如图所示，customer screen模块负责接受用户请求并显示客户信息，它不知道数据在哪里，如何检索，也不了解数据库，该模块收到查询客户信息的请求后，将该请求转发到customer delegate模块，该模块负责了解业务层中哪些模块可以处理该请求，以及如何获取该模块需要的数据（例如合同数据）。业务层中的custom object负责收集(aggregating)业务请求所需的所有信息（该例子中是指客户信息）。该模块调用customer dao(数据访问对象)模块在持久层中获取客户数据，并且还通过order dao模块获取订单信息。这些模块又执行SQL语句来检索相应的数据，并将其传回给业务层中的customer object模块。一旦customer object接收到数据，它整合数据并将该信息传递回customer delegate，然后将该数据传递到customer screen以呈现给用户。

从技术的角度来看，这些模块可以有很多实现方式。 例如，在Java平台中，customer screen模块可以是(JSF) Java Server Faces screen，它和customer delegage可以作为被管理的bean组件 。业务层中的customer object可以是本地Spring bean对象或远程EJB3 bean对象。 之前示例中的客户和订单数据可以由POJO，[MyBatis](http://www.mybatis.org/mybatis-3/zh/) XML Mapper，甚至可以是JDBC调用或Hibernate查询返回的数据封装。 在Microsoft平台中，customer screen可以是基于.net技术体系ASP（活动服务器页面）模块，客户和订单数据可以由为ADO（ActiveX数据对象）实现。

## 注意事项(Considerations)

分层架构是比较实用并且通用的模式，对于多数程序是一个良好起点，尤其是当您不确定哪种架构模式最适合您的应用程序时。 当选择这种模式时，你需要从架构的角度来考虑一下几点。

要注意的第一件事是所谓的污水池反模式(architecture
sinkhole anti-pattern)[谁能解释一下这是啥意思]。 这个反模式是这样的，请求流简单的穿过几个层，每层里面基本没有做任何业务逻辑，或者做了很少的业务逻辑。 例如，假设表示层响应来自用户的检索客户数据的请求。 表示层将请求传递给业务层，业务层简单地将请求传递给持久层，然后对数据库层进行简单的SQL调用以检索客户数据。 然后，数据一路返回，没有用于聚集(aggregate)，计算或变换数据的额外处理或逻辑。

每个分层架构将具有至少一些落入反模式(architecture
sinkhole anti-pattern)的情况。 这里面的关键是分析出属于此类别的请求的百分比。 80-20规则通常是一个良好的做法，以确定是否正在经历这种情况。通常将大约20％的请求作为简单传递处理，80％的请求具有与请求相关联的某些业务逻辑。 但是，如果您发现此比例相反，并且大多数请求是简单的直通处理，您可能需要考虑使一些架构层打开，需要注意的是这样会让层次的耦合度提高，使软件变得不易改变。

分层体系结构模式需要注意的另一个问题是，它会使得应用程序变得monolithic[这个词不知道怎么翻译合适]，即使你将表示层和业务层分成单独的可部署单元。 虽然某些应用程序不在乎这些问题[规模小的程序]，但它的确会带来部署，通用鲁棒性和可靠性，性能和可扩展性等方面的一些潜在问题。

## 模式分析(Pattern Analysis)
以下内容包含分层架构模式的常见架构特性的评级和分析。每个特性的评级基于适用于该模式的普通场景。 对于此模式与本报告中其他模式如何相关的并行比较，请参见本报告末尾的附录A.

### 灵活性(Overall agility)
* 级别:低
* 分析:灵活性是针对不断变化的环境做出快速反应的能力。 虽然可以通过该模式的层次隔离特点来应对变化，但是由于monolithic性质和这种模式带来的组件间紧耦合的特性，在该体系结构模式中进行改变仍然是繁琐和耗时的。

### 可部署性(Ease of deployment)
* 级别:低
* 分析:对于由该模式实现的大型应用程序来讲，部署可能会是一个问题 。对组件的一个小改变可能需要重新部署整个应用程序（或应用程序的大部分），导致需要加班进行规划，调度和执行的部署。因此，此模式不适合进行可持续部署，进一步降低了部署的总体评级。

### 可测试性(Testability)
* 级别:高
* 分析:因为组件都隶属于某层，而且其他层可以被插桩或者模拟，所以该模式相对容易测试。 开发人员可以通过模拟展示层中的组件中进行业务层中组件的测试，或者模拟业务层以测试某些展示层组件的功能。

### 性能(Performance)
级别:低
分析:虽然一些分层架构可以很好地执行，但是由于必须经历架构的多个层以满足业务请求，效率较低，所以该模式不适用于高性能应用.

### 可扩展性(Scalability)
* 级别:低
* 分析:由于这种模式的紧密耦合和monolithic特点，使用此架构模式的应用程序构建通常难以扩展。 您可以通过将层拆分为单独的物理部署或将整个应用程序复制到多个节点来扩展分层体系结构，但总体来说，粒度太分散，这使其扩展成本昂贵。

### 开发容易度(Ease of development)
* 级别:高
* 分析:易于开发获得相对高的分数，主要是因为这种模式是如此众所周知的，并且实现起来不太复杂。 因为大多数公司通过分层（界面，业务，数据库）分离技能集来开发应用程序，所以这种模式成为大多数业务应用程序开发的自然选择。 公司的沟通和组织结构之间的联系以及开发软件的方式被概述了所谓的 [Conway’s law](https://yq.aliyun.com/articles/8611)。 


# 2.事件驱动架构(Event-Driven Architecture)


事件驱动架构模式是一个构建高扩展性程序的分布式异步架构模式。 它还具有高度适应性，可用于小型应用和大型大型应用或者是复杂的应用。事件驱动架构由异步接收、处理事件的组件构成，这些组件是高度解耦和职责单一的。

事件驱动的架构模式由两种拓扑(topology)方式，即mediator和broker。当你想要在事件内编排一些步骤时需要mediator拓扑[wtf这句话]，而当您想要将事件链接在一起而不使用中央mediator时，你需要broker拓扑。因为这两种拓扑结构的架构特性和实现策略不同，所以了解每一种拓扑结构最适合您的特定情况是非常重要的。

## Mediator拓扑(Mediator Topology)
Mediator拓扑适用于事件需要经过多个步骤处理的情况。 例如，进行股票交易的一个事件可能需要您首先验证交易，然后检查该股票交易是否符合各种合规规则，将交易分配给经纪人，计算佣金，最后完成交易。 所有这些步骤需要进行精心的安排,从而可以顺序的执行.

在Mediator拓扑又四部分组成:事件队列,事件mediator,事件通道和事件处理器.事件流从客户端向事件队列发送事件开始，事件队列用于将事件传输到事件mediator.事件mediator接收事件后,通过向事件通道发送异步事件来执行该事件的每个处理步骤.事件处理器监听事件通,并执行特定的业务逻辑来处理事件。 图2-1说明了事件驱动架构模式的Mediator拓扑.

![图2-1 中介拓扑](http://upload-images.jianshu.io/upload_images/1650675-07d8d2ae770a0213.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在事件驱动架构中，通常有十几到几百个事件队列。 该模式不指定事件队列组件的实现; 它可以是消息队列，web服务端点或者其他什么东西。

该模式中有两种类型的事件：初始事件和处理事件.初始事件是由调解器接收的原始事件，而处理事件是由调解器生成并且由事件处理组件接收的事件。

事件mediator 组件负责协调初始事件中包含的步骤。 对于初始事件中的每一步，事件mediator将特定的处理事件发送到事件通道，然后由事件处理器接收并处理。 重要的是注意事件mediator实际上不执行处理初始事件所必需的业务逻辑; 它只知道处理初始事件所需的步骤。

事件通道被事件mediator异步地将初始事传递给事件处理模块. 事件通道可以是消息队列或消息topics，尽管消息topics和mediator拓扑通常放在一起使用的更多一些，使得处理事件可以由多个事件处理器（每个事件处理器基于接收到的处理事件执行不同的任务）来处理[我感觉这里消息队列和消息topics是两个并列的概念,消息topics是指的是每个事件都能被处理模块得到,有点像pub/sub,而消息队列指的是每个事件只能又一个处理模块得到,有点像pipeline]。

事件处理器组件包含处理处理事件所需的业务逻辑代码。 事件处理器是在应用或系统中执行特定任务的自包含的(self-contained)，独立的，低耦合的架构组件[就是指业务模块]。尽管事件处理器组件的粒度可以从细粒度（例如，计算订单上的销售税）到 粗粒度（例如，处理保险索赔）,重要的是每个事件处理器组件应该执行单个业务任务，而不依赖于其他事件处理器完成其特定任务[业务模块之间不应该产生相互依赖]。

事件mediator可以以多种方式实现. 作为架构师，您应该了解每个细节，以确保该模式的解决方案符合您的需求。

事件mediator的最简单和最常见的实现是通过开源框架，例如[Spring Integration](http://www.infoq.com/cn/articles/Spring-Integration-Joshua-Long)，[Apache Camel]()或[Mule ESB](http://www.infoq.com/cn/articles/http://www.infoq.com/cn/articles/eai-with-apache-camelESB-Integration).这些开源集成中心中的事件流通常通过Java代码或DSL（领域专用语言）实现。 对于更复杂的中介和编排，您可以使用BPEL（domain-specific language）以及BPEL引擎（如开放源代码Apache ODE）。 BPEL是一种标准的类似XML的语言，它描述了处理初始事件所需的数据和步骤。 对于需要更复杂的编排（包括涉及人员交互的步骤）的非常大的应用程序，您可以使用业务流程管理器（BPM）（如jBPM）实现事件仲裁器。

理解您的需求并选择合适的事件mediator实现，对使用此拓扑的任何事件驱动成功至关重要。 使用开源集成中心来执行非常复杂的业务流程管理编排是不正确的，就像实施一个BPM解决方案来执行简单的路由逻辑一样[宰牛不能用杀鸡刀,杀鸡不能用宰牛刀]。

为了说明mediator拓扑是如何工作的，假设你通过保险公司投保，并决定修改。 在这种情况下，初始事件可能被称为重定位事件(relocation
event)。 处理重定位事件涉及的步骤包含在事件mediator中，如图2-2所示。 对于每个初始事件步骤，事件中介器创建处理事件（例如，改变地址，重新计算报价等），将该处理事件发送到事件信道，并等待由相应的事件处理器处理的处理事件（例如 ，客户过程，报价过程等）。 该过程继续，直到初始事件中的所有步骤都已被处理。 在Event Mediator中,Recalc Quate和Update Claims上面的箭头代表这两个步骤可同时进行。
![图2-2 中介模式例子](http://upload-images.jianshu.io/upload_images/1650675-f5f5ad7a9ead1903.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Broker拓扑架构[broker就是代理的意思]
Broker拓扑不同于mediator拓扑，因为没有中心事件mediator,消息通过轻量级消息代理（例如，ActiveMQ，HornetQ等)[Rabbitmq,zeromq]发布给业务组件。 当您具有相对简单的事件处理流，并且您不需要中心mediator去安排更上层的业务流程，此拓扑非常有用[适合多数业务不太复杂的系统]。

该拓扑中有两种主要类型的架构组件：代理组件和事件处理器组件。 代理组件可以是一个或者好几个(centralized or federated)，并且包含了所有的事件通道,这些通道涵盖在事件流内。代理组件中包含的事件通道可以是消息队列，消息主题发布或两者的组合。

此拓扑如图2-3所示。 从图中可以看出，没有event-mediator组件来控制和编排初始事件; 相反，每个事件处理器组件负责处理事件并发布事件处理结果。 例如，平衡股票投资组合的事件处理器可以接收称为股票分割的初始事件。 基于该初始事件，事件处理器可以进行一些组合重新平衡，然后向代理发布称为重新平衡组合的新事件，其然后将由不同的事件处理器拾取。 注意，可能存在事件由事件处理器发布但是没有别的处理器接受的情况出现。这是常见的,尤其是当您正在开发应用程序或提供未来的功能和扩展。
![图2-3 代理拓扑](http://upload-images.jianshu.io/upload_images/1650675-57ce89cb464b00de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
为了说明代理拓扑如何工作，我们将使用与mediator拓扑中相同的示例。由于没有中央事件mediator来接收代理拓扑中的初始事件，客户过程组件直接接收事件，改变客户地址，并且发出该地址改变事件。在此示例中,有两个对更改地址事件感兴趣的事件处理器：报价过程和声明过程。报价处理器组件根据地址更改重新计算新的自动保险率，并向系统的其余部分发布事件指示它做的事情（例如，重新计算报价事件）。另一方面，声明处理组件接收相同的改变地址事件，但是在这种情况下，它更新未完成的保险声明并且向系统发布事件作为更新声明事件。这些新事件随后被其他事件处理器组件拾取，并且事件链继续通过系统，直到不再有针对该特定启动事件发布的事件.[这不就是pr系统的设计嘛]
![图2-4 代理拓扑例子](http://upload-images.jianshu.io/upload_images/1650675-f45556074dd510e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
从图2-4中可以看出，代理拓扑结构都是关于执行业务功能的事件总线。 理解代理拓扑的最好方法是将其视为中继竞赛。在接力赛中，跑步者持有接力棒并运行一定距离，然后将指挥棒交给下一个跑步者，依此类推 直到最后一个运动员穿过终点线。 在接力赛中，一旦运动员交出指挥棒，她就完成了比赛。 这对于代理拓扑也是如此：一旦事件处理器切换事件，它不再参与该特定事件的处理。
## 注意事项(Considerations)
事件驱动的架构模式是一个相对复杂的模式实现，主要是由于它的异步的特点。当实现这种模式时，您必须解决各种分布式架构问题，如远程进程可用性，缺乏响应性和代理重新连接逻辑 事件代理或mediator失败。

这种架构模式适用于缺少原子性事务的业务流程。 因为事件处理器组件是高度去耦合和分布式的，所以很难跨越多个业务组件的同时去维护业务的事务特点[ACID]。 因此，在使用此模式设计应用程序时，必须不断考虑哪些事件可以独立运行或者不单独运行，并相应地计划事件处理器的粒度。 如果您发现由多个事件处理器构成一个工作单元，换句话说,如果您使用分散的处理器来处理不可分割的事务，在这种情况下,该模式可能不适合您的应用程序。

事件驱动的架构模式的最困难的方面之一可能是事件处理器组件之间契约(contracts)的创建，维护和管理。 每个事件通常具有与它相关联的契约形式（例如，数据值和数据格式).使用此模式来建立标准数据格式（例如，XML，JSON，Java对象等）并从一开始就建立契约的版本控制策略至关重要[业务模块之间的接口很重要,也非常容易发生变化!!!]。

## 模式分析(Pattern Analysis)

该章节包含事件驱动架构模式的常见架构特性的评级和分析。 每个特性的评级基于自然趋势或该特性作为基于图案的典型实施的能力，以及通常已知的图案。 对于此模式与本报告中其他模式如何相关的并行比较，请参见本报告末尾的附录A.

### 灵活性(Overall agility)
* 级别:高
* 分析:灵活性是指软件应对变化的能力。 由于事件处理器组件是职责单一的的并且与其他事件处理器组件完全解耦，因此改变通常被隔离到一个或几个事件处理器内，并且可以快速地做出改变而不影响其他组件.

### 易部署性(Ease of deployment)
* 级别:高
* 分析:总得来说，由于事件处理器组件的去耦特性，这种模式相对容易部署。 代理拓扑趋向于比中介器拓扑更易于部署，主要是因为事件中介器组件和事件处理组件是紧耦合的：事件处理器组件中的更改也可能需要事件介体进行更改，两者都要进行重新部署.

### 可测试性(Testability)
* 级别:低
* 分析:虽然单个单元测试不是特别困难，但它需要某种专门的测试客户端或测试工具来生成事件。 这种模式的异步性质也使测试复杂化。

### 性能(Performance)
* 级别:高
* 分析:虽然该架构的效率很大程度上取决于消息发送基础结构，但是通常来说，该模式通过其异步能力实现高性能; 换句话说，执行解耦，并行异步操作的能力胜过入队和出队消息的成本。

### 可扩展性(Scalability)
* 级别:高
* 分析:由于拥有独立和低耦合的事件处理器,该架构天生的具有高可扩展性。 每个事件处理器可以单独缩放，允许细粒度可伸缩性。

### 开发容易度(Ease of development)
* 级别:低
* 分析:由于该模式异步的特性,需要建立契约,以及需要考虑针对代理错误以及异常事件的处理,该架构不同意上手.

# 3.微内核架构 (Microkernel Architecture)
微内核架构模式（有时称为插件架构模式）是实现基于产品的应用程序所使用的模式。 基于产品的应用程序是作为典型的第三方产品被打包并提供下载的应用程序。 然而，许多公司也开发和发布其内部业务应用程序，如软件产品，完整版本，发行说明和可插拔功能。 这些也是这种模式的典型应用。 微内核架构模式允许您将附加应用程序功能作为插件添加到核心应用程序，提供可扩展性以及功能分离和隔离。
## 模式描述(Pattern Description)
微内核架构模式由两种类型的架构组件组成：核心系统和插件模块。业务逻辑分开在独立插件模块和基本核心系统之间，这种方式提供功能和业务逻辑的可扩展性，灵活性和隔离的特点。 图3-1说明了基本微内核架构模式.

![图3-1 微核架构设计](http://upload-images.jianshu.io/upload_images/1650675-b535875e9e2d483b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

微内核架构模式的核心系统仅包含使系统运行所需的最小功能集.许多操作系统实现了微内核架构模式，因此是该模式名称的起源。从业务程序的角度来看，核心系统通常被定义为通用业务逻辑，不包含针对特殊情况或者复杂情况的自定义代码.

插件模块是独立的独立组件，包含特殊的处理，附加的功能和自定义代码，用于增强或扩展核心系统以产生额外的业务功能。 通常,插件模块应该独立于其他插件模块，但是您当然可以设计需要其他插件的插件。 无论哪种方式，重要的是保持插件之间的通信最小化，以避免依赖性问题。

核心系统需要知道哪些插件模块可用以及如何获取它们。 一种常见的实现方式是通过某种插件注册表。 此注册表包含有关每个插件模块的信息,包括其名称,数据结构和远程访问协议详细信息（取决于插件如何连接到核心系统). 例如,用于标记高风险税务审计项目的税务软件的插件可能具有包含服务名称(AuditChecker)，数据结构（输入数据和输出数据）和合同格式的注册表项 (XML）.如果插件通过SOAP访问，它还可能包含WSDL（Web服务定义语言）.

插件模块可以通过各种方式连接到核心系统,包括OSGi（开放服务网关倡议），消息，web服务或甚至直接点对点绑定（即对象实例化）。 您使用的连接类型取决于您正在构建的应用程序类型（小型产品或大型业务应用程序）和您的特定需求（例如，单一部署或分布式部署）。 架构模式本身不指定任何这些实现细节，只有插件模块必须保持彼此独立。

插件模块可以通过各种方式连接到核心系统,包括OSGi（开放服务网关倡议），消息，web服务或甚至直接点对点绑定（即对象实例化）. 您使用的连接类型取决于您正在构建的应用程序类型（小型产品或大型业务应用程序）和您的特定需求（例如，单一部署或分布式部署）.架构模式本身不指定任何这些实现细节，只有插件模块必须保持彼此独立。

插件模块和核心系统之间的契约范围可以从标准契约到自定义契约. 自定义契约通常在插件组件由第三方开发的情况下建立，其中您无法控制插件使用的契约。 在这种情况下,在插件契约和标准契约之间插入一个适配器，以便核心系统不需要每个插件都有专门的代码。 在创建标准契约(通常通过XML或Java Map实现)时，务必记住从开始就创建版本控制策略。

## 模式例子(Pattern Examples)
内核架构的最好的例子是Eclipse IDE.下载基本的Eclipse产品只是一个漂亮的编辑器。 然而,一旦你开始添加插件，它就成为一个高度可定制和有用的产品。互联网浏览器是另一个常见的产品示例使用微内核架构：查看器和其他插件添加额外的功能，在基本的浏览器 即核心系统）。

类似这种基于产品软件的例子是挺多的，但是大型企业应用程序呢？ 微内核架构也适用于这些情况。 为了说明这一点，让我们使用另一个保险公司的例子，但这次涉及保险索赔处理。

索赔处理是一个非常复杂的过程。 每个地区对保险索赔中的内容都有不同的规则和的规定。 例如，当您的挡风玻璃被岩石损坏,一些地区允许更换挡风玻璃，而其他地区则不会。 这为标准索赔创建了一个很大的条件集合。

毫不奇怪，大多数保险索赔应用程序利用大型和复杂的规则引擎来处理这种复杂性。 然而，这些规则引擎可以成长为一个复杂的大泥球，其中改变一个规则影响其它规则，或者做一个简单的规则改变就需要一支分析师，开发人员和测试人员的队伍。 使用微内核架构模式可以解决许多这些问题。

图3-2中显示的文件夹堆栈表示声明处理的核心系统。 它包含保险公司处理索赔所需的基本业务逻辑,不包含任何定制处理。 每个插件模块包含该地区的特定规则。 在该示例中，可以使用定制源代码或单独的规则引擎实例来实现插件模块。 不管实现如何，关键点是地区特定的规则和处理与核心声明系统分离，并且可以进行添加,删除和修改,并且对核心系统或其他插件模块几乎没有影响.
![图3-2 微核架构例子](http://upload-images.jianshu.io/upload_images/1650675-71f8b6e0002d2246.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 结论(Considerations)
微内核架构模式的一个好处是它可以嵌入或用作另一个架构模式的一部分。例如，如果此模式解决了应用程序的特定易失性区域中存在的特定问题，您可能会发现不能使用这种模式实现整个架构。 在这种情况下，您可以将微服务架构模式嵌入您正在使用的另一个模式（例如，分层架构）.类似地,可以使用微服务架构模式来实现在先前关于事件驱动架构的部分中描述的事件处理器组件.

微服务架构模式为进化设计和增量开发提供了极大的支持.您可以首先生成一个坚实的核心系统，随着应用程序的不断发展，添加功能和功能，而不必对核心系统进行重大更改。

对于基于产品的应用程序，微内核架构模式应始终是您的首选架构，特别是对于那些将随着时间的推移发布其他功能，并希望控制不同用户获得不同的功能。 如果您发现该模式不能满足您的所有需求，您可以随时将您的应用程序重构为更适合您的特定需求的另一个架构模式。

## 模式分析(Pattern Analysis)

该章节包含事件驱动架构模式的常见架构特性的评级和分析。 每个特性的评级基于自然趋势或该特性作为基于图案的典型实施的能力，以及通常已知的图案。 对于此模式与本报告中其他模式如何相关的并行比较，请参见本报告末尾的附录A.
### 灵活性(Overall agility)
* 级别:高
* 分析:总体灵活性是对不断变化的环境做出快速反应的能力.通过松散耦合的插件模块.可以在很大程度上隔离和实现更改。 一般来说，大多数微内核架构的核心系统趋于快速稳定，因此相当稳健，并且很少需要随时间变化。

### 可部署性(Ease of deployment)
* 级别:高
* 分析:根据如何实现模式，插件模块可以在运行时动态地添加到核心系统（例如，热部署），从而最小化部署期间的停机时间。

### 可测试性(Testability)
* 级别:高
* 分析:插件模块可以进行隔离测试，并且可以很容易地被核心系统模仿以论证或原型化特定特征，而对核心系统的改变很少或没有改变。

### 性能(Performance)
* 级别:高
* 分析:虽然微内核模式不适合高性能应用程序，但一般来说，使用微内核架构模式构建的大多数应用程序性能良好，因为您可以自定义和简化应用程序，仅包含您需要的功能. [JBoss](http://baike.baidu.com/item/Jboss)应用服务器就是一个很好的例子:使用它的插件架构，你可以将应用服务器修剪到你需要的那些特性，消除昂贵的未使用的功能，例如远程访问，消息传递和消耗内存的缓存 ，CPU和线程等并减慢应用程序服务器.

### 可扩展性(Scalability)
* 级别:低
* 分析:因为大多数微内核架构实现是基于产品的，并且通常规模较小，所以它们被实现为单个单元，因此不是高度可扩展的。根据如何实现插件模块，有时可以在插件功能级别提供可扩展性 ，但总的来说，这种模式对于生产高度可扩展的应用程序比较少。

### 开发容易度(Ease of development)
* 级别:低
* 分析:微内核架构需要周密的设计和合同管理,使其实现起来相当复杂.契约版本控制，内部插件注册表，插件粒度以及可用于插件连接的广泛选择都有助于实现此模式所涉及的复杂性。

# 4.微服务架构(Microservices Architecture Pattern)
微服务架构模式作为单体应用程序和面向服务的架构的可行替代方案，正在业界迅速获得成功。 因为这种架构模式一直在发展,所以业界对这个模式是什么以及如何实现有很多困惑.本章节将向您提供了解这种重要架构模式的优点（和权衡）所必需的关键概念和基础知识，以及它是否适合您的应用程序。
 
无论您选择的拓扑结构或实现风格如何，都有一些常见的核心概念适用于一般的架构模式。 首先是单独部署单元的概念。 如图4-1所示，微服务架构的每个组件都作为一个单独的单元部署，允许通过有效和简化的交付管道更容易地部署，提高可扩展性，以及在应用程序中实现高度的应用程序和组件解耦。



从单体应用到微服务架构风格的演进路径主要是通过开发持续交付,从开发到生产的连续部署的概念，这简化了应用程序的部署.单体应用通常由多个紧密耦合的组件构成,这些组件是部署单元的一部分,这使得改变,测试和部署应用变得麻烦和困难（因此，大多数大型IT商店中常见的“每月部署”周期的兴起 ). 这些因素通常导致脆弱的应用程序，每次部署新的东西时容易被破坏.微服务架构模式通过将应用程序分为多个可部署单元（服务组件）来解决这些问题，这些可单独开发，测试和部署，而与其他服务组件无关。

![图4-1 基本微服务架构](http://upload-images.jianshu.io/upload_images/1650675-86741baec415e432.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

理解这个模式最重要的概念是服务组件的概念。 相比考虑微服务架构中的服务，最好还是考虑服务组件，服务组件从单个模块到大块应用的粒度可能有所不同。 服务组件包含表示单一用途功能（例如，为特定城市或城镇提供天气）或大型商业应用的独立部分（例如，股票交易放置或 确定汽车保险费率）。 设计正确级别的服务组件粒度是微服务架构中最大的挑战之一。 此挑战在以下服务组件编排子部分中有更详细的讨论。

微服务架构模式内的另一个关键概念是其是分布式架构，意味着架构内的所有组件彼此完全解耦并且通过某种远程访问协议（例如JMS，AMQP，REST，SOAP， RMI等）。 这种架构模式的分布式特性有助于实现其一些卓越的可扩展性和部署特性。

微服务架构的一个吸引人的地方在于它是从与其他通用架构模式相关的问题演变而成,而不是作为等待问题发生的解决方案来创建.微服务架构风格自然地从两个主要来源演变而来:使用分层架构模式开发的单体应用程序和通过面向服务的架构模式开发的分布式应用程序.

从单体应用到微服务架构风格的演进路径主要是通过开发持续交付，从开发到生产的连续部署管道的概念，这简化了应用程序的部署。 单片应用通常包含紧密耦合的组件,使得改变包含改变,测试和部署应用变得麻烦和困难（因此,大多数大型IT商店中常见的“每月部署”周期的兴起 ）。 这些因素通常导致脆弱的应用程序，每次部署新的东西时都会产生风险。 微服务架构模式通过将应用程序分为多个可部署单元（服务组件）来解决这些问题，这些可单独开发，测试和部署，而与其他服务组件无关。

导致微服务体系结构模式的另一个来源是来自实现面向服务的体系结构模式（SOA）的应用程序出现的问题.虽然SOA模式非常强大,提供了强大的抽象级别,异构连接,服务编排,以及将业务目标与IT功能对齐的承诺,但是它是复杂，昂贵，分散的，难以理解和实施的，而且对于大多数应用是过度的. 微服务架构风格通过简化服务概念，消除编排需求以及简化连接和访问服务组件来解决这种复杂性。

## 模式拓扑(Pattern Topologies)
虽然有几十种方法来实现微服务架构模式，但其中三个主要拓扑结构是最常见和最受欢迎的：基于API REST的拓扑(API REST-based),基于应用程序REST的拓扑(API REST-based)和集中式消息传递拓扑(centralized messaging)。

基于API REST的拓扑通过一些API（应用程序编程接口）暴露的较少，独立的单独服务,这对于一些网站是有用的。 此拓扑（如图4-2所示）由非常细粒度的服务组件（因此称为微服务）组成，其中包含一个或两个模块，这些模块独立于其他服务执行特定的业务功能。 在这种拓扑中，这些细粒度的服务组件通常使用通过单独部署的基于web的API层实现的基于REST的接口来访问。该拓扑的示例包括由Yahoo,Google和Amazon建立的单一职责的RESTful web服务.

![图4-2 API REST结构](http://upload-images.jianshu.io/upload_images/1650675-f331cb632b122477.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

应用程序基于REST的拓扑与基于API REST的方法不同的,它是通过传统的基于Web或胖客户端的业务应用程序界面接收客户端请求,而不是通过简单的API层. 如图4-3所示，应用程序的用户界面层被部署为单独的Web应用程序，通过简单的基于REST的接口远程访问单独部署的服务组件（业务功能）。 此拓扑中的服务组件与基于API-REST的拓扑中的服务组件不同，这些服务组件往往更大，更粗略，并且代表整体业务应用程序的一小部分，而不是细粒度的单一服务 。 这种拓扑对于具有相对较低复杂度的小到中等规模的业务应用是常见的。

![图4-3 Application REST拓扑](http://upload-images.jianshu.io/upload_images/1650675-bb939f8f3332b6f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

微服务架构模式中的另一种常见方法是集中式消息传递拓扑。 此拓扑（如图4-4所示）类似于之前基于REST的应用程序拓扑，除了不使用REST进行远程访问，此拓扑使用轻量级集中式消息代理（例如ActiveMQ，HornetQ等).当查看此拓扑时，不要将其与面向服务的架构模式混淆或将其视为“SOA-Lite”，这是至关重要的.在此拓扑中找到的轻量级消息代理不会执行任何编排，转换或复杂路由,它只是一个轻量级的传输来访问远程服务组件.

![图4-4 集中消息拓扑](http://upload-images.jianshu.io/upload_images/1650675-2577650ed9bded16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

集中式消息拓扑适用于在需要对用户接口和服务组件之间的传输层进行更复杂控制的更大的业务应用或应用.与先前讨论的简单的基于REST的拓扑相比，此拓扑的优点是高级排队机制，异步消息传递，监视，错误处理以及更好的总体负载平衡和可扩展性。 通常通过代理群集和代理联合（将单个代理实例分成多个代理实例，以根据系统的功能区域划分消息吞吐量负载）来解决通常与集中式代理相关联的单点故障和架构瓶颈问题。

## 避免依赖和编排(Avoid Dependencies and Orchestration)

微服务架构模式的主要挑战之一是为服务组件确定正确的粒度级别。 如果服务组件过粗，您可能无法实现这种架构模式（部署，可扩展性，可测试性和松耦合）带来的好处。 然而，过于细粒度的服务组件将导致服务编排的需求，这会将精简的微服务架构转变为重量级的面向服务的架构，并且会包含在SOA所特有的复杂性，混乱，昂贵和瑕疵.

如果您发现需要从应用程序的用户界面层或API层中编排您的服务组件，那么您的服务组件可能太细粒度。 同样，如果您发现需要在服务组件之间执行服务间通信来处理单个请求，则您的服务组件可能过于细粒度，或者从业务功能的角度来看，它们没有正确分区.

可以通过共享数据库来处理可能强制组件之间不期望的耦合的服务间通信。 例如，如果处理因特网订单的服务组件需要客户信息，则它可以去到数据库以检索必要的数据，而不是调用客户 - 服务组件内的功能。

共享数据库可以提供所需的信息，但是共享功能怎么办？ 如果服务组件需要包含在另一个服务组件中或所有服务组件都共用的功能，您有时可以跨服务组件复制共享功能（从而违反DRY原则：不要重复自己).在大多数实现微服务架构模式的业务应用程序中，这是相当常见的做法,为了保持服务组件独立和分离其部署，需要将业务逻辑小部分的冗余.小型实用程序类可能属于此类重复的代码。

如果您发现无论服务组件粒度级别如何，您仍然无法避免服务组件编排，那么这是一个好的迹象，这可能不是您的应用程序的正确的架构模式.由于此模式的分布式特性,很难在服务组件之间（和之间）维护单个事务性工作单元. 这种做法将需要某种用于回滚事务的事务补偿框架，这为这种相对简单和优雅的架构模式增加了显着的复杂性[事物不能跨组件来完成]。

## 注意事项(Considerations)
微服务架构模式解决了在单片应用程序以及面向服务的架构中发现的许多常见问题.由于主要应用程序组件被拆分成更小的,单独部署的单元，使用微服务架构模式构建的应用程序通常更健壮，提供更好的可扩展性，并且可以更容易地支持连续传递。

这种模式的另一个优点是它提供了进行实时生产部署的能力，从而显着减少对传统的每月或周末“大爆炸”生产部署的需求。 由于更改通常与特定服务组件隔离，因此只需要部署需要更改的服务组件。 如果您只有一个服务组件的一个实例，您可以在用户界面应用程序中编写专门的代码来检测活动的热部署，并将用户重定向到错误页面或等待页面。 或者，您可以在实时部署期间交换服务组件的多个实例，从而在部署周期内实现连续可用性（这对于分层架构模式非常困难）。

要考虑的最后一个考虑因素是，由于微服务架构模式是分布式架构，它分享了在事件驱动架构模式中发现的一些相同的复杂问题，包括契约创建，维护和治理，远程系统可用性， 远程访问认证和授权。




## 模式分析(Pattern Analysis)

该章节包含常见架构特性的评级和分析。 每个特性的评级基于自然趋势或该特性作为基于图案的典型实施的能力，以及通常已知的图案。 对于此模式与本报告中其他模式如何相关的并行比较，请参见本报告末尾的附录A.
### 灵活性(Overall agility)
* 级别:高
* 分析:总体灵活性是对不断变化的环境做出快速反应的能力.由于单独部署的单元这个特点，改变通常被隔离到单独的服务组件，这允许快速和容易的部署。此外，使用这种模式的应用建立趋于非常松散耦合，这也有助于促进改变。

### 可部署性(Ease of deployment)
* 级别:高
* 分析:总体上，由于事件处理器组件的去耦特性，这种模式相对容易部署.代理拓扑趋向于比中介器拓扑更容易部署,主要是因为事件中介器组件在某种程度上紧密耦合到事件处理器：事件处理器组件的变化也可能需要事件中介器的改变， 被部署为任何给定的变化。

### 可测试性(Testability)
* 级别:高
* 分析:由于将业务功能分离和隔离为独立应用程序，因此可以对测试进行限定，从而实现更有针对性的测试工作。 特定服务组件的回归测试比整个单片应用程序的回归测试容易得多且更可行。此外，由于此模式中的服务组件松散耦合，因此从进行变更的开发角度看的很少能够影响应用程序的另一部分，减轻了测试整个应用程序的一个小的变化的测试负担。

### 性能(Performance)
* 级别:低
* 分析:虽然您可以创此模式实现的应用程序执行非常好，但由于微服务架构模式的分布式特性，这种模式本身并不适合高性能应用程序。

### 可扩展性(Scalability)
* 级别:高
* 分析:由于应用程序分为单独部署的单元,每个服务组件可以单独缩放,允许对应用程序进行微调缩放. 例如,由于该功能的低用户量，股票交易应用的管理区域可能不需要缩放，但是交易布置组件需要包含缩放功能,由于大多数交易应用为此所需的高吞吐量.

### 开发容易度(Ease of development)
* 级别:高
* 分析:由于功能被隔离到单独的和不同的服务组件中,由于较小和隔离的范围,开发变得更容易. 开发人员对一个服务组件进行更改会影响其他服务组件的机会少得多，从而减少了开发人员或开发团队之间的协调。

# 5.云架构(Space-Based Architecture)

大多数基于Web的业务应用程序都遵循相同的一般请求流：一个来自浏览器的请求发送至Web服务器,然后是应用程序服务,最后是数据库服务.虽然此模式对于一小部分用户非常有用，但是随着用户负载的增加，瓶颈开始首先出现在Web服务器层，然后在应用程序服务器层，最后在数据库服务器层。基于用户负载增加的瓶颈的应对方案通常是是向外扩展web服务器.这是相对容易和便宜的,通常能顺利解决问题.但是,在大多数用户负载较高的情况下，扩展Web服务器层只是将瓶颈移动到应用程序服务器.缩放应用程序服务器可能比Web服务器更复杂和昂贵，通常只是将瓶颈移动到数据库服务器，这是更加困难和昂贵的扩展。即使你可以扩展数据库，你最终得到的是一个三角形拓扑(triangleshaped)，三角形的最宽部分是Web服务器（最容易扩展），最小的部分是数据库（最难以扩展).

在任何具有极大并发用户负载的大容量应用程序中，数据库能够同时处理多少事务是最终限制因素.虽然各种缓存技术和数据库扩展产品有助于解决这些问题,但事实仍然是,扩展正常应用程序解决极端负载问题是一个非常困难的命题.

基于云的架构模式专门设计用于处理和解决可扩展性和并发性问题.对于具有可变和不可预测的并发用户卷的应用程序,它也是一种有用的体系结构模式.在架构上解决极端和可变的可伸缩性问题通常是比将数据库扩展或将缓存技术改进更好。

## 模式描述(Pattern Description)

基于空间的模式（有时也称为云架构模式）将限制应用程序缩放的因素将至最低. 这个模式顾名思义,来源于从元组空间( tuple space)的概念和分布式共享内存的概念得。 通过去掉中央数据库约束并使用复制的内存数据网格实现高可扩展性。 应用数据保存在内存中并在所有活动处理单元之间复制.当用户负载增加和减少时，处理单元可以动态地启动和关闭,从而解决可变的可扩展性。 因为没有中央数据库,所以去掉了数据库瓶颈,在应用程序中提供了近乎无限的可扩展性.

大多数符合此模式的应用程序是从浏览器接收请求并执行某种操作的标准网站. 招标拍卖网站就是一个很好的例子. 该网站通过浏览器请求不断接收互联网用户的出价. 应用程序将接收对特定项目的出价，记录具有时间戳的出价，并且更新项目的最新出价信息，并将信息发送回浏览器。

此架构模式中有两个主要组件：处理单元(processing unit)和虚拟中间件(virtualized middleware). 图5-1说明了基本的基于空间的架构模式及其主要架构组件。

![图5-1 云架构](http://upload-images.jianshu.io/upload_images/1650675-e09b9af4170ea09a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

处理单元组件包含应用组件（或应用组件的部分). 这包括基于web的组件以及后端业务逻辑.处理单元的内容基于应用的类型而变化 - 较小的基于web的应用可能被部署到单个处理单元中，而较大的应用可基于应用的功能区域将应用功能分割成多个处理单元。 处理单元通常包含应用程序模块，以及内存中数据网格和可选的异步持久存储器,防止异常丢失数据. 它还包含复制引擎，虚拟化中间件使用复制引擎将一个处理单元所做的数据更改复制到其他活动处理单元。

虚拟化中间件组件处理管理和通信.它包含控制数据同步和请求处理的各个方面的组件.虚拟化中间件中包含消息传递网格,数据网格,处理网格和部署管理器. 这些组件在下一节中有详细描述，可以定制或作为第三方产品购买。

## Pattern Dynamics

基于空间的架构模式的魔力在于虚拟化的中间件组件和包含在每个处理单元内的内存数据网格.图5-2显示了典型的处理单元架构,包含应用程序模块,内存数据网格,用于故障转移的可选异步持久存储以及数据复制引擎.

![图5-2 处理单元](http://upload-images.jianshu.io/upload_images/1650675-c503ea004edbd114.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

虚拟化中间件本质上是架构的控制器，并且管理请求,会话,数据复制,分布式请求处理和过程单元部署.在虚拟化中间件中有四个主要的架构组件:消息传递网格,数据网格,处理网格和部署管理器.

### 消息网格(Messaging Grid)
消息网格（如图5-3所示）管理输入请求和会话信息。 当一个请求进入虚拟化中间件组件时,消息传递网格组件确定哪些活动处理组件可用于接收请求,并将请求转发到这些处理单元之一.消息网格的复杂性可以从简单的循环算法到更复杂的 临近可用算法(next-available algorithm)，保持跟踪由哪个处理单元处理哪个请求。



![图5-3 消息网格组件](http://upload-images.jianshu.io/upload_images/1650675-679f8867ebe2387a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 数据网络(Data Grid)
数据网格组件可能是这种模式中最重要和最重要的组件. 数据网格利用每个处理单元中的数据复制引擎,当数据更新发生时,管理单元之间的数据复制. 由于消息传送网格可以向任何可用的处理单元转发请求,因此每个处理单元在其内存数据网格中包含完全相同的数据是必要的. 虽然图5-4显示了处理单元之间的同步数据复制,但实际上这是以异步和非常快速的并行方式完成的,有时在几微秒（百万分之一秒）内完成数据同步.

![图5-4 数据网络组件](http://upload-images.jianshu.io/upload_images/1650675-af527cdd6442d173.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 处理网格(Processing Grid)
如图5-5所示，处理网格是虚拟化中间件中的一个可选组件，当存在多个处理单元（每个单元处理应用程序的一部分）时管理分布式请求处理.如果进入需要处理单元类型（例如,订单处理单元和客户处理单元）之间的协调的请求，则是在这两个处理单元之间中介和协调请求的处理网格。

![图5-5 处理网络组件](http://upload-images.jianshu.io/upload_images/1650675-372fd0bcb5c413af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 部署管理(Deployment Manager)
部署管理器组件根据负载条件管理处理单元的动态启动和关闭.该组件持续监视响应时间和用户负载,并在负载增加时启动新的处理单元,并在负载减少时关闭处理单元.它是在应用程序中实现可变可扩展性需求的关键组件.

## 注意事项(Considerations)
基于云架构模式是一个实现复杂和昂贵的模式. 对于具有可变负载且规模较小的基于网络的应用（例如，社交媒体网站，竞价和拍卖网站）,这是一种良好的架构选择。 然而，它不太适合具有大量操作数据的传统大规模关系数据库应用程序。

尽管基于空间的架构模式不需要集中式数据存储，但是需要用于执行初始内存数据网格加载和异步保持由处理单元进行的数据更新。 常见的做法是创建将易失性和广泛使用的事务数据与非活动数据隔离的单独分区，以便减少每个处理单元内的内存数据网格的存储器占用。

需要着重注意的是，虽然这种模式的替代名称是基于云的架构，但是处理单元（以及虚拟化中间件）不一定要驻留在基于云的托管服务或PaaS（平台即服务）上， 它可以很容易驻留在本地服务器上，这是我更喜欢名为“基于空间的架构”的原因之一。

从产品实现的角度来看,您可以通过第三方产品（如GemFire，JavaSpaces，GigaSpaces，IBM Object Grid，nCache和Oracle Coherence)实现此模式中的许多架构组件.由于此模式的实施在成本和功能(特别是数据复制时间）方面有很大差异，因此作为架构师，您应首先确定具体目标和需求，然后再进行任何产品选择.

## 模式分析(Pattern Analysis)

该章节包含事件驱动架构模式的常见架构特性的评级和分析。 每个特性的评级基于自然趋势或该特性作为基于图案的典型实施的能力，以及通常已知的图案。 对于此模式与本报告中其他模式如何相关的并行比较，请参见本报告末尾的附录A.

### 灵活性(Overall agility)
* 级别:高
* 分析:总体灵活性是对不断变化的环境做出快速反应的能力.因为处理单元（应用的所部署的实例)可以快速进行调整,所以应用对于与用户负载（环境变化）的变化能够很好的地响应.使用该模式创建的结构通常对由于应用程序的大小和模式的动态特性。

### 可部署性(Ease of deployment)
* 级别:高
* 分析:虽然基于空间的架构通常不是解耦和分布式的，但它们是动态的，复杂的基于云的工具允许应用程序轻松地“推送”到服务器，简化部署。

### 可测试性(Testability)
* 级别:低
* 分析:在测试环境中实现非常高的用户负载既昂贵又耗时，使得难以测试应用的可扩展性方面。

### 性能(Performance)
* 级别:高
* 分析:通过内存数据访问和缓存机制构建到此模式中来实现高性能。

### 可扩展性(Scalability)
* 级别:高
* 分析:高可扩展性来自于对集中式数据库的依赖很少或没有依赖性，因此基本上从可扩展性方程中去除了这个限制性瓶颈。

### 开发容易度(Ease of development)
* 级别:低
* 分析:复杂的缓存和内存数据网格产品使得这种模式开发起来相对复杂,主要是因为缺乏对用于创建这种类型的架构的工具和产品的熟悉.此外,在开发这些类型的体系结构时必须特别小心，以确保源代码中不会影响性能和可伸缩性

# 附录A


![图A-1 架构分析概述](http://upload-images.jianshu.io/upload_images/1650675-bce2781c996c047c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

-------


