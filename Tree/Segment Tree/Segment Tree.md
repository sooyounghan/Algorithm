-----
### 세그먼트 트리
-----
1. 주어진 데이터들의 구간 합과 데이터 업데이트를 빠르게 수행하기 위해 고안해 낸 자료구조의 형태
2. 더 큰 범위는 '인덱스 트리'라고 불리는데, 코딩 테스트 영역에서는 큰 차이가 없음
3. 세그먼트 트리 종류는 구간 합, 최대 · 최소 구하기로 나눌 수 있고, 구현 단계는 트리 초기화하기, 질의값 구하기(구간 합 또는 최대 · 최소), 데이터 업데이트하기로 나눌 수 있음
4. 트리 초기화하기
   - 리프 노드의 개수가 데이터의 개수(N) 이상이 되도록 트리 배열을 만듬
     + 트리 배열의 크기를 구하는 방법은 $2^{k}$ ≥ N을 만족하는 k의 최솟값을 구한 후, $2^{k}$ * 2를 트리 배열 크기로 정의하면 됨
     + 예를 들어, 다음과 같은 샘플 데이터가 있다면, N = 8이고, $2^{3}$ ≥ 8이므로 배열의 크기를 $2^{3}$ * 2 = 16으로 정의
<div align="center">
<img src="https://github.com/user-attachments/assets/e9fc3296-8077-479b-9a9c-c4f76fe591b7">
</div>

   - 리프 노드에 원본 데이터를 입력
     + 이 떄, 리프 노드의 시작 위치를 트리 배열의 인덱스로 구해야 하는데, 구하는 방식은 $2^{k}$을 시작 인덱스로 취하면 됨
     + 예를 들어, k = 3이면, start index = 8이 됨
<div align="center">
<img src="https://github.com/user-attachments/assets/e0e7e852-7f5d-4268-8159-d8439c9ef100">
</div>

   - 리프 노드를 제외한 나머지 노드의 값을 채움 ($2^{k} - 1$부터 1번 쪽으로 채움)
     + 채워야 하는 인덱스가 N이라고 가정하면, 자신의 자식 노드를 이용해 해당 값을 채울 수 있음
     + 자식 노드의 인덱스는 이진 트리 형식이므로 2N, 2N + 1이 됨 :  각 케이스별로 적절하게 계산해야 함
<div align="center">
<img src="https://github.com/user-attachments/assets/b90c0e26-09b1-430e-b36b-32aa1f3b2f2a">
</div>

   - 샘플을 이용해 3개의 케이스와 관련된 세그먼트 트리를 구성
<div align="center">
<img src="https://github.com/user-attachments/assets/ed0e0cf4-9934-46cc-9f61-0cc3ae7075e2">
</div>

   - 세그먼트 트리를 구성해 놓으면, 이후 질의와 관련된 결과값이나 데이터 업데이트 요구사항에 관해 좀 더 빠른 시간 복잡도 안에서 해결 가능

5. 질의값 구하기
   - 주어진 질의 인덱스를 세그먼트 트리의 리프 노드에 해당하는 인덱스로 변경
   - 기존 샘플을 기준으로 한 인덱스 값과 세그먼트 트리 배열에서 인덱스 값이 다르므로 인덱스를 변경해야 함
<div align="center">
<img src="https://github.com/user-attachments/assets/37fa96a6-30f4-48de-94dd-00174db9e540">
</div>

   - 질의에서의 시작 인덱스와 종료 인덱스에 관해 부모 노드로 이동하면서 주어진 질의에 해당하는 값을 구하는 과정
     + start_index % 2 == 1일 떄, 해당 노드 선택
     + end_index % 2 == 0일 때, 해당 노드 선택
     + start_index_depth 변경 : start_index = (start_index + 1) / 2 연산 실행
     + end_index_depth 변경 : end_index = (end_index + 1) / 2 연산 실행
     + 네 과정을 반복하다가 end_index < start_index가 되면 종료

   - 첫 번째와 두 번째 과정에서 해당 노드를 선택했다는 것은 해당 노드의 부모가 나타내는 범위가 질의 범위를 넘어가므로 해당 노드를 질의값에 영향을 미치는 독립 노드로 선택하고, 해당 노드의 부모 노드는 대상 범위에서 제외한다는 뜻
   - 부모 노드를 대상 범위에서 제거하는 방법은 세 번째와 네 번째 과정에서 질의 범위에 해당하는 부모 노드로 이동하기 위해 인덱스 연산을 index / 2가 아닌 (index + 1) / 2, (index - 1) / 2로 수행하는 것

   - 질의에 해당하는 노드를 선택하는 방법은 구간 합, 최댓값 구하기, 최솟값 구하기 모두 동일하며, 선택된 노드들에 관해 마지막 연산하는 방식만 다름
<div align="center">
<img src="https://github.com/user-attachments/assets/c7b8162f-610a-4fc1-8bf8-98d9966fa5d8">
</div>

6. 예) 트리 초기화하기에서 나온 구갑 함 샘플을 이용해 2 ~ 6번째 구간합을 구하는 간단한 예제
   - 먼저 리프 노드의 인덱스 변경
<div align="center">
<img src="https://github.com/user-attachments/assets/34b10aa7-63f4-44d6-85b8-427f4bfcc333">
</div>

   - 부모 노드로 이동
<div align="center">
<img src="https://github.com/user-attachments/assets/96157b15-92a3-44c1-a11e-d965347e4f19">
</div>

   - 한 번 더 부모 노드로 이동
<div align="center">
<img src="https://github.com/user-attachments/assets/88dc044f-b6ad-494f-9f53-55b18945b18a">
</div>

   - end_index < start_index이므로 종료하고 값을 구함 : 2 ~ 6번 구간 합의 값은 선택된 노드들의 합인 8 + 9 + 7 = 24

7. 데이터 업데이트하기
   - 업데이트 방식은 자신의 부모 노드로 이동하면서 업데이트하는 것은 동일하지만, 어떤 값으로 업데이트할 것인지에 관해서는 트리 타입별로 조금씩 다름
     + 부모 노드로 이동하는 방식은 세그먼트 트리가 이진 트리이므로 index = index / 2로 변경하면 됨

   - 구간 합 : 원래 데이터와 변경 데이터의 차이만큼 부모 노드로 올라가면서 변경
   - 최댓값 찾기 : 변경 데이터와 자신과 같은 부모를 지니고 있는 다른 자식 노드와 비교해 더 큰 값으로 업데이트
   - 최솟값 찾기 : 변경 데이터와 자신과 같은 부모를 지니고 있는 다른 자식 노드와 비교해 더 작은 값으로 업데이트
   - 5번 데이터 값을 7에서 10으로 업데이트 하는 예시 (5번 데이터 인덱스를 리프 노드 인덱스로 변경하면 5 + 7 = 12이므로 12번 노드의 값 업데이트)
<div align="center">
<img src="https://github.com/user-attachments/assets/c094e56b-48bb-4e82-8251-f572a5fcce3f">
</div>
