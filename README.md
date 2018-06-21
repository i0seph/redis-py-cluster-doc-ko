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


    
