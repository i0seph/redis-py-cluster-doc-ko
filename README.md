# redis-py-cluster-doc-ko
redis cluster text base user interface 만들기 설계

## 필요한 것들
* 노드 트리
  * 노드 추가/삭제/이동(master -> slave, slave -> master)
  * 슬롯 통계

## 슬롯 이동
1. 슬롯번호로 해당 노드 찾고, 접속
1. 옮겨 갈 대상 노드로 접속
1. 대상 노드에서 cluster setslot #slotnum importing 해당슬롯노드
1. 원래 노드에서 cluster setslot #slotnum migrating 대상노드
1. 원래 노드에서 cluster countkeysinslot 값이 0보다 크면
    * for key in cluster getkeysinslot
      * migrate 대상노드_host 대상노드_port key 0 10000
1. 원래 노드에서 cluster setslot #slotnum node 대상노드
1. 대상 노드에서 cluster setslot #slotnum node 대상노드
  

## 노드 추가 meet

## 노드 삭제 forget

## docker를 이용한 redis cluster 구축

윗 작업이 다 의미 없어졌다.

```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce
docker network create redis-net
systemctl stop docker
systemctl start docker
docker network inspect redis-net
mkdir /docker/redis && cd /docker/redis
mkdir redis1 redis2 redis3
cat > /docker/redis/default.conf
cluster-enabled yes
cluster-config-file nodes.conf
^D
cp /docker/redis/default.conf redis1
cp /docker/redis/default.conf redis2
cp /docker/redis/default.conf redis3
docker run --network=redis-net --ip=172.19.0.11 -it --name=redis1 -p 6379:6379 --sysctl net.core.somaxconn=1024 -v /docker/redis/redis1:/data redis redis-server /data/redis.conf &
docker run --network=redis-net --ip=172.19.0.12 -it --name=redis1 -p 6380:6379 --sysctl net.core.somaxconn=1024 -v /docker/redis/redis1:/data redis redis-server /data/redis.conf &
docker run --network=redis-net --ip=172.19.0.13 -it --name=redis1 -p 6381:6379 --sysctl net.core.somaxconn=1024 -v /docker/redis/redis1:/data redis redis-server /data/redis.conf &
docker exec -it redis1 redis-cli --cluster create 172.19.0.11:6379 172.19.0.12:6379 172.19.0.13:6379
docker exec -it redis1 redis-cli cluster info
docker exec -it redis1 redis-cli cluster nodes
```
