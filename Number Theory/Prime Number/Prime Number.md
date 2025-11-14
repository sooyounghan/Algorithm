-----
### 소수 구하기
-----
1. 소수 (Prime Number) : 자신보다 작은 2개의 자연수를 곱해 만들 수 없는 1보다 큰 자연수
2. 즉, 1과 자기 자신 외에 약수가 존재하지 않는 수
3. 소수를 구하는 대표적인 판별법 : 에라토스테네스의 체 (원리)
   - 구하고자 하는 소수의 범위만큼 1차원 배열 생성
   - 2부터 시작해서 현재 선택한 숫자가 지워지지 않은 숫자라면, 해당 숫자의 배수에 해당하는 수를 배열에서 끝까지 탐색하면서 지움 (이 때, 처음으로 선택한 숫자는 지우지 않음)
   - 배열의 끝까지 2번 과정을 반복한 후, 배열에서 남아 있는 모든 수를 출력

4. 에라토스테네스의 체 원리 이해 : 1부터 30까지 수 중 소수를 구하는 예시
   - 먼저 주어진 범위까지 배열을 생성 : 1은 소수가 아니므로 삭제하고, 배열은 2부터 시작
<div align="center">
<img src="https://github.com/user-attachments/assets/c80c73b4-32cd-454d-bfa6-4a64196c8f3d">
</div>

   - 선택한 수의 배수를 모두 삭제 : 현재의 경우는 2의 배수를 모두 삭제
<div align="center">
<img src="https://github.com/user-attachments/assets/0accd2c9-9844-4436-b9fe-c9de28a8e635">
</div>

   - 다음 지워지지 않은 수를 선택 : 즉, 3을 선택하고 선택한 수의 모든 배수를 삭제 (이미 지운 수는 다시 지우지 않음)
<div align="center">
<img src="https://github.com/user-attachments/assets/f4b6c8da-0578-4a5d-bbe1-78799755c715">
</div>

   - 앞의 과정을 배열의 끝까지 반복
<div align="center">
<img src="https://github.com/user-attachments/assets/91fae6ad-4f49-46a0-b0d7-7f87bde34308">
</div>

   - 삭제되지 않은 수를 모두 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/d506bb58-be3e-42ec-9778-49e6ab227eed">
</div>

   - 즉, 1부터 30까지의 수 중 소수는 2, 3, 5, 7, 11, 13, 17, 19, 23, 29

5. 에라토스테네스의 체를 사용할 때 시간 복잡도
   - 일반적으로 에라토스테네스의 체를 구현하려면 이중 for문을 이용하므로 시간 복잡도가 O($N^{2}$) 정도로 판단 가능
   - 하지만, 실제 시간 복잡도는 최적화 정도에 따라 다르겠지만, 일반적으로 O(N log(log N))
     + 그 이유는 배수를 삭제하는 연산으로 실제 구현에서 바깥쪽 for문을 생략하는 경우가 빈번하게 발생하기 떄문임
     + 이러한 이유 떄문에 에라토스테네스 체 기법은 코딩 테스트에서 소수를 구하는 일반적 방법으로 통용
