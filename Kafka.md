## Kafka



### ***概念***

Broker: 集群中的机器节点。

Topic: 订阅的主题，顶层的逻辑概念，Producer、Consumer 所使用模型中最重要的概念。

Partition: 分区，从属于主题，提及 Partition，一般默认特定 Topic 下的 Partition。

Replica: 副本，从属于 Partition，同一 Partition 的不同 Replica 需保持一致，保证高可用的设计。

Leader/Follower: 同一 Partition 下，只有一个 Replica 是 Leader，其余是 Follower。Follower 需从 Leader 同步数据。

Controller: 集群控制器，集群中只有一个 Broker 可以成为 Controller。

Producer: 生产者，可指定消息分区，或自定义分区器，可自定义序列化器。

Consumer Group: 消费者群组，不同的 Consumer Group 中的 Consumer，消费同一 Topic 下的消息，互不影响。

Consumer: 消费者，可指定消费者分区分配策略，可自定义分区分配策略，可自定义反序列化器。

Offset: 偏移量，每个 Partition 在每个 Consumer Group 均有唯一的偏移量。



### ***Producer***

*过程*：

消息指定 Topic，分区器分区（默认、自定义），消息发送（序列化，默认、自定义），消息传递保证（acks，0、1、all）

选择该分区的 Leader 副本发送生产者消息。



### ***Consumer***

*过程*：

订阅 Topic，分区分配（分配策略，获取分区，Rebalance），offset （无效时，策略）开始，消息获取（反序列化，默认、自定义）、提交 offset

选择该分区的 Leader 副本发送消费者消息。

*消费者分区分配*：

策略：Range、RoundRobin。注：同一个 ConsumerGroup 下，一个 Topic-Partition 只能被分配给一个 Consumer。自定义 ParitionAssignor。

实施：GroupCoordinator，选举 Consumer leader，选举分配策略，交给 leader 实施。

Rebalance：消费者加入、退出时，触发。

自定义 ConsumerRebalanceListener。



### ***Kafka Cluster***



选举 Controller，管理集群元信息，分区副本在集群中的分布信息，分区副本中的 Leader 选举。



*场景*：

当新的 Broker 添加至集群时，不会重新均衡「分区副本在集群中的分布」。需手动迁移分区副本。

当 Controller 不可用时，所有 Broker 重新参与 Controller 选举。

当 Broker 不可用时，重新选举相关的分区副本的 Leader，由 Controller 选举。

当创建新的 Topic 时，在集群中分配该 Topic 下的各分区副本。

当动态变更 Partition、副本数时？借助工具



*分区副本在集群中的分布规则*：

原则：

- 在 broker 间平均地分布分区副本。
- 每个分区的每个副本分布在不同的 broker 上。
- 如果为 broker 指定了机架信息，那么尽可能把每个分区的副本分配到不同机架的 broker 上。

分区副本分配算法（无机架信息）：

1. 将 N 个 Broker 和待分配的多个 Partition 排序。

2. 将第 i 个 Partition 分配到第 (i mod N) 个 Broker 上。

3. 将第 i 个 Partition 的第 j 个副本分配到第 ((i + j) mod N) 个 Broker 上。



### ***分区副本***

*复制*：

同一分区下，分为 Leader 副本（仅一个）、Follower 副本。

所有该分区的生产者请求和消费者请求，都会经过 Leader 副本。

Follower 副本向 Leader 副本发送请求（与消费者请求相同），同步数据。持续请求最新数据（zk 活跃，10s 内请求，偏移量最新）的副本是同步副本。

只有同步副本，才有可能在 Leader 副本不可用时，被选举为 Leader 副本。这样的选举是完全选举。

如果允许非同步副本参与选举，成为 Leader 副本，这样选举是非完全选举。

























