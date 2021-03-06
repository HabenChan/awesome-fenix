# SOA时代

::: tip SOA架构（Service-Oriented Architecture）

面向服务的架构是第一次系统性地成功解决分布式服务主要问题的架构模式。

:::

当系统规模变大后，为了能对系统进行垂直拆分、复用，人们尝试过多种途径，笔者列举以下三种较有代表性的架构模式，分别为：

- [烟囱式架构](https://en.wikipedia.org/wiki/Information_silo)（Information Silo Architecture）：信息烟囱又名信息孤岛（Information Island），使用这种架构的系统也被称为孤岛式信息系统或者烟囱式信息系统。它指的是一种完全不与其他相关信息系统之间进行互操作或者说协调工作的信息系统。这样的系统其实并没有什么“架构”可言，如果两个部门真的完全不会发生交互，就并没有什么理由强迫把它们必须在一栋楼里办公；两个不发生交互的信息系统，让他它们使用独立的数据库、服务器即可完成拆分，而唯一的问题，也是致命的问题是，企业中真的存在完全不发生交互的部门？对于两个信息系统来说，哪怕真的毫无业务往来关系，但系统的人员、组织、权限等等主数据，会是完全独立、没有任何重叠的吗？这样“独立拆分”、“老死不相往来”的系统，显然不可能是企业所希望见到的。

- [微内核架构](https://zh.wikipedia.org/wiki/%E5%BE%AE%E5%85%A7%E6%A0%B8)（Microkernel Architecture）：微内核架构也被称为插件式架构（Plug-in Architecture）。既然烟囱式架构中，两个没有业务往来关系的系统也可能需要共享一部分的公共的主数据，那不妨就将这些主数据，连同其他可能被所有系统使用到的公共服务、数据、资源集中到一块，成为一个被所有业务系统共同依赖的核心系统（Kernel，也称为Core System），具体的业务系统就以插件模块（Plug-in Modules）的形式存在，这样便可提供可扩展的，灵活的，天然隔离的功能特性。<br/>
  这种模式很适合桌面应用程序，也可以在Web应用程序中使用。事实上，本文列举的各种不同的架构模式一般都可视为整个系统的一种插件。对于产品型应用程序来说，如果我们想将新特性和功能及时加入系统，微内核架构是一种不错的选择。微内核的架构也可以嵌入到其它的架构模式之中，通过插件还可以提供逐步演化的功能和增量开发。所以如果你实现能够支持二次开发的软件系统，微内核是一种良好的架构模式。<br/>
  不过，微内核架构也有它的局限和前提，这便是它假设各个插件模块之间是互不认识（不可预知系统会安装哪些模块），不会发生交互的，但无论在企业还是互联网，这一前提假设却通常并不成立，我们必须找到办法，既能垂直拆分系统，也能让拆分后的子系统之间可以互相调用通讯。

:::center
![](./images/coresystem.png)
图片来自O'Reilly的开放文档《[Software Architecture Patterns](https://www.oreilly.com/programming/free/files/software-architecture-patterns.pdf)》
:::

- [事件驱动架构](https://en.wikipedia.org/wiki/Event-driven_architecture)（Event-Driven Architecture）：为了能让子系统互相通讯，一种可行的方案是在子系统之间建立一套事件队列管道（Event Queues），来自系统外部的消息将以事件的形式发送至管道中，各个子系统可以从管道里获取自己感兴趣、可以处理的事件消息，可以为事件新增或者修改其中的附加信息，甚至可以自己发布一些新的事件到管道队列中去，如此，每一个消息的处理者都是独立的，高度解耦的，但又能与其他处理者（如果存在该消息处理者的话）通过事件管道进行互动。

:::center
![](./images/eventbus.png)
图片来自O'Reilly的开放文档《[Software Architecture Patterns](https://www.oreilly.com/programming/free/files/software-architecture-patterns.pdf)》
:::

当系统演化至事件驱动架构时，第一节提到的仍在并行发展的远程服务调用也来到了SOAP时代（详见[远程服务调用](/architect-perspective/general-architecture/api-style/rpc.html)一文），此时SOA已经有了它登场所需要的全部前置条件。SOA的概念最初由Gartner公司在1994年提出，2006年，由IBM、Oracle、SAP等公司共同成立了OSOA联盟（Open Service Oriented Architecture），用于制定和推进SOA相关标准，在2007年，国际标准组织（OASIS）领导下，OSOA联盟的职能并入了新成立了[Open CSA](http://www.oasis-opencsa.org/)组织。

尽管SOA仍是抽象概念，而不是特指某一种具体的技术，但它已经比前面所说的三种架构模式要相对具体、充实了很多，已经不能简单视其为一种架构模式，可以称为是一套软件设计的方法论了。它拥有领导制定技术标准的组织Open CSA；有清晰软件设计的指导原则，譬如服务的封装性、自治、松耦合、可重用、可组合、无状态，等等；明确了采用SOAP作为远程调用的协议，依靠SOAP协议族（WSDL、UDDI和一大票WS-*协议）来完成服务的发布、发现和治理；利用一个被称为[企业服务总线](https://zh.wikipedia.org/zh-hans/%E4%BC%81%E4%B8%9A%E6%9C%8D%E5%8A%A1%E6%80%BB%E7%BA%BF)（Enterprise Service Bus，ESB）的消息管道来实现各个子系统之间的通讯交互，令各服务间在ESB调度下无需相互依赖却能相互通讯，既带来了服务松耦合的好处，也为以后可以进一步实现[业务流程编排](https://zh.wikipedia.org/wiki/%E4%B8%9A%E5%8A%A1%E6%B5%81%E7%A8%8B%E7%AE%A1%E7%90%86)（Business Process Management，BPM）提供了基础；使用[服务数据对象](https://zh.wikipedia.org/wiki/%E6%9C%8D%E5%8A%A1%E6%95%B0%E6%8D%AE%E5%AF%B9%E8%B1%A1)（Service Data Object，SDO）来访问和表示数据，使用[服务组件架构](https://zh.wikipedia.org/wiki/%E6%9C%8D%E5%8A%A1%E7%BB%84%E4%BB%B6%E6%9E%B6%E6%9E%84)（Service Component Architecture，SCA）来定义服务封装的形式和服务运行的容器，等等。

当软件架构发展至SOA时代，其中的许多思想都已经能在今天微服务中找到对应的身影了。服务之间的松散耦合、注册、发现、治理，隔离、编排，等等。这些今天微服务中耳熟能详的名词概念，多数是在分布式服务刚被提出时就已经存在的难题，这些难题在SOA时代进行过系统性的探索，才形成了前面所列的这些概念；今天的微服务架构依然要面临这些问题，但它们在SOA架构中就曾经被解决过一次，甚至如果仅从技术可行性这一个角度来评判的话，SOA可以算基本成功地解决了这些问题。

但是，SOA并没有能真正解决的关键性问题反而是三十年前原始分布式时代时提出的“如何使用多个独立的分布式服务共同构建一个更大型系统？”——这本该是SOA或者任何一个分布式架构的首要目标。笔者曾在[远程服务调用](/architect-perspective/general-architecture/api-style/rpc.html)一文中提到SOAP协议被逐渐边缘化的本质原因：过于严格的规范定义带来过度的复杂性。而构建在SOAP基础之上的ESB、BPM、SCA、SDO等诸多上层建筑，进一步加剧了这种复杂性。SOA诞生的那一天起，就已经注定了它只能是少数系统阳春白雪式的精致奢侈品，它可以实现多个异构大型系统之间的复杂集成交互，却很难作为一种具有广泛普适性的软件架构风格来推广。SOA最终没有获得成功的致命伤与当年的[EJB](https://zh.wikipedia.org/wiki/EJB)如出一辙，尽管有Sun Microsystems和IBM等一众巨头在背后力挺，EJB仍然败于以Spring、Hibernate为代表的“草根框架”，可见一旦脱离人民群众，终究会淹没在群众的海洋之中，连信息技术也不曾例外过。

当你读到这一段的时候，不妨重新翻到开头，回头想一想“如何使用多个独立的分布式服务共同构建一个更大型系统”这个问题，再回顾下“原始分布式时代”一节中Unix DCE提出的分布式服务的主旨：“让开发人员不必关心服务是远程还是本地，都能够透明地调用服务或者访问资源”。经过了三十年的技术进步，信息系统经历了巨石、烟囱、微内核、事件驱动、SOA等等的架构模式，应用受架构复杂度的牵绊却是越来越大，已经距离“透明”二字越来越远了，这是否算不自觉间忘记掉了当年的初心？接下来我们所谈论的微服务时代，似乎正是带着这样的自省式的问句而开启的。