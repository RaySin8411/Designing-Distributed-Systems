# 分片服務

## 複本服務 v.s. 分片服務

![](https://www.oreilly.com/library/view/designing-distributed-systems/9781491983638/assets/ddis_08in01.png)

類型 | 用途
--- | ---
複本服務 | 建置無狀態服務
分片服務 | 建置有狀態服務

## 分片快取 Sharded Cache
位於用戶請求和實際前端實現之間的暫存

需考慮到的設計面向:
* 為什麼需要
    
    增加儲存在服務的資料大小

* 快取角色在架構中的作用

    * 效能
        * 密中率(Hit Rate)
        * 延遲(Latency)
        
    * 複本(Replicated)
* 複本的分片快取
    * 原因
        * 為了延遲或負載而相當依賴於快取，當出現故障或進行部署時，遺失整個快取分片是無法接受的
        * 在特定的快取分片(Cache Shard)上可能有大量的負載，而且需要擴展快取分片來處理負載
    * 優點
        * 特過複本服務取代單一伺服器(Single Server)，每個快取分片都是彈性、且可以預防故障，在故障期間始終存在
        * 可以依賴快取提供的效能改進，而不是將系統設計成為了容錯，當快取分片失敗時，導致效能下降。
        * 因為都是獨立的服務，因此可以擴展每個快取分片以響應負載
    
    * 實作
        * 為分片快取部屬大使和Memcache
* 分片函式
    
    給定Req 和 Shard
        
        Shard = ShardingFunction(Req)
    
    分片函式(Sharding Function)通常是用雜湊函式和取模運算(mod, %)所定義。
    
    雜湊函式對於分片具有兩個重要特徵:
        
        確定性(Determinism)
        
            對於唯一輸入，輸出應始終相同
        
        均勻度(Uniformity)
    
            輸出空間的輸出分配應該相等
        
    10個分片的分片服務
    
        Shard = hash(Req) % 10
    
    * 實作
        * 建立一致性HTTP分片代理服務
        
## 熱分片系統
因為有機負載模式(Organic Load Pattern)會驅使更多流量到一個特定分片