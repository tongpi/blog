

[TOC]



## 一、Rabbitmq路由消息的过程

消息到达rabbitmq之后，在到达使用者之前，会发生以下几种情况：

-  rabbitmq需要找到消息需要路由到的一个或多个队列，具体取决于交换类型。 
- rabbitmq将消息的副本放入这些队列中的每个队列中，或者决定将消息返回给发布者。
-  rabbitmq将消息推送到这些队列上的使用者，或者等待应用程序按需获取消息。

### 1.1、更加深入地描述Rabbitmq路由消息的过程： 

1. rabbitmq需要查询消息发布到的交换的绑定列表，以便找到消息需要路由到的一个或多个队列（步骤1） 
2. 如果在步骤1中找不到合适的队列，并且消息是作为强制发布的，那么它将返回给发布者（步骤1b）。
3.  如果有合适的队列，则将消息的副本放入每个队列中（步骤2）。 
4. 如果消息是强制发布的，但没有活动的使用者，则会将其返回给发布者（步骤2b）。 
5. 如果这些队列上有活动的使用者，并且basic.qos设置允许，则消息将被推送到这些使用者（步骤3）

## 二、交换

交换接受来自生产者应用程序的消息，并将它们路由到消息队列。它们可以被认为是AMQP世界的“邮箱”。与其他一些消息中间件产品和协议不同，在AMQP中，消息不会直接发布到队列中。消息将发布到使用预先安排的标准（称为绑定）将它们路由到队列的交换。

AMQP0.9.1规范中有多种交换类型，每种类型都有自己的路由语义。可以创建自定义交换类型来处理复杂的路由方案（例如基于地理位置数据或边缘案例的路由）。

AMQP v0.9.1中有四种内置的交换类型： 

- 直接（Direct） 
- 扇出（Fanout） 
- 话题（Topic） 
- 报头（Headers）

 每种交换类型都有自己的路由语义，并且可以通过使用插件扩展代理来添加新的交换类型。自定义交换类型以“x-”开头，很像自定义HTTP头，例如x-consistent-hash交换或x-random交换。

## 2.1、直接(Direct)交换

直接交换根据消息路由键（每个AMQP v0.9.1消息都包含的属性）将消息传递到队列。它的工作原理如下：

- 队列使用路由密钥K绑定到交换机;
- 当路由键为R的新消息到达直接交换时，如果k=r，交换将其路由到队列。

直接交换是消息单播路由的理想选择（尽管它们也可以用于多播路由）。如下图所示：

