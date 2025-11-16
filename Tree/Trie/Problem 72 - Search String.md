-----
### 문제 72 - 문자열 찾기 (문제 14425번)
-----
1. 문제
```
총 N개의 문자열로 이뤄진 집합 S가 있다.
입력으로 주어지는 M개의 문자열 중 집합 S에 포함되어 있는 것이 총 몇 개인지 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 문자열의 개수 N과 M(1 ≤ N ≤ 10,000, 1 ≤ M ≤ 10,000)이 주어짐
   - 그 다음 N개의 줄에는 집합 S가 포함되어 있는 문자열이 주어지고, 그 다음 M개의 줄에는 검사해야 하는 문자열이 주어짐
   - 입력으로 주어지는 문자열은 알파벳 소문자로만 이뤄져 있으며, 길이는 500을 넘지 않음
   - 집합 S에 같은 문자열이 여러 번 주어지는 경우는 없음

3. 출력 : 1번쨰 줄에 M개의 문자열 중 총 몇 개가 집합 S에 포함되어있는지 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/d128abad-1c60-4f48-b25e-7a1c565d17b8">
</div>

4. 1단계 : 문제 분석하기
   - 집합 S에 속해있는 단어들을 이용해 트라이 구조를 생성하고, 트라이 검색을 이용해 문자열 M개의 포함 여부를 카운트하는 전형적 트라이 자료구조 문제

5. 2단계
   - 트라이 자료구조 생성 : 현재 문자열을 가리키는 위치의 노드가 공백 상태라면 신규 노드를 생성하고, 아니라면 이동하며 문자열의 마지막에 도달하면 리프 노드라고 표시
<div align="center">
<img src="https://github.com/user-attachments/assets/a48c65cb-9da9-40d4-ae1d-4af4792bb6cc">
</div>

   - 트라이 자료구조 검색으로 집합 S에 포함된 문자열을 셈 : 부모-자식 관계 구조를 이용해 대상 문자열을 검색했을 때, 문자열이 끝날 때까지 공백 상태가 없고, 현재 문자의 마지막 노드가 트라이의 리프 노드라면 이 문자를 집합 S에 포함된 문자열로 셈
<div align="center">
<img src="https://github.com/user-attachments/assets/880e6cd1-aad5-43f3-9442-fe3689908712">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/c3ff4442-01ce-4624-b21a-22bb0652ca03">
</div>

7. 코드
```java
package tree.trie;

import java.util.Scanner;

public class SearchString {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int m = sc.nextInt();

        tNode root = new tNode();

        while(n > 0) { // 트라이 자료구조 구축하기
            String text = sc.next();

            tNode now = root;

            for(int i = 0; i < text.length(); i++) {
                char c = text.charAt(i);

                // 26개의 알파벳 위치를 배열 index로 나타내기 위해 - 'a' 연산 수행
                if(now.next[c - 'a'] == null) {
                    now.next[c - 'a'] = new tNode();
                }

                now = now.next[c - 'a'];
                if (i == text.length() - 1) {
                    now.isEnd = true;
                }
            }

            n--;
        }

        int count = 0;
        while(m > 0) { // 트라이 자료구조 검색하기
            String text = sc.next();

            tNode now = root;

            for(int i = 0; i < text.length(); i++) {
                char c = text.charAt(i);

                if(now.next[c - 'a'] == null) { // 공백 노드라면 이 문자열을 포함하지 않음
                    break;
                }

                now = now.next[c - 'a'];
                if(i == text.length() - 1 && now.isEnd) { // 문자열의 끝이고 현재까지 모두 일치하면
                    count++; // S 집합에 포함되는 문자열
                }
            }

            m--;
        }

        System.out.println(count);
    }
}

class tNode {
    tNode[] next = new tNode[26];
    boolean isEnd; // 문자열의 마지막 여부 표시
}
```
