-----
### 숫자의 합 (11720번)
-----
1. N개의 숫자가 공백 없이 쓰여 있는데, 이 숫자를 모두 합해 출력하는 프로그램을 작성
2. 입력 : 1번째 줄에 숫자의 개수 N(1 ≤ N ≤ 100), 2번째 줄에 숫자 N개가 공백 업이 주어짐
3. 출력 : 입력으로 주어진 숫자 N개의 합을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/edc18e53-c9d4-406b-b785-afcc3bcd2308">
</div>

4. 1단계 : 문제 분석하기
   - N의 범위가 1부터 100까지이므로 int 형, long 형과 같은 숫자형으로 담을 수 없음
     + 먼저 문자열 형태로 입력값을 받은 후 이를 문자 배열로 변환하고, 문자 배열 값을 순서대로 읽으면서 숫자형으로 변환하여 더해야함
     + 예를 들어, 입력값 "1234"와 같이 문자열로 입력받은 후 이를 다시 '1', '2', '3', '4'와 같은 이 문자 배열로 변환하고, 다시 문자 배열을 1, 2, 3, 4로 변환한 다음 더해 10을 구함
     + 문자열을 숫자형으로 변경하려면 아스키 코드를 이해해야 하는데, 아스키코드에서 같은 의미와 문자와 숫자의 코드 값 차이는 48 (예를 들어, 문자 '1'은 아스키코드 값이 49이므로, 문자 '1'을 숫자 1로 변환하려면 '1' - 48 또는 '1' - '0'과 같이 연산하면 됨)

5. 2단계
   - 숫자의 개수만큼 입력받은 값을 String 형으로 저장
```java
String sNum = 10987654321;
```
   - String 형으로 입력받은 값을 char[] 형으로 변환
<div align="center">
<img src="https://github.com/user-attachments/assets/2d4660e3-9af7-44a1-a91a-c823e9434e53">
</div>

   - 인덱스 0부터 끝까지 배열을 탐색하며 각 값을 정수형으로 변환하고 결과값을 더하여 누적
<div align="center">
<img src="https://github.com/user-attachments/assets/7534d38f-9c44-4fcd-9f51-b25b46fc5e3e">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/46a84d20-faa3-46f2-8a55-00f40e90c429">
</div>

7. 4단계 : 코드 구현하기
```java
package dataStructure.array;

import java.util.Scanner;

public class SumNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();

        // 입력값을 String형 변수 sNum에 저장한 후, char[]형 변수로 변환
        String sNum = sc.next();
        char[] cNum = sNum.toCharArray();

        int sum = 0;

        for (int i = 0; i < cNum.length; i++) {
            sum += cNum[i] - '0'; // cNum[i]을 정수형으로 변환하면서 sum에 더하여 누적
        }

        System.out.println(sum);
    }
}
```

8. 자바에서의 형 변환
   - String 형 → 숫자형 (int, double, float, long, short)
```java
String sNum = "1234";
int i1 = Integer.parseInt(sNum);
int i2 = Integer.valueOf(sNum);

double d1 = Double.parseDouble(sNum);
double d2 = Double.valueOf(sNum);

float f1 = Float.parseFloat(sNum);
float f2 = Float.valueOf(sNum);

long l1 = Long.parseLong(sNum);
long l2 = Long.valueOf(sNum);

short s1 = Short.parseShort(sNum);
short s2 = Short.valueOf(sNum);
```

   - 숫자형 (int, double, float, long, short) → String 형
```java
int i = 1234;
String i11 = String.valueOf(i);
String i12 = Integer.toString(i);

double d = 1.23;
String d11 = String.valueOf(d);
String d12 = Double.toString(d);

float f = (float) 1.23;
String f11 = String.valueOf(f);
String f12 = Float.toString(f);

long l = 1234L;
String l11 = String.valueOf(l);
String l12 = Long.toString(l);

short s = 1234;
String s11 = String.valueOf(s);
String s12 = Integer.toString(s);
```
