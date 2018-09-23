# 天猫回头客预测

## 问题定义

商家有时会在特定日期（例如节假日，“黑色星期五”或“双11”）进行大促销（例如折扣或代金券），以吸引大量新买家。然而吸引而来的买家大多只做一锤子买卖，这些促销可能几乎无法对销售产生长期影响。为了缓解这个问题，商家必须确定哪些买家可以成为回头客。通过锁定这些潜在的回头客，商家可以大大降低促销成本，提高投资回报率（ROI）。众所周知，在线广告领域中的客户定位极具挑战性，特别是对于新买家而言。但是，凭借天猫长期积累的用户行为记录，我们或许可以解决这个问题。

我们提供在“双十一”促销期间获得的一组商家及其相应新买家的集合。您的任务是预测给定商家的哪些新买家将来会成为回头客。换句话说，您需要预测这些新买家在 6 个月内再次从同一商家购买商品的概率。

## 数据描述

数据集包含在“双十一”当天和前 6 个月内的匿名用户的购物记录，以及指示他们是否（在未来 6 个月内从同一商家）再次购买商品的标签信息。由于隐私问题，数据以偏向性方式取样，因此该数据集的统计结果将偏离天猫的实际情况，但这不会影响解决方案的适用性。用于训练和测试的数据集文件见 [`data_format2.zip`](Data/data_format2.zip)。数据格式详见下表。

| 数据字段       | 定义                                                                                                                                                                                                               |
| -------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `user_id`      | 用户唯一标识                                                                                                                                                                                                       |
| `age_range`    | [用户年龄范围](#用户年龄范围)                                                                                                                                                                                      |
| `gender`       | 用户性别：`0` 为女性；`1` 为男性；`2`/`NULL` 为未知。                                                                                                                                                              |
| `merchant_id`  | 商家唯一标识                                                                                                                                                                                                       |
| `label`        | 回头客标签：取值范围 `{0, 1，-1，NULL}`。`1` 表示该用户是该商家的回头客； `0` 则反之；`-1` 表示该用户不是给定商家的新买家，故不进行预测，但此类记录可能提供额外信息；`NULL` 仅出现在测试数据中，表明需要进行预测。 |
| `activity_log` | 该用户与该商家之间的交互记录集合，其中每个记录是表示为 `item_id`:`category_id`:`brand_id`:`time_stamp`:`action_type`（定义详见 [用户行为记录](#用户行为记录)）；`#` 用于分隔两个相邻记录；记录无特定排列顺序。     |

#### 用户年龄范围

| `age_range` | 表示范围 |
| ----------: | -------- |
| 0           | 未知     |
| 1           | <18      |
| 2           | [18.24]  |
| 3           | [25,29]  |
| 4           | [30,34]  |
| 5           | [35,39]  |
| 6           | [40,49]  |
| 7           | >50      |
| 8           | >50      |
| NULL        | 未知     |

### 另一种格式的数据

我们还以另一种格式提供相同的数据集，其中包含 4 个文件，对特征工程可能更为用户友好。文件参见 [`data_format1.zip`](Data/data_format1.zip)。数据格式详情如下。

#### 用户行为记录

数据详见 `user_log_format1.csv`（由 [`data_format1.zip`](Data/data_format1.zip) 解压得到）

| 数据字段      | 定义                                                                                                     |
| ------------: | -------------------------------------------------------------------------------------------------------- |
| `user_id`     | 用户唯一标识                                                                                             |
| `item_id`     | 商品唯一标识                                                                                             |
| `cat_id`      | 商品类别                                                                                                 |
| `seller_id`   | 商家唯一标识                                                                                             |
| `brand_id`    | 商品品牌                                                                                                 |
| `time_stamp`  | 行为发生日期（格式为 `MMDD`）。                                                                          |
| `action_type` | 行为类型：取值范围 `{0, 1, 2, 3}`。其中 `0` 为点击；`1` 为添加到购物车；`2` 为购买；`3` 为添加到收藏夹。 |

#### 用户信息

数据详见 `user_info_format1.csv`（由 [`data_format1.zip`](Data/data_format1.zip) 解压得到）

| 数据字段    | 定义                                                  |
| ----------: | ----------------------------------------------------- |
| `user_id`   | 用户唯一标识                                          |
| `age_range` | [用户年龄范围](#用户年龄范围)                         |
| `gender`    | 用户性别：`0` 为女性；`1` 为男性；`2`/`NULL` 为未知。 |

#### 训练数据

数据详见 `train_format1.csv`（由 [`data_format1.zip`](Data/data_format1.zip) 解压得到）

| 数据字段      | 定义                                                               |
| ------------: | ------------------------------------------------------------------ |
| `user_id`     | 用户唯一标识                                                       |
| `merchant_id` | 商家唯一标识                                                       |
| `label`       | 回头客标签：取值范围 `{0, 1}`。其中 `1` 表示为回头客；`0` 则反之。 |

#### 测试数据

数据详见 `test_format1.csv`（由 [`data_format1.zip`](Data/data_format1.zip) 解压得到）

| 数据字段      | 定义                                                   |
| ------------: | ------------------------------------------------------ |
| `user_id`     | 用户唯一标识                                           |
| `merchant_id` | 商家唯一标识                                           |
| `prob`        | 该用户成为该商家回头客的概率，值介于 `0` 和 `1` 之间。 |

> **注：测试数据集中的 `prob` 字段为空，待填入预测结果。**

### 预测结果提交格式

提交应命名为 `prediction.csv`，格式如下。

| 数据字段      | 定义                                                   |
| ------------: | ------------------------------------------------------ |
| `user_id`     | 用户唯一标识                                           |
| `merchant_id` | 商家唯一标识                                           |
| `prob`        | 该用户成为该商家回头客的概率，值介于 `0` 和 `1` 之间。 |

> 提交文件示例参见 [`sample_submission.csv`](Data/sample_submission.csv)。
