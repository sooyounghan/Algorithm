-----
### 위상 정렬
-----
1. 사이클이 없는 방향 그래프에서 노드 순서를 찾는 알고리즘
<div align="center">
<img src="https://github.com/user-attachments/assets/53bec65c-c2ff-4a67-8725-e2a2bb89e489">
</div>

2. 위상 정렬에서는 항상 유일한 값으로 정렬되지 않음
3. 또한 사이클이 존재하면 노드 간의 순서를 명확하게 정의할 수 없으므로 위상 정렬을 적용할 수 없음
4. 위상 정렬의 원리
   - 진입 차수(In-Degree) : 자기 자신을 가리키는 엣지
     + ArrayList를 그래프로 표현 (그래프는 사이클이 없는 상태)
<div align="center">
<img src="https://github.com/user-attachments/assets/a014e966-496f-4ef1-a5dd-046424019a30">
</div>

   - 진입 차수 배열 D를 다음과 같이 업데이트 : 1에서 2, 3을 가리키고 있으므로 D[2], D[3]을 각 1만큼 증가시킨 배열
<div align="center">
<img src="https://github.com/user-attachments/assets/d7123188-ea15-4b35-99c6-502a16b99379">
</div>

   - 진입 차수 배열에서 진입 차수가 0인 노드를 선택하고, 선택된 노드를 정렬 배열에 저장 : 그 후 인접 리스트에서 선택된 노드가 가리키는 노드들의 진입 차수를 1씩 뺌
<div align="center">
<img src="https://github.com/user-attachments/assets/fc3269fe-0cf1-40cc-a9f8-83ba07fb433d">
</div>

   - 위 그림의 경우 진입 차수가 0인 노드 1을 선택하여, 2, 3의 진입 차수를 1씩 빼 D[2], D[3]을 0으로 만든 것
     + 계속해서 다음 노드 2를 선택하여 반복
     + 이 과정을 모든 노드가 정렬될 때까지 반복
   - 여기서 진입 차수가 0인 노드를 3을 먼저 선택했다면, 3이 우선 위상 정렬 배열에 들어갈 것 (위상 정렬은 늘 같은 정렬 결과를 보장하지 않음)
<div align="center">
<img src="https://github.com/user-attachments/assets/bb7ac807-7b85-4746-96ad-10e52462eca0">
</div>

   - 위상 정렬 배열 결과
<div align="center">
<img src="https://github.com/user-attachments/assets/6e94d8f8-b074-452e-8f3a-2a165e70db6b">
</div>
