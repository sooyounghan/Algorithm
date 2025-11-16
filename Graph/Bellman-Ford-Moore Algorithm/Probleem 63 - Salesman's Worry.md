-----
### 문제 63 - 세일즈맨의 고민 (문제 1219번)
-----
1. 문제
```
오민식은 세일즈맨이다. 오민식의 나라에는 N개의 도시가 있다. 도시는 0번부터 N - 1번까지 번호가 매겨져있다.
오민식은 A 도시에서 시작해 B 도시에서 끝나는 출장으로 최대한 많은 돈을 벌고 싶다.

오민식이 이용할 수 있는 교통수단에는 여러 가지가 있다.
오민식은 모든 교통수단의 출발 도시와 도착 도시를 알고 있고, 비용도 알고 있다.
더욱이 오민식은 각각의 도시를 방문할 떄마다 벌 수 있는 돈을 알고 있다.
해당 값은 도시마다 다르고, 액수는 고정되어 있다.

같은 도시를 여러 번 방문할 수 있고, 도시를 방문할 때마다 그 돈을 벌어오게 된다.
오민식이 버는 돈보다 쓰는 돈이 많다면 도착 도시에 도착할 때 지니고 있는 돈의 액수가 음수가 될 수도 있다.

모든 교통 수단은 입력으로 주어진 방향으로만 이용할 수 있고, 여러 번 이용할 수도 있다.

오민식이 도착 도시에 도착할 때 지니고 있는 돈의 액수를 최대로 하려고 한다.

이 최대값을 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 도시의 수 N과 시작 도시, 도착 도시 그리고 교통 수단의 개수 M이 주어짐
   - 2번째 줄부터 M개의 줄에는 교통 수단의 정보가 주어짐 : 교통 수단의 정보는 '시작 끝 가격'과 같은 형식
   - 마지막 줄에는 오민식이 각 도시에서 벌 수 있는 돈의 최댓값이 0번 도시부터 차례대로 주어짐
   - N과 M은 100보다 작거나 같고, 돈의 최대값과 교통 수단의 가격은 1,000,000보다 작거나 같은 음이 아닌 정수

3. 출력
   - 1번째 줄에 도착 도시에 도착할 때 지니고 있는 돈의 액수의 최댓값을 출력
   - 만약 오민식이 도착 도시에 도착할 수 없을 때는 'gg'를 출력
   - 그리고 오민식이 도착 도시에 도착했을 때, 돈을 무한히 많이 지니고 있을 수 있다면' Gee'를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/c313194c-5d61-4696-9799-a038adcadfcb">
</div>

4. 1단계 : 문제 분석하기
   - 벨만-포드 알고리즘의 원리를 바탕으로 요구사항에 따라 내부 로직을 바꿔야 하는 문제
   - 기존 벨만-포드는 최단 거리를 구하는 알고리즘이지만, 이 문제에서는 도착 도시에 도착할 때 돈의 액수를 최대로 하고 싶으므로, 업데이트 방식을 반대로 변경해야 함
   - 또한, 돈을 무한히 많이 버는 케이스가 있다고 하는 것을 바탕으로 음수 사이클이 아닌 양수 사이클을 찾도록 변경해야 함
   - 그리고 마지막 예외 처리 1개가 필요 : 양수 사이클이 있어도 출발 노드에서 이 양수 사이클을 이용해 도착 도시에 가지 못할 때
<div align="center">
<img src="https://github.com/user-attachments/assets/5c8514f9-afeb-4e2f-847d-5252b8760c17">
</div>

   - 이 부분을 해결할 수 있는 방법은 여러 가지가 존재하는데, 여기서는 엣지의 업데이트를 N - 1번이 아닌 충분히 큰 수(도시 개수 N의 최대치 = 100)만큼 최대로 돌리면서 업데이트를 수행하도록 로직을 변경해 해결
     + 이유는 엣지를 충분히 탐색하면서 양수 사이클에서 도달할 수 있는 모든 노드를 양수 사이클에 연결된 노드로 업데이트 하기 위함

5. 2단계
   - 엣지 리스트에 엣지 데이터를 저장하고, 거리 배열값을 초기화 : 최초 시작점에 해당하는 거리 배열값은 0으로 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/93391881-002b-4ec2-835e-26284f132b21">
</div>

   - 변형된 벨만-포드 알고리즘 수행
     + 모든 엣지와 관련된 정보를 가져와 다음 조건에 따라 거리 배열의 값 업데이트
       * 시작 도시가 방문한 적이 없는 도시일 때, (시작 도시 == MIN) 업데이트 하지 않음
       * 시작 도시가 양수 사이클과 연결된 도시일 때, (도착 도시 == MAX) 도착 도시도 양수 사이클과 연결된 도시로 업데이트
       * '도착 도시값 < 시작 도시값 + 도착 도시 수입 - 엣지 가중치'일 때, 더 많이 벌 수 있는 새로운 경로로 도착한 것이므로 값을 업데이트
     + 노드보다 충분히 많은 값(N + 100)으로 첫 번째 단계 반복
<div align="center">
<img src="https://github.com/user-attachments/assets/649a4d8f-c139-429f-bea9-247361d33f29">
</div>

   - 도착 도시의 값에 따라 결과를 출력 (결과 출력 경우의 수)
     + 도착 도시의 값이 MIN이고, 도착하지 못할 때 'gg'를 출력
     + 도착 도시의 값이 MAX이고, 무한히 많이 벌 수 있을 때 'Gee'를 출력
     + 이 외에는 도착 도시의 값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/cf9d86d6-912a-420b-805f-5e6b986fc868">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/17128f6f-784c-46aa-babc-6b9c7365f2bf">
</div>

7. 코드
```java
package graph.bellmanford.salesmanworry;

