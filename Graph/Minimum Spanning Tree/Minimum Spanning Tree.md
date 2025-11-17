-----
### 최소 신장 트리
-----
1. 모든 노드를 연결할 때 사용된 에지들의 가중치의 합을 최소로 하는 트리
2. 특징
   - 사이클이 포함되면, 가중치의 합이 최소가 될 수 없으므로 사이클을 포함하지 않음
   - N개의 노드가 있으면 최소 신장 트리를 구성하는 엣지의 개수는 항상 N - 1개

3. 작동 원리
   - 엣지 리스트로 그래프를 구현하고 유니온 파인드 배열 초기화하기
     + 최소 신장 트리는 데이터를 노드가 아닌 엣지 중심으로 저장하므로 인접 리스트가 아닌 엣지 리스트 형태로 저장
       + edge 클래스는 일반적으로 노드 변수 2개와 가중치 변수로 구성
       + 사이클 처리를 위한 유니온 파인드 배열도 함께 초기화
       + 배열의 인덱스로 해당 자리의 값을 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/2d556ad9-6946-4001-bfea-c9e0a1aa4212">
</div>

   - 그래프 데이터를 가중치 기준으로 정렬하기 : 엣지 리스트에 담긴 그래프 데이터를 가중치 기준으로 오름차순 정렬
<div align="center">
<img src="https://github.com/user-attachments/assets/6725b19e-d791-4695-8963-a458fb47a305">
</div>

   - 엣지 리스트의 1개 객체를 클래스로 표현하면 Comparable() 함수를 사용해 높은 자유도로 정렬 수행 가능

   - 가중치가 낮은 엣지부터 연결 시도 : 이 떄, 바로 연결하지 않고 이 엣지를 연결했을 때 그래프에 사이클 형성 여부를 find 연산을 이용해 확인한 후 사이클이 형성되지 않을 때만 union 연산을 이용해 두 노드 연결
<div align="center">
<img src="https://github.com/user-attachments/assets/fcc2795f-7ff1-4242-a786-decc0b7af142">
</div>

   - 위 과정 반복하기 : 전체 노드 개수가 N개이면, 연결한 엣지의 개수가 N - 1이 될떄까지 반복
<div align="center">
<img src="https://github.com/user-attachments/assets/ed6ac4f0-5006-48f7-a849-f5a4bcedae30">
</div>

   - 총 엣지 비용 출력하기 : 엣지 개수가 N - 1이 되면, 알고리즘을 종료하고, 완성된 최소 신장 트리의 총 엣지 비용을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/1f6849eb-ba6d-42de-9187-edea66f0d3f6">
</div>

4. 최소 신장 트리는 엣지 리스트 형태를 이용해 데이터를 담는 특징이 존재
   - 그 이유는 엣지를 기준으로 하는 알고리즘이기 때문임
   - 또한, 사이클이 존재하면 안 되는 특징을 가지고 있어 사이클 판별 알고리즘인 유니온 파인드 알고리즘을 내부에서 구현해야 함
