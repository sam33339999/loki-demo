# Loki learn

> Grafana Loki 是 Prometheus 系列的類似工具，但是面向的是 log 相關功能，透過 Grafana 的介面，使得 log 監控可視化展示，並且可以查詢。


## docker-compose 的相關資源：
- [Loki](https://grafana.com/oss/loki/): 下方有說明
- [promtail](https://grafana.com/docs/loki/latest/clients/promtail/): 下方有說明
- [Grafana](https://grafana.com/docs/grafana/latest/): 下方有說明
- [Prometheus](https://prometheus.io/): 監控軟體
- [Minio](https://min.io/): object-storage 工具，可以模擬 S3 的存儲。

## 相關流程：

- Loki: 是一個 Server 可以使 log 導入、分析、查詢的一個 api server
- promtail: 是一個代理，他將 log 監聽，並且傳入 Loki ， 正常來說， promtail 可以是宿主機或是外部機器，但是 loki 要確定能夠訪問即可；這樣他會將蒐集到的日誌資料網 Loki 傳入。
- Grafana: 一個 UI 介面，提供方便查詢 Loki 的條件，並且可視化監控狀況，了解系統日誌的相關資訊。

## 值得一提：
在嘗試使用 Loki 的時候，以為 docker-compose 只需要啟動兩個容器(`Loki`, `Grafana`)即可，但是實際案例是，Loki 必須要有資料收集後，Test 按鈕才不會噴錯。[{grafana}/connections/datasources](http://127.0.0.1:3000/connections/datasources) 這邊點擊已經註冊的提供者，點入後有個 Test 的按鈕可以測試是否連接成功。

## Loki api:
### [api-document](https://grafana.com/docs/loki/latest/reference/api/)
- `/ready`: health check api
- `/loki/api/v1/push`: 推入 log 紀錄的 api，如上所示，如果並未使用 promtail 將資料加載進入 Loki 的話，可以透過該 api 將紀錄打入 Loki