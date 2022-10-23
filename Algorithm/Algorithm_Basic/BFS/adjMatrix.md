# 인접 행렬에서의 BFS

## 1. 실전에서 인접 행렬에 대한 마음가짐
- 인접행렬은 from/to 행렬이고, 데이터로 간선 정보가 들어간다.  
- 문제에서 그래프 인풋이 2차원 행렬로 주어진다면, 인접 행렬로 푼다.  
- 인접 리스트의 두 정점의 연결 여부를 알기위한 시간 복잡도는 O(간선수) 이다. 이 부분은 from/to 배열인 인접행렬이 한번에 찾을 수 있는것에 비해 비효율적이다.  
- 인접 노드를 파악함에 있어 인접 리스트는 특정 인덱스에 연결된 리스트만 확인하면 된다. 이것은 인접 행렬이 N*N만큼 루프를 돌아 확인하는것에 비해 효율적이다.


문제에서 그래프 인풋이 2차원 배열로 오거나, 정점간 연결여부 파악이 주 포인트일떄 인접 행렬을 쓰자.

## 2. 인접 행렬 받기 

2차원으로 올때는 그냥 받고, 연결정보로 올때는 아래처럼 구현한다.  

```java
for (int i = 0; i < E; i++) {
    int from = sc.nextInt();
    int to = sc.nextInt();

    // 무향 그래프
    adjMatrix[from][to] = 1;
    adjMatrix[to][from] = 1;
}
```

## 3. BFS

1. 큐 생성 및 처음 위치 방문체크 및 큐에 넣기 & 큐가 빌 때 까지 while문 돌리기
2. 큐에서 데이터를 꺼내고, 꺼낸 데이터를 from으로 하는 to 정보들을 탐색 -> 방문 체크와 인접 체크 진행 
3. 방문하지 않은 인접한 to 정점을 큐에 넣고 방문 체크


```java
private static void bfs() {
    // 1. 큐 생성 및 처음 위치 방문체크 및 큐에 넣기 & 큐가 빌 때 까지 while문 돌리기
    Queue<Integer> queue = new ArrayDeque<>();
    queue.offer(0);
    visited[0] = true;

    while (!queue.isEmpty()) {
        // 2. 큐에서 데이터를 꺼내고, 꺼낸 데이터를 from으로 하는 to 정보들을 탐색 -> 방문 체크와 인접 체크 진행 
        int cur = queue.poll();
        for (int i = 0; i < N; i++) {
            if (!visited[i] &&
                    adjMatrix[cur][i] != 0) { // 방문 x && 인접
                
                // 3. 방문하지 않은 인접한 to 정점을 큐에 넣고 방문 체크
                queue.offer(i);
                visited[i] = true;
            }
        }
    }
}
```
