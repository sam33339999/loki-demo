# Loki-config 筆記

> rules 規則存儲，主要支援本地 (local) 和對象文件系統(azure, gcs, s3, swift)

- [loki-configure](https://grafana.com/docs/loki/latest/configure/)
- [loki-configure-example](https://grafana.com/docs/loki/latest/configure/examples/)

---

## schema_config: 用於配置 Loki 的存儲方式和索引方式。他定義了 Loki 在不同時間段使用的存儲模式和索引模式。

- from: 指定了該配置黨的生效起始日
- store: 指定了存儲模式，如 `boltdb-shipper`
- object_store: 指定了對象存儲方式，如 `filesystem`
- schema: 指定了存儲模式的版本，如 v10
- index: 指定了索引前綴和週期。

> 值得注意的是， `boltdb-shipper` 不支援高可用或集群的 Loki 部署，因為他是一個單節點的存儲引擎。他通常與文件系統塊存儲一起用於概念驗證、嘗試 Loki 和開發等用途。


## memberlist: 用於服務發現和成員管理的庫。他是 Loki 的分佈式系統中一個關鍵組件，實現了節點間的通訊及協調。
Memberlist 提供了一種輕量級成員管理與集群發現的解決方案，他使用基於 UDP 的 `gossip` 協議來維護一個動態的成員列表，
使得節點能夠早點被發現和加入集群，並在成員變化時進行動態更新。

- 自動發現和加入集群：上述
- 成員管理：上述
- 節點通訊：memberlist 提供了節點之間通訊機制，用於數據的`複製`和`同步`。


## ring: 是一種數據結構，用於將日誌數據分片發配給不同組件。
> ring 通常存儲在類似 `Consul` 或是 `etcd` 的鍵值存儲(KV 存儲)中。

