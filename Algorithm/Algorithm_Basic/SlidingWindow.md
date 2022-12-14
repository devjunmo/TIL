# Sliding Window

## 1. 개념

[1, 2, 3], 4, 5, 6, 7  
1, [2, 3, 4], 5, 6, 7  
1, 2, [3, 4, 5], 6, 7  
1, 2, 3, [4, 5, 6], 7  
1, 2, 3, 4, [5, 6, 7]  
  

그림과 같이 고정된 윈도우 크기만큼 배열의 요소들을 순차적으로 지나가며 문제를 해결.  
완전 탐색으로 했을때 O(N제곱)의 시간복잡도를 O(N)의 시간복잡도로 해결하기 위한 아이디어.

window가 한칸 이동했을 때
- 큐에 원소가 하나 빠진다
- 큐에 원소가 하나 들어온다

윈도우가 이동하기 전후 공통되는 정보는 살리면서 바뀐 부분에 대해서만 갱신하도록 코드를 짜야함.


## 2. 예제 

### ✒︎ BOJ 15961 회전초밥 

이 문제는 완전탐색으로 접근하면 300만 * 3000 = 90억으로 시간초과가 발생한다.  
떄문에 O(N제곱)은 불가능한 상황이고, 연속된 K개의 초밥에 대한 문제이므로 슬라이딩 윈도우를 떠올린다.  

큐 자료구조를 통해 window를 구현하였고, 초밥 고유 넘버를 인덱스로, 윈도우에 선택된 초밥 종류의 카운트를 값으로 하는 1차원 배열을 통해 해시와 비슷한 효과를 냈다.  

window 큐에 k개만큼 초밥이 들어간 상태에서 window가 움직이며 초밥을 빼고 / 넣는 작업을 진행하며, 초밥을 뺄때와 넣을때 초밥 카운트 배열을 참고하여 고유한 초밥의 갯수를 update 하였다. 이때 윈도우 움직임 전후에 공유되는 고유한 초밥의 갯수는 유지되며, 양 말단에 바뀌는 부분에 대해서만 카운트가 변화 되는것이 핵심이다.