![direct exchange routing](https://github.com/ruby-amqp/amqp/raw/master/docs/diagrams/005_direct_exchange.png)

### 直接交换应用场景：

- 在MMO游戏中直接（接近实时）向单个玩家发送消息；
- 向特定地理位置（例如，销售点）发送通知；
- 在同一应用程序的多个实例之间分配任务，所有实例都具有相同的功能，例如，图像处理器。
- 在工作流步骤之间传递数据，每个步骤都有一个标识符（也可以考虑使用头交换）向网络中的各个软件服务发送通知。

## 2.2、默认交换

默认交换是一个没有名字的直接交换（bunny使用空字符串引用它），由broke预先声明。它有一个特殊的属性，这使得它对于简单的应用程序非常有用，即每个队列都自动绑定到它，并使用与队列名称相同的路由键。

例如，当您声明名为“search.indexing.online”的队列时，rabbitmq将使用“search.indexing.online”作为路由键将其绑定到默认交换。因此，使用routing key=“search.indexing.online”发布到默认交换的消息将被路由到队列“search.indexing.online”。换言之，默认交换使得直接将消息传递到队列似乎是可能的，尽管这在技术上不是这样。

## 2.3、主题（topic）交换：

主题根据消息路由密钥与用于将队列绑定到交换的模式之间的匹配，将消息路由到一个或多个队列。

- 主题交换类型通常用于实现各种发布/订阅模式变化。

- 主题交换通常用于消息的多播路由。

- 主题交换可以用于广播路由，但扇出交换对于这个用例通常更有效。

- 基于主题的路由的两个经典例子是股票价格更新和特定位置的数据（例如，天气广播）。消费者指出他们感兴趣的主题（比如订阅一个feed来获取你最喜欢的博客的单个标签，而不是完整的feed）。通过向queuebind方法指定路由模式来启用路由。

  ![img](http://upload.wikimedia.org/wikipedia/commons/thumb/3/30/Multicast.svg/500px-Multicast.svg.png)

### 主题交换应用场景： 

主题交换有一组非常广泛的用例。当一个问题涉及多个消费者/应用程序，这些消费者/应用程序有选择地选择他们想要接收的消息类型时，应该考虑使用主题交换。举几个例子： 

- 分发与特定地理位置相关的数据，例如销售点； 
- 后台任务处理由多个工作人员完成，每个人都能处理特定的任务集； 
- 股票价格更新（以及其他类型财务数据的更新） 涉及分类或标记的新闻更新（例如，仅针对特定运动项目或团队） 
- 云中不同类型服务的协调 分布式体系结构/OS特定的软件构建或打包，其中每个构建者只能处理一个体系结构或OS。

## 2.4、扇出（Fanout）交换

扇出交换将消息路由到绑定到它的所有队列，并且忽略路由密钥。如果N个队列绑定到扇出交换，则当新消息发布到该交换时，消息的副本将传递到所有N个队列。扇出交换是消息广播路由的理想选择。

![fanout exchange routing](https://github.com/ruby-amqp/amqp/raw/master/docs/diagrams/004_fanout_exchange.png)







## 三、绑定的概念

 绑定是队列和交换之间的关联。队列必须绑定到至少一个交换才能接收来自发布服务器的消息。在“绑定”指南中了解有关绑定的更多信息。

### 绑定详解

> 绑定是交换使用（除其他外）将消息路由到队列的规则。要指示Exchange E将消息路由到队列Q，必须将Q绑定到E。绑定可能具有某些交换类型使用的可选路由密钥属性。路由密钥的目的是有选择地将发布到Exchange的特定（匹配）消息与绑定队列匹配。换句话说，路由键的作用就像一个过滤器。 类比如下： 队列就像你在纽约的目的地； 交换就像肯尼迪机场； 绑定是从肯尼迪机场到目的地的路由。可能没有办法，也可能不止一种方法去达到它。 
>
> 有些交换类型使用路由密钥，而有些交换类型不使用路由密钥（无条件地或基于消息元数据路由消息）。如果无法将AMQP消息路由到任何队列（例如，因为没有将其发布到的Exchange的绑定），则根据发布服务器设置的消息属性，将其删除或返回到发布服务器。
>
> 如果应用程序希望将队列连接到Exchange，则需要绑定它们。相反的操作称为解除绑定。
>
> Exchange到Exchange绑定是AMQP 0.9.1的rabbitmq扩展。

## 四、消息

## 4.1、消息数据序列化：

 鼓励您在发布前处理数据序列化（即通过使用JSON、Thrift、Protocol Buffers或其他序列化库）。注意，由于amqp是一个二进制协议，像json这样的文本格式在数据通过网络传输时很容易被探查，因此，如果带宽效率很重要，请考虑使用messagepack或Protocol Buffers

## 4.2、发布强制消息

发布消息时，可以使用强制（mandatory）选项将消息发布为“强制”。当这个强制消息无法路由到任何队列（例如，没有绑定或绑定都不匹配）时，该消息将返回给生产者。

但强制消息无法投递返回给生产者后，生产者可以用用不同的方式来处理它，比如： 将其存储在持久性存储中，以备日后重新交付； 将其发布到其他目标； 记录事件并丢弃消息；

## 4.3、发布持久化消息

 消息可能会在它们被路由到的队列中花费一些时间，然后再被使用。在此期间，代理可能崩溃或经历重新启动。要生存下来，必须将消息保存到磁盘。这会对性能产生负面影响，尤其是对于网络连接存储，如NAS设备和Amazon EBS。AMQP 0.9.1允许应用程序在消息传递的基础上权衡性能的耐久性，反之亦然。

## 4.4、消息属性

 每个AMQP消息都有许多属性。有些属性很重要，经常使用，有些很少使用。AMQP消息属性是元数据，在目的上与HTTP请求和响应头类似。 每个AMQP 0.9.1消息都有一个名为routing key的属性。路由密钥是一个“地址”，Exchange可以使用它来决定如何路由消息。这与HTTP中的URL类似，但更通用。大多数交换类型使用路由键来实现路由逻辑，但有些交换类型忽略它并使用其他条件（例如消息内容）。

