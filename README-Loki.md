# Loki-README

- 在 production 環境中， Loki 應該要前面掛上一個反向代理，讀寫分離。並且開啟 Loki 集群，透過 memberlist 去將讀寫的資料達成一致性。
- 正式環境中的 `schema_config` 會不建議使用 store: `boltdb-shipper`，他不支援高可用及集群。
