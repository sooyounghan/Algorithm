-----
### 플로이드-워셜
-----
1. 그래프에서 최단 거리를 구하는 알고리즘
2. 특징
<div align="center">
<img src="https://github.com/user-attachments/assets/73816f83-1496-48b0-b24d-b9b8d31b0c2c">
</div>

3. 핵심 원리
   - A노드에서 B노드 까지 최단 경로를 구했다고 가정했을 때, 최단 경로 위에 K 노드가 존재한다면, 그것을 이루는 부분 경로 역시 최단 경로
<div align="center">
<img src="https://github.com/user-attachments/assets/892ee69c-93dd-49db-b73a-9509f9106eee">
</div>

   - 색칠된 엣지 경로가 1 → 5 최단 경로라면, 1 → 4 최단 경로와 4 → 5 최단 경로 역시 색칠된 에지로 이뤄질 수 밖에 없음
   - 즉, 전체 경로의 최단 경로는 부분 경로의 최단 경로의 조합으로 이뤄진다는 의미
   - 플로이드-워셜 점화식 : D[S][E] = Math.min(D[S][E], D[S][K] + D[K][E])

4. 구현 방법
   - 배열을 선언하고 초기화하기
     + D[S][E]는 노드 S에서 노드 E까지의 최단 거리를 저장하는 배열
     + S와 E의 값이 같은 칸은 0, 다른 칸은 ∞ 으로 초기화 (여기서 S == E는 자기 자신에게 가는 데 걸리는 최단 경로값)
<div align="center">
<img src="https://github.com/user-attachments/assets/fa1828c5-1d52-43a9-991b-49ea07ec3bfc">
</div>

   - 최단 거리 배열에 그래프 데이터 저장하기
     + 출발 노드는 S, 도착 노드는 E, 엣지의 가중치는 W라고 했을 때, D[S][E] = W로 엣지의 정보를 배열에 입력
     + 이로써, 플로이드-워셜 알고리즘은 그래프를 인접 행렬로 표현한다는 것을 알 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/4960caac-fd3c-46aa-8fd2-ec402f4bbbd8">
</div>

   - 점화식으로 배열 업데이트 : 점화식을 3중 for문의 형태로 반복하면서 배열의 값 업데이트
<div align="center">
<img src="https://github.com/user-attachments/assets/ce4b223f-8a3f-4363-a63d-6b0df869a761">
</div>

   - 완성된 배열은 모든 노드 간의 최단 거리를 알려줌 : 예를 들어, 1번 노드에서 5번 노드까지 가는 최단 거리는 D[1][5] = 6으로 나타낼 수 있음

5. 💡 플로이드-워셜 알고리즘은 모든 노드 간의 최단 거리를 확인하므로 시간 복잡도가 O($V^{3}$)으로 빠르지 않은 편
   - 따라서, 이를 사용해야 하는 문제가 나오면 일반적으로 노드 개수 범위가 다른 그래프에 비해 적게 나타나는 것을 알 수 있음
  
