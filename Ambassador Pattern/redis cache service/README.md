# Redis 分片服務
Redis 用途
1. Cache
2. Persistent Storage

* 部屬

    kubectl create -f redis-shards.yaml
    
    kubectl create configmap twem-config --from-file=./nutcracker.yaml
    

* 取得資訊
    kubectl get pods