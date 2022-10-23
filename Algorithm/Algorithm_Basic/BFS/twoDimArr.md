# 2차원 배열에서의 BFS

보통 N방탐색 유형으로 나옴. 시작점을 인자로 받고 큐를 통해 구현.  
보통 포지션을 큐에 넣는 시점에 방문 체크도 같이 한다.

크게 아래와 같이 세 덩어리로 나눌 수 있다.

1. 큐 생성 및 처음 위치 방문체크 및 큐에 넣기 & 큐가 빌 때 까지 while문 돌리기
2. 큐에서 데이터를 꺼내고, 사방탐색 돌려서 next position 얻어내기
3. next position의 유효성검사 + 방문체크 필터링 후 큐에 넣고 방문체크 하기

(구조 예시)

```java
public static void bfs(Coor start){

        // 1. 큐 생성 및 처음 위치 방문체크 및 큐에 넣기 & 큐가 빌 때 까지 while문 돌리기
        Queue<Coor> queue = new PriorityQueue<>();
        visArr[start.x][start.y] = true;
        queue.offer(start);

        while (!queue.isEmpty()) {
            // 2. 큐에서 데이터를 꺼내고, 사방탐색 돌려서 next position 얻어내기
            Coor curPos = queue.poll();

            if (curPos.x == N - 1 && curPos.y == N - 1) {
                res = curPos.cost + mapArr[N-1][N-1];
                break;
            }
            for (int i = 0; i < 4; i++) {

                int nx = curPos.x + dRow[i];
                int ny = curPos.y + dCol[i];

                // 3. next position의 유효성검사 + 방문체크 필터링 후 큐에 넣고 방문체크 하기
                if (nx>=0 && ny>=0 && nx<N && ny<N && !visArr[nx][ny]){
                    int saveCost = mapArr[nx][ny] + curPos.cost;
                    queue.offer(new Coor(nx, ny, saveCost));
                    visArr[curPos.x][curPos.y] = true;
                }
            }
        }
    }
```
