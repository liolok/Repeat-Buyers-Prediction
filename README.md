# 天猫回头客预测

## 问题定义

商家有时会在特定日期（例如节假日，“黑色星期五”或“双11”）进行大促销（例如折扣或代金券），以吸引大量新买家。然而吸引而来的买家大多只做一锤子买卖，这些促销可能几乎无法对销售产生长期影响。为了缓解这个问题，商家必须确定哪些买家可以成为回头客。通过锁定这些潜在的回头客，商家可以大大降低促销成本，提高投资回报率（ROI）。众所周知，在线广告领域，客户定位极具挑战性，特别是对于新买家而言。但是，凭借天猫长期积累的用户行为记录，我们或许可以解决这个问题。

我们提供在“双十一”促销期间获得的一组商家及其相应新买家的集合。您的任务是预测给定商家的哪些新买家将来会成为回头客。换句话说，您需要预测这些新买家在6个月内再次从同一商家购买商品的概率。

## 数据描述

数据集包含在“双十一”当天和前6个月内的匿名用户的购物记录，以及指示他们是否为重复购买者的标签信息。由于隐私问题，数据以偏向方式进行采样，因此该数据集的统计结果将偏离天猫的实际情况。但这不会影响解决方案的适用性。用于训练和测试的数据集文件见 [`data_format2.zip`](Data/data_format2.zip)。数据格式详见下表。

| 数据字段       | 定义                                                                                                                                                                                                                   |
| -------------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `user_id`      | 用户唯一标识                                                                                                                                                                                                           |
| `age_range`    | 用户年龄范围：(1)[<18],(2)[18,24],(3)[25,29],(4)[30,34],(5)[35,39],(6)[40,49],(7 and 8)[>=50],0 and NULL表示未知                                                                                                       |
| `gender`       | 用户性别：0表示女性，1表示男性，2表示NULL表示未知                                                                                                                                                                      |
| `merchant`     | 商家唯一标识                                                                                                                                                                                                           |
| `label`        | {0,1，-1，NULL} '1'表示'user_id'是'merchant_id'的重复买家，而'0'则相反。 '-1'表示'user_id'不是给定商家的新客户，因此超出了我们的预测。 但是，此类记录可能会提供其他信息。 'NULL'仅出现在测试数据中，表明它是一对预测。 |
| `activity_log` | {user_id，merchant_id}之间的交互记录集，其中每个记录是表示为'item_id：category_id：brand_id：time_stamp：action_type'的动作。 '＃'用于分隔两个相邻元素。 记录不按任何特定顺序排序。                                 

您的提交应命名为“prediction.csv”，格式如下。

| 数据字段      | 定义                                               |
| ------------: | -------------------------------------------------- |
| `user_id`     | 用户唯一标识                                       |
| `merchant_id` | 商家唯一标识                                       |
| `prob`        | 该用户成为该商家回头客的概率，值介于 0 和 1 之间。 |

> 提交文件示例参见 [`sample_submission.csv`](Data/sample_submission.csv)

> 另一种格式的数据

我们还以另一种格式提供了相同的数据集，其中包含4个文件，并且对于特征工程用户来说可能更加友好。文件见 [`data_format1.zip`](Data/data_format1.zip)。数据格式详见下表：

用户行为日志：

| 数据字段      | 定义                                                                                              |
| ------------: | ------------------------------------------------------------------------------------------------- |
| `user_id`     | 用户唯一标识                                                                                      |
| `item_id`     | 项目唯一标识                                                                                      |
| `cat_id`      | 项目所属类别唯一标识                                                                              |
| `merchant_id` | 商家唯一标识                                                                                      |
| `brand_id`    | 品牌唯一标识                                                                                      |
| `time_tamp`   | 用户行为发生日期（格式：MMDD）。                                                                  |
| `action_type` | 用户行为描述，枚举类型{0,1,2,3}，其中0表示单击，1表示添加到购物车，2表示购买，3表示添加到收藏夹。 |

用户资料：

| 数据字段    | 定义                                               |
| ----------: | -------------------------------------------------- |
| `user_id`   | 用户唯一标识                                       |
| `age_range` | 用户年龄范围描述，1代表<18；2代表[18，24]；3代表[25，29]；4代表[30，34]；5代表[35，39]；6代表[40，49]；7和8代表>=50；0和NULL代表未知。                                    |
| `gender`      | 用户性别描述，其中0表示女性，1表示男性，2表示用户性别未知。 |

训练及测试数据：

| 数据字段      | 定义                                               |
| ------------: | -------------------------------------------------- |
| `user_id`     | 用户唯一标识                                       |
| `merchant_id` | 商家唯一标识                                       |
| `label`        | 用户购买描述，枚举类型{ 0, 1 }，其中1表示重复买方，0为非重复买方。此字段为空则为测试数据。 |
