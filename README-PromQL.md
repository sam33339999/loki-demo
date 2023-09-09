# Prometheus Query Language.(PromQL)

- [loki-query: PromQL](https://grafana.com/docs/loki/latest/query/)

> PromQL 是 Prometheus 自己開發的數據查詢 DSL 語言，語言表達線非常豐富，內置函式很多，
日常數據可視化或是 rule 告警都會使用到他。


## PromQL查詢結果主要有三種類型：
- 瞬時數據 (Instant vector): 包含一組時序，每個時序只有一個點，如 http_requests_total
- 區間數據 (Range vector): 包含一組時序，北個時序有多個點。如 http_requests_total[5m]
- 純量數據 (Scalar): 純量只有一個數字，沒有時序。如 count(http_requests_total)

## 語法：
- 算數運算符: `+`(加), `-`(減), `*`(乘), `/`(除_商), `%`(除_餘), `^`(次方),
- 比較運算符: `==`, `!=`, `>`, `>=`, `<`, `<=`
- 邏輯運算符: `and`(交集), `or`(聯集), `unless`(差集)
- 聚合運算符:
    `sum` (calculate sum over dimensions)                               # 范围内求和
    `min` (select minimum over dimensions)                              # 范围内求最小值
    `max` (select maximum over dimensions)                              # 范围内求最大值
    `avg` (calculate the average over dimensions)                       # 范围内求最大值
    `stddev` (calculate population standard deviation over dimensions)  # 计算标准偏差
    `stdvar` (calculate population standard variance over dimensions)   # 计算标准方差
    `count` (count number of elements in the vector)                    # 计算向量中的元素数量
    `count_values` (count number of elements with the same value)       # 计算向量中相同元素的数量
    `bottomk` (smallest k elements by sample value)                     # 样本中最小的元素值
    `topk` (largest k elements by sample value)                         # 样本中最大的元素值
    `quantile` (calculate φ-quantile (0 ≤ φ ≤ 1) over dimensions)      # 计算 0-1 之间的百分比数量的样本的最大值

> 優先級: `^` > `(* / %)` > `(+ -)` > `(== != >= > <= <)` > `(and unless)` > `or`

## (進階語法)矢量選擇器：
- `=`: 匹配與標籤相等的內容
- `!=`: 不匹配與標籤相等的內容
- `=~`: 根據正規表達式匹配與標籤符合的結果
- `!~`: 根據正規表達式不匹配與標籤方符合的結果

## (進階語法)範圍矢量選擇器：
- s 秒
- m 分
- h 時
- d 天
- w 週
- y 年

## (進階語法)偏移量修改器：
> 偏移量修改允許改變查詢中各個即時和範圍向量的時間偏移，簡單說就是可以查偏移時間的部分，預設是全查
如同下方 <a href="#search_5_min_ago">範圍查詢，過去 5 分鐘</a>




## 舉個栗子

- 模糊查詢，以 2XX 狀態為例
> http_requests_total{code~="2xx"}

- 比較查詢，以 value > 100 的數據
> http_requests_total > 100

- <div id="search_5_min_ago">範圍查詢，過去 5 分鐘</div>
> http_requests_total[5m]
> 

- 聚合，統計高級查詢
> count(http_requests_total)

- sum 查詢
> sum(http_requests_total)

- avg 查詢
> avg(http_requests_total)

- top 查詢，查詢靠前的前三筆資訊
> topk(3, http_requests_total)

- irate 查詢，過去 5分鐘平均每秒數值
> irate(http_requests_total[5m])

