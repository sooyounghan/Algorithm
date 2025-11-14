-----
### 유클리드 호제법
-----
1. 두 수의 최대 공약수를 구하는 알고리즘
2. 일반적으로 최대 공약수를 구하는 방법은 소인수분해를 이용한 공통된 소수들의 곱으로 표현할 수 있지만, 유클리드 호제법은 좀 더 간단한 방법 제시
3. 유클리드 호제법을 수행하려면 먼저 MOD 연산을 이해하고 있어야 함 : MOD 연산이 최대 공약수를 구하는 데 핵심 연산
<div align="center">
<img src="https://github.com/user-attachments/assets/4602b5d7-700a-49bc-9264-2cbe233afc87">
</div>

4. MOD 연산으로 구현하는 유클리드 호제법
   - 큰 수를 작은 수로 나누느 MOD 연산 수행
   - 앞 단계에서의 작은 수와 MOD 연산 결과값(나머지)로 MOD 연산 수행
   - 나머지가 0이 되는 순간 작은 수를 최대 공약수로 선택

5. 예) 270과 192의 최대 공약수를 유클리드 호제법으로 찾기 (최대 공약수를 구하는 연산은 일반적으로 gcd로 정의)
<div align="center">
<img src="https://github.com/user-attachments/assets/ad25fbb1-360a-4cae-a450-c49f5e9c856e">
</div>
