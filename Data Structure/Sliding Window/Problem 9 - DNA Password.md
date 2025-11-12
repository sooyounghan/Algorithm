-----
문제 9 - DNA 비밀번호 (문제 12891번)
-----
1. 문제
```
평소 문자열을 이용해 노는 것을 좋아하는 민호는 DNA 문자열을 알게 됐다. DNA 문자열은 모든 문자열에 등장하는 문자가 {'A', 'C', 'G', 'T'}인
문자열을 말한다. 예를 들어, "ACKA"는 DNA 문자열은 아니지만, "ACCA"는 DNA 문자열이다. 이런 신비한 문자열에 완전히 매료된 민호는 임의의
DNA 문자열을 만들고 이렇게 만든 DNA 문자열의 부분 문자열을 비밀번호로 사용하기로 마음먹었다.

하지만 민호는 이 방법에는 큰 문제가 있다는 것을 발견했다. 임의의 DNA 문자열의 부분 문자열을 뽑았을 때 "AAAA"와 같이 보안에 취약한
비밀번호가 만들어질 수 있기 때문이다. 그래서 민호는 부분 문자열에 등장하는 각 문자의 개수가 특정 개수 이상이어야 비밀번호를 사용할 수
있다는 규칙을 만들었다.

예를 들어 임의의 DNA 문자열이 "AAACCTGCCAA"이고, 민호가 뽑을 문자열의 길이를 4라고 가정해보자. 그리고 부분 문자열에는 'A'는 1개 이상,
'C'는 1개 이상, 'G'는 1개 이상, 'T'는 0개 이상 등장해야 비밀번호를 사용할 수 있다고 가정해보자. 이 때, 'ACCT'는 'G'가 1개 이상 등장해야
한다는 조건을 만족하지 못하므로 비밀번호를 상요할 수 없지만, 'GCCA'는 모든 조건을 만족하므로 비밀번호로 사용할 수 있다.

민호가 만든 임의의 DNA 문자열과 비밀번호로 사용할 부분 문자열의 길이 그리고 {'A', 'C', 'G', 'T'}가 각각 몇 번 이상 등장해야 비밀번호로
사용할 수 있는지, 순서대로 주어졌을 때 민호가 만들 수 있는 비밀번호의 가짓수를 구하는 프로그램을 작성하시오.

단, 부분 문자열이 등장하는 위치가 다르면 부분 문자열의 내용이 같더라도 다른 문자열로 취급한다.
```

2. 입력
   - 1번째 줄에 민호가 임의로 만든 DNA 문자열의 길이 |S|와 비밀번호로 사용할 부분 문자열의 길이 |P|가 주어짐 (1 ≤ |P| ≤ |S| ≤ 1,000,000)
   - 2번째 줄에 민호가 임의로 만든 DNA 문자열이 주어짐
   - 3번째 줄에 부분 문자열에 포함되어야 할 {'A', 'C', 'G', 'T'}의 최소 개수가 공백 문자를 사이에 두고 각각 주어짐
   - 각각의 수는 |S|보다 작거나, 같은 음이 아닌 정수로 총합은 |S|보다 작거나 같다는 것이 보장

3. 출력 : 민호가 만들 수 있는 비밀번호 가짓수 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/b7fff178-9eb1-4b8f-84d7-e4dcb30bd779">
</div>

4. 1단계: 문제 분석하기
   - P와 S의 길이가 1,000,000으로 매우 크기 때문에 O(n)의 시간 복잡도 알고리즘으로 문제를 해결해야 함
   - 이 때, 부분 문자열의 길이가 P이므로 슬라이딩 윈도우 개념을 이용하면 문제를 쉽게 해결 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/d8dc3364-6a83-4fa1-865b-c9ff624f1a10">
</div>

   - 길이가 P인 윈도우를 지정하여 배열 S의 시작점에 놓음
   - 그런 다음 윈도우를 오른쪽으로 밀면서 윈도우의 잡힌 값들이 조건에 맞는지 탐색
   - 배열 S의 길이만큼 탐색을 진행하면 O(n) 시간 복잡도로 문제 해결 가능