import java.io.*;
import java.util.Arrays;
import java.util.StringTokenizer;

public class SalesManWorry {
    private static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    private static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    public static int N, M, sCity, eCity;
    public static long distance[], cityMoney[];
    public static Edge edges[];

    public static void main(String[] args) throws IOException {
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        sCity = Integer.parseInt(st.nextToken());
        eCity = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        edges = new Edge[M];
        distance = new long[M];
        cityMoney = new long[M];

        Arrays.fill(distance, Long.MIN_VALUE); // 최단 거리 배열 초기화

        for(int i = 0; i < M; i++) { // 엣지 리스트에 데이터 저장
            st = new StringTokenizer(br.readLine());

            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());
            int price = Integer.parseInt(st.nextToken());

            edges[i] = new Edge(start, end, price);
        }

        st = new StringTokenizer(br.readLine());
        for(int i = 0; i < N; i++) {
            cityMoney[i] = Long.parseLong(st.nextToken());
        }

        distance[sCity] = cityMoney[sCity]; // 변형된 벨만-포드 알고리즘 수행

        // 양수 사이클이 전파되도록 충분히 큰 수로 반복
        for(int i = 0; i <= N + 100; i++) {
            for(int j = 0; j < M; j++) {
                int start = edges[j].start;
                int end = edges[j].end;
                int price = edges[j].price;

                // 출발 노드가 방문하지 않은 노드이면 Skip
                if (distance[start] == Long.MIN_VALUE) {
                    continue;
                } else if (distance[start] == Long.MAX_VALUE) { // 출발 노드가 양수 사이클에 연결된 노드라면 종료 노드도 연결 노드로 업데이트
                    distance[end] = Long.MAX_VALUE;
                } else if (distance[end] < distance[start] + cityMoney[end] - price) { // 더 많은 돈을 벌 수 있는 새로운 경로가 발견됐을 때, 새로운 경로 값으로 업데이트
                    distance[end] = distance[start] + cityMoney[end] - price;

                    // N - 1번 반복 이후 업데이트되는 종료 노드는 양수 사이클 연결 노드로 변경
                    if(i >= N - 1) distance[end] = Long.MAX_VALUE;
                }
            }
        }

        if(distance[eCity] == Long.MIN_VALUE) {
            System.out.println("gg");
        } else if(distance[eCity] == Long.MAX_VALUE) { // 양수 사이클에 연결되어 무한대 돈을 벌 수 있을 때
            System.out.println("Gee");
        } else {
            System.out.println(distance[eCity]);
        }
    }
}

class Edge { // 엣지 리스트를 편하게 다루기 위한 클래스 별도 구현
    int start, end, price;

    public Edge(int start, int end, int price) {
        this.start = start;
        this.end = end;
        this.price = price;
    }
}
```
