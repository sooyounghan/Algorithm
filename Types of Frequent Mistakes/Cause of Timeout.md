-----
### 시간 초과의 원인
-----
1. 코딩 테스트에서 가장 자주 마주치는 문제로, 입력과 출력 방식부터 최적화할 수 있는지 점검해야 함
   - 예를 들어 Scanner와 System.out.print 대신 BufferedReader와 BufferedWriter를 활용하면 처리 속도를 크게 향상 가능
   - 모든 문제에는 제한 시간이 있으며, 이 시간 안에 정답을 출력하지 못하면 시간 초과로 간주됨

2. 점검 사항
   - 풀이 로직의 시간 복잡도가 제한 시간 안에 문제를 해결할 수 있는 수준인지 확인 : 시간 복잡도가 너무 높다면 풀이 로직 자체를 변경해야 함
   - 입출력 최적화를 고려해야 함 : Scanner와 System.out.print가 항상 성능을 문제를 일으키는 것이 아님 (입출력 데이터 양이 많지 않다면, 두 방법이 BufferedReader, BufferedWriter와 큰 성능 차이를 보이지 않지만, 입출력 데이터 양이 많아질수록 성능 차이가 발생)
     + 1,000,000개의 정수를 입력받고 출력하는데 걸리는 시간을 비교한 표
<div align="center">
<img src="https://github.com/user-attachments/assets/badc90fc-6251-4ee1-a5f1-b8e7d8a6e8dc">
</div>

   - Scanner와 System.out.print는 데이터를 간편하게 입력받고 출력할 수 있어 코드를 작성할 때 자주 사용하는 방법 : 하지만 입출력이 대량으로 필요한 문제에서 이 방법은 시간 초과를 유발할 수 있으므로 조심해야 함

3. 성능 차이 발생 이유
   - Scanner는 입력할 때마다 필요한 자료형으로 변환하는 과정을 거치므로 처리 속도가 느려질 수 있음
   - System.out.print는 출력이 발생할 때마다 버퍼를 지우는 작업이 이뤄지므로 성능 저하 가능
   - BufferedReader는 입력을 버퍼에 저장한 후 데이터를 한 번 읽어오는 방식으로 I/O 작업 횟수를 줄여 성능을 향상
   - BufferedWriter도 마찬가지로, 출력할 데이터를 먼저 버퍼에 저장한 후 한 번에 출력하는 방식으로 성능 개선
     + write() 함수로 데이터를 버퍼에 추가하고, flush() 함수로 버퍼의 내용을 한꺼번에 출력함으로써 효율을 높임

4. 따라서, 대량의 데이터를 처리할 때는 BufferedReader와 BufferedWriter를 사용하는 것이 더 효과적
```java
package frequentMistake;

import java.io.*;
import java.util.Scanner;

public class timeout {
    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);

        int a = sc.nextInt();
        System.out.println(a);

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int b = Integer.parseInt(br.readLine());
        bw.write(String.valueOf(b)); // BufferedWriter는 정수는 String으로 변환하여 출력
        bw.flush();

    }
}
```
   - 실행 결과
```
1
2
1
2
```
