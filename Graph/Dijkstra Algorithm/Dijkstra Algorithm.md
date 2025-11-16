-----
### 다익스트라 알고리즘
-----
1. 그래프에서 최단 거리를 구하는 알고리즘
2. 특징
<div align="center">
<img src="https://github.com/user-attachments/assets/d8de7b8a-a622-4df8-9e5d-16dabd881a08">
</div>

3. 특정 노드에서 다른 노드들의 최단 거리를 구하는 문제가 주어졌을 때 사용하면 문제 해결 가능
4. 핵심
   - 인접 리스트로 그래프 구현하기 : 주어진 그래프를 인접 리스트로 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/b4dc7a56-939c-4ef6-b7a0-16ffcd88d6c6">
</div>

   - 다익스트라 알고리즘은 인접 행렬로 구현해도 좋지만, 시간 복잡도 측면과 N의 크기가 클 것을 대비해 인접 리스트를 선택해 구현하는 것이 좋음
   - 그래프 연결을 표현하기 위해 인접 리스트에 연결한 배열의 자료형은 (노드, 가중치)와 같은 형태 사용

   - 최단 거리 배열 초기화하기 : 최단 거리 배열을 만들고, 출발 노드는 0, 이외는 노드는 무한(∞)으로 초기화
     + 이 때 무한은 적당히 큰 값 사용 (실제 구현 시, 아주 큰 값, 에를 들어 99,999,999 정도로 설정)
<div align="center">
<img src="https://github.com/user-attachments/assets/55e55312-9737-40fa-bc25-7f8d2891d6e1">
</div>

   - 값이 가장 작은 노드 고르기 : 최단 거리 배열에서 현재 값이 가장 노드를 고름 (여기서는 값이 0인 출발 노드에서 시작)
<div align="center">
<img src="https://github.com/user-attachments/assets/c5081d89-c7d5-4a67-918b-7281c8b739da">
</div>

   - 최단 거리 배열 업데이트하기 : 선택된 노드에 연결된 에지의 값을 바탕으로 다른 노드의 값을 업데이트
     + 첫 번쨰 단게에서 저장해 놓은 연결 리스트를 이용해 현재 선택된 노드의 에지들을 탐색하고 업데이트
     + 연결 노드의 최단 거리는 다음과 같이 두 값 중 더 작은 값으로 업데이트 : Min(선택 노드의 최단 거리 배열의 값 + 에지 가중치, 연결 노드의 최단 거리 배열의 값)
<div align="center">
<img src="https://github.com/user-attachments/assets/62f5f6a0-c629-40a2-ba20-d65ca7a1ada3">
</div>

   - 3 ~ 4번째 과정을 반복해 최단 거리 배열 완성하기 : 모든 노드가 처리될 때까지 반복
     + 4번째 과정에서 선택되었던 노드가 다시 선택되지 않도록 방문 배열을 만들어 처리하고, 모든 노드가 선택될 때까지 반복하면 최단 거리 배열이 완성
<div align="center">
<img src="https://github.com/user-attachments/assets/8cc4aef0-5701-4f99-94e4-755e5f864ea1">
</div>

5. 정리
   - 다익스트라 알고리즘은 출발 노드와 그 외 노드 간 최단 거리를 구하는 알고리즘
   - 에지는 항상 양수여야 한다는 제약 조건 존재
   - 💡 실제 완성된 배열은 출발 노드와 이 외의 모든 노드 간의 최단 거리 (출발 노드와 도착 노드 간의 최단 거리가 아님)
   