5. 2단계
   - S 배열과 비밀번호 체크 배열 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/31828c3c-dc60-4024-b842-2926cacb44d1">
</div>

   - 윈도우에 포함된 문자로 현재 배열 상태를 생성 : 그런 다음 현재 상태 배열과 비밀번호 체크 배열을 비교하여 유효한 비밀번호인지를 판단
<div align="center">
<img src="https://github.com/user-attachments/assets/9a863cea-9de6-4603-9540-fb6776eb3e6e">
</div>

   - 윈도우를 한 칸씩 이동하며 현재 상태 배열을 업데이트
     + 현재 상태 배열을 업데이트 한 이후에는 비밀번호 체크 배열과 비교하여 비밀번호 유효성을 판단
     + 현재 상태 배열을 업데이트 할 때는 빠지는 문자열, 신규 문자열만 확인하여 업데이트 하는 방식으로 진행
<div align="center">
<img src="https://github.com/user-attachments/assets/66931853-2e7f-411b-8a9d-7d9ccb16d3c2">
</div>

   - 윈도우를 한 칸 이동하여 C가 빠지고, G가 추가되어 현재 상태 배열을 1, 2, 2, 3에서 1, 1, 3, 3으로 업데이트 : 비밀번호 체크 배열과 비교했을 때 A가 2보다 작으니 유효한 비밀번호가 아님

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/72ea8270-f408-416f-b3b1-b9ee5a552e80">
</div>

   - 유효한 비밀번호를 검사할 때 기존 검사 결과에서 새로 들어온 문자열, 제거되는 문자열만 반영하여 확인하는 것이 핵심

7. 코드
```java
package dataStructure.slidingwindow;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class DNA {
    static int checkArr[];
    static int myArr[];
    static int checkSecret;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int S = Integer.parseInt(st.nextToken());
        int P = Integer.parseInt(st.nextToken());
        int Result = 0;

        char A[] = new char[S];
        checkArr = new int[4];
        myArr = new int[4];
        checkSecret = 0;

        A = br.readLine().toCharArray();

        st = new StringTokenizer(br.readLine());

        for (int i = 0; i < 4; i++) {
            checkArr[i] = Integer.parseInt(st.nextToken());
            if(checkArr[i] == 0) {
                checkSecret++;
            }
        }

        for (int i = 0; i < P; i++) { // 초기 P 부분 문자열 처리 부분
            Add(A[i]);
        }

        if (checkSecret == 4) {
            Result++;
        }

        // 슬라이딩 윈도우 처리 부분
        for (int i = P; i < S; i++) {
            int j = i - P;

            Add(A[i]);
            Remove(A[j]);

            if(checkSecret == 4) { // 4종류 문자의 개수 조건을 만족하면 유효한 비밀번호 
                Result++;
            }
        }

        System.out.println(Result);
        br.close();
    }

    private static void Add(char c) { // 새로 들어온 문자를 처리하는 함수
        switch(c) {
            case 'A':
                myArr[0]++;
                if (myArr[0] == checkArr[0]) {
                    checkSecret++;
                }
                break;

            case 'C':
                myArr[1]++;
                if (myArr[1] == checkArr[1]) {
                    checkSecret++;
                }
                break;

            case 'G':
                myArr[2]++;
                if (myArr[2] == checkArr[2]) {
                    checkSecret++;
                }
                break;

            case 'T':
                myArr[3]++;
                if (myArr[3] == checkArr[3]) {
                    checkSecret++;
                }
                break;
        }
    }

    private static void Remove(char c) { // 제거되는 문자를 처리하는 함수
        switch(c) {
            case 'A':
                if (myArr[0] == checkArr[0]) {
                    checkSecret--;
                }
                myArr[0]--;
                break;

            case 'C':
                if (myArr[1] == checkArr[1]) {
                    checkSecret--;
                }
                myArr[1]--;
                break;

            case 'G':
                if (myArr[2] == checkArr[2]) {
                    checkSecret++;
                }
                myArr[2]--;
                break;

            case 'T':
                if (myArr[3] == checkArr[3]) {
                    checkSecret++;
                }
                myArr[3]--;
                break;
        }
    }
}
```
