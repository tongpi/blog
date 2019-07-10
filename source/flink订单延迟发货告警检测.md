

# flink 订单发货监控

# 流程：

- 1、从kafka订单主题和发货主题读取事件（消息）；
- 2、为了解决使用事件（消息）时间处理时的消息乱序问题，分配水位线watermarks;
- 3、按订单分组这些事件（消息），主要靠两种消息中的关联属性：订单号；
- 4、给每一个订单号分配一个具有自定义的时间触发器的滚动时间窗口(TumblingEventTimeWindows)；
- 5、排序窗口内到达的事件直到触发器满足触发条件。触发器检查水印是否已经在窗口中传递了最大的时间戳。这样可以确保窗口已经收集了足够的元素以进行排序；
- 6、再指定一个7天的滚动时间窗口TumblingEventtimeWindow（该窗口使用一个自定义的数量和时间触发）；
- 7、可以靠次数和生成的全部包裹已发货（ALL_PARCELS_SHIPPED）或者是靠时间和生成的THRESHOLD_EXCEEDED来触发。次数可以靠检测同一个窗口中ORDER_CREATED时间的属性“parcels_to_ship”;
- 8、 分开包含ALL_PARCELS_SHIPPED 和 THRESHOLD_EXCEEDED事件的流为两个支流分别写到不同的kafka主题中。


#  代码片段

```java

// 1
List<String> topicList = new ArrayList<>();
topicList.add("ORDER_CREATED");
topicList.add("PARCEL_SHIPPED");
DataStream<JSONObject> streams = env.addSource(
       new FlinkKafkaConsumer09<>(topicList, new SimpleStringSchema(), properties))
       .flatMap(new JSONMap()) // parse Strings to JSON
 
// 2-5
DataStream<JSONObject> orderingWindowStreamsByKey = streams
       .assignTimestampsAndWatermarks(new EventsWatermark(topicList.size()))
       .keyBy(new JSONKey("order_number"))
       .window(TumblingEventTimeWindows.of(Time.days(7)))
       .trigger(new OrderingTrigger<>())
       .apply(new CEGWindowFunction<>());
 
// 6-7
DataStream<JSONObject> enrichedCEGStreams = orderingWindowStreamsByKey
       .keyBy(new JSONKey("order_number"))
       .window(TumblingEventTimeWindows.of(Time.days(7)))
       .trigger(new CountEventTimeTrigger<>())
       .reduce((ReduceFunction<JSONObject>) (v1, v2) -> v2); // always return last element
 
// 8
enrichedCEGStreams
       .flatMap(new FilterAllParcelsShipped<>())
       .addSink(new FlinkKafkaProducer09<>(Config.allParcelsShippedType, 
              new SimpleStringSchema(), properties)).name("sink_all_parcels_shipped");
 
enrichedCEGStreams
       .flatMap(new FilterThresholdExceeded<>())
       .addSink(new FlinkKafkaProducer09<>(Config.thresholdExceededType,
              new SimpleStringSchema(), properties)).name("sink_threshold_exceeded");


```

# 挑战和学习

## CEG触发条件要求事件是有序的

> 按照我们的问题，我们需要all_parcels_shipped事件有一个最后parcel_shipped时间。该CountEventTimeTrigger触发条件因此需要在窗口的事件是有序的，以便我们知道这是最后parcel_shipped事件。

> 我们在步骤2-5中实现排序。当每个元素出现时，键控状态存储这些元素的最大时间戳。在注册时间，触发器检查水印是否大于最大时间戳。如果是这样，窗口已经收集了足够的元素进行排序。我们保证只允许水印在所有事件之间的最早时间戳中取得进展。请注意，按窗口状态的大小排序事件是非常昂贵的，这将它们保存在内存中。


## 事件以不同的速率到达窗口。

> 我们从两个不同的卡夫卡主题：order_created和parcel_shipped读取事件流。前者比后者大得多。因此，前者的阅读速度比后者慢。

> 事件以不同的速度到达窗口。这影响了业务逻辑的实现，尤其是在OrderingTrigger发射条件。它等待两种类型的类型以最小的可见时间戳作为水位线达到相同的时间戳的事件。事件堆在窗口的状态直到引发触发和清除他们。具体来说，如果在主题order_created的事件从1月3日开始，在parcel_shipped的事件是从1月1日开始的，后者将堆积如山，只有在Flink在1月3日处理了前者之后清除他们。这会消耗大量的内存。

## 在计算开始时，一些生成的事件将是不正确的。

> 由于资源有限，我们的卡夫卡队列不能有无限的保留时间，因此事件会过期。当我们启动Flink任务时，计算将不考虑这些过期的事件。由于缺少数据，一些复杂事件不会生成或将不正确。例如，parcel_shipped失踪事件将导致一个threshold_exceeded事件的产生，而不是一个all_parcels_shipped事件。

# 结论

	上面我们已经表明你如何设计和实施threshold_exceeded复杂事件all_parcels_shipped。我们已经表明我们如何生成这些使用Flink的事件处理能力的实时时间。我们也提出了我们前进的道路上遇到了我们是怎么认识的那些使用Flink的强大的事件时处理的特点，即水印的挑战，时间窗口和自定义触发事件.

	Flink的技术人员知道Flink提供了CEP库。当我们开始了我们的用例（Flink1.1），我们确定这些不能很容易地实现它。我们相信完全控制触发器可以在迭代地改进模式时给我们更多的灵活性。

	同时，CEP 库也已经成熟，并在即将到来的Flink1.4中，它也将支持CEP模式状态的动态变化。这将使类似我们的用例的实现更加方便。