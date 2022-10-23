# 인접 리스트에서의 BFS

## 1. 실전에서 인접 리스트에 대한 마음가짐

- 인접 리스트의 두 정점의 연결 여부를 알기위한 시간 복잡도는 O(간선수) 이다. 이 부분은 from/to 배열인 인접행렬이 한번에 찾을 수 있는것에 비해 비효율적이다.  

- 인접 노드를 파악함에 있어 인접 리스트는 특정 인덱스에 연결된 리스트만 확인하면 된다. 이것은 인접 행렬이 N*N만큼 루프를 돌아 확인하는것에 비해 효율적이다.

문제에서 그래프 인풋이

```
0 1
0 2
1 3
1 4
3 5
4 5
5 6
2 6
```

이런식으로 오고, 문제에서 정점간 연결여부 파악이 주 포인트가 아닐때 연결 리스트를 쓰자.

자바에서 인접 리스트는 연결리스트로도 구현이 가능하지만, 더 마음 편한 방법은

```java
ArrayList<ArrayList<Integer>> adjLst;
```

이렇게 어레이리스트로 구성된 배열의 배열 형태를 이용하는 것이다.

그래프 생성은

1. 리스트들을 꽂을 base arrList 생성
2. base의 각 인덱스에 new로 arrLst 객체 꽂기
3. 무향그래프 일 때 기준으로 from, to를 양방향으로 넣어주기

```java
adjLst = new ArrayList<>(); // 1. 리스트들을 꽂을 base arrList 생성

for (int i = 0; i < V; i++) {
    adjLst.add(new ArrayList<>()); // 2. base의 각 인덱스에 new로 arrLst 객체 꽂기
}

for (int i = 0; i < E; i++) {
    int from = sc.nextInt();
    int to = sc.nextInt();

    adjLst.get(from).add(to); // 3. 무향그래프 일 때 기준으로 from, to를 양방향으로 넣어주기
    adjLst.get(to).add(from);
}
```

## 2. BFS

시작 정점을 받고

1. 큐 생성 및 첫 정점을 offer 후 큐가 빌 때 까지 while문 돌리기
2. 큐에서 데이터를 꺼내고 꺼낸 데이터에 대해 방문체크. 방문을 했다면 pass, 안했다면 방문체크
3. 해당 정점에 연결되어있는 정점 리스트를 foreach로 돌면서 offer

```java
private static void bfs(int start) {
    // 1. 큐 생성 및 첫 정점을 offer 후 큐가 빌 때 까지 while문 돌리기
    Queue<Integer> q = new ArrayDeque<>();
    q.offer(start); // 첫 스타트 정점을 큐에 오퍼

    while (!q.isEmpty()) {
        // 2. 큐에서 데이터를 꺼내고 꺼낸 데이터에 대해 방문체크. 방문을 했다면 pass, 안했다면 방문체크
        int curV = q.poll();
        if (vis[curV]) continue;
        vis[curV] = true;
        // 3. 해당 정점에 연결되어있는 정점 리스트를 foreach로 돌면서 offer
        for (int v :
                adjLst.get(curV)) {
            q.offer(v);
        }
    }
}
```
