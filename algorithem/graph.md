## 그래프 문제에서 정점과 간선 정보를 입력받았을때
### 1. 인접리스트
각 정점에 연결된 정점들을 리스트로 저장하는 방식
```java
int V = 5;

ArrayList<ArrayList<Integer>> graph = new ArrayList<>();

// 리스트 초기화
for (int i = 0; i <= V; i++) {
    graph.add(new ArrayList<>());
}

// 간선 입력 (1-2, 1-4, 2-3, 2-5, 4-5)
graph.get(1).add(2);
graph.get(2).add(1);

graph.get(1).add(4);
graph.get(4).add(1);

graph.get(2).add(3);
graph.get(3).add(2);

graph.get(2).add(5);
graph.get(5).add(2);
```
### 2. 인접행렬
2차원 배열을 이용해서 정점 사이의 연결 여부를 저장하는 방식

### 3. 장단점

|구분| 인접 리스트  | 인접 행렬|
|---|---------|------|
|자료구조| 	ArrayList | 	2차원 배열 |
공간 복잡도	| O(V + E) | 	O(V²) |
연결 여부 확인	| 상대적으로 느림| 	O(1) |
연결된 정점 탐색| 	효율적    | 	O(V) |
일반적인 그래프 탐색	| 적합| 	정점 수가 작을 때 적합 |
- 일반적인 그래프 문제 → 인접 리스트
- 정점 수가 적고 연결 여부를 자주 확인 → 인접 행렬
- DFS, BFS → 보통 인접 리스트
---