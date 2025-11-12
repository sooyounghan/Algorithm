-----
### 스택과 큐
-----
1. 배열에서 발전된 형태의 자료 구조
2. 스택 (Stack)
   - 삽입과 삭제 연산이 후입선출(LIFO : Last-In First-Out)로 이뤄진 자료구조로, 후입 선출은 삽입과 삭제가 한 쪽에서만 일어나는 특징이 존재
<div align="center">
<img src="https://github.com/user-attachments/assets/3b69d0b0-5cb7-430e-9a08-dc557f6c6b99">
</div>

   - 새 값이 들어가면 top이 새 값을 가리킴
   - 스택에서 값을 빼낼 때 pop은 top이 가리키는 값을 스택에서 빼게 되어 있으므로 결과적으로 가장 마지막에 넣었던 값이 나오게 됨
   - 스택 용어
      + 위치 - top : 삽입과 삭제가 일어나는 위치를 뜻함
      + 연산
        * push : top 위치에 새로운 데이터 삽입 연산
        * pop : top 위치에 현재 있는 데이터를 삭제하고 확인하는 연산
        * peek : top 위치에 현재 있는 데이터를 단순 확인하는 연산

   - 깊이 우선 탐색(DFS : Depth First Search)과 백트래킹 종류의 코딩 테스트에 효과적
   - 후입선출은 개념 자체가 재귀 함수 알고리즘 원리와 일맥상통함

3. 큐 (Queue)
   - 삽입과 삭제 연산이 선입선출(FIFO : First-In First-Out)로 이뤄지는 자료구조
   - 스택과 다르게 먼저 데이터가 먼저 나가므로, 삽입과 삭제가 양 방향으로 이뤄짐
<div align="center">
<img src="https://github.com/user-attachments/assets/5418a745-dc9a-4906-8701-a3b130d64644">
</div>

   - 새 값 추가는 큐의 rear에서 이뤄지고, 삭제는 큐의 front에서 이뤄짐
   - 큐 용어
     + rear : 큐에서 가장 끝 데이터를 가리키는 영역
     + front : 큐에서 가장 앞의 데이터를 가리키는 영역
     + add : rear 부분에 새로운 데이터를 삽입하는 연산
     + poll : front 부분에 있는 데이터를 삭제하고 확인하는 연산
     + peek : 큐의 맨 앞(front)에 있는 데이터를 확인할 때 사용하는 연산

   - 큐는 너비 우선 탐색(BPS : Breadth First Search)에 자주 사용

4. 우선순위 큐 (Priority Queue)
   - 값이 들어간 순서와 상관 없이 우선순위가 높은 데이터가 먼저 나오는 자료 구조
   - 큐 설정에 따라 front에 따라 항상 최댓값 또는 최솟값이 위치
   - 일반적으로 힙(Heap)을 이용해 구현하는데, 힙은 트리 종류 중 하나
