-----
### 구간 합
-----
1. 합 배열을 이용해 시간 복잡도를 더 줄이기 위해 사용하는 특수 목적 알고리즘
2. 구간 합의 핵심 이론
   - 합 배열을 구하기 : 배열 A가 있을 때, 합 배열 S
<div align="center">
<img src="https://github.com/user-attachments/assets/786b5573-d8ef-4868-b479-cb1efc625d51">
</div>

   - 합 배열은 기존 배열을 전처리한 배열
     + 이렇게 합 배열을 미리 구해 놓으면 기존 배열의 일정 범위합을 구하는 시간 복잡도가 O(N)에서 O(1)로 감소
<div align="center">
<img src="https://github.com/user-attachments/assets/153d5483-ce63-437a-b147-a357d4a62aa9">
</div>

   - A[i]부터 A[j]까지 배열 합을 합 배열 없이 구하는 경우, 최악의 경우는 i가 0이고, j가 N인 경우로 시간 복잡도는 O(N)
   - 합 배열을 이용하면 O(1)안에 도출 가능
   - 합 배열 공식
<div align="center">
<img src="https://github.com/user-attachments/assets/883fafcf-51d3-48f6-bdca-9f47fb530a50">
</div>

   - 합 배열을 이용하여 구간 합을 구하는 방법 : i에서 j까지 구간 합을 구하는 공식
<div align="center">
<img src="https://github.com/user-attachments/assets/ebbb73ea-8dcc-4313-86c3-f84ecaea3c92">
</div>

   - 예) 배열 A의 A[2]에서부터 A[5]까지 구간 합을 합 배열을 통해 구하는 과정
<div align="center">
<img src="https://github.com/user-attachments/assets/95f52627-fb42-42de-a747-de1e0aae0b21">
</div>

   - 합 배열과 구간 합은 연관되어 있음 : A[0] + ... + A[5]에서 A[0] + A[1]을 빼면, A[2] + ... + A[5]가 나오므로, S[5]에서 S[1]을 빼면 구간 합을 쉽게 구할 수 있음
   - 합 배열만 미리 구해 두면 구간 합은 한 번의 계산 만으로 구할 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/2524f493-106c-45ae-b1e7-c89e335dd9b9">
</div>
