-----
### 문제 68 - 다리 만들기 (문제 17472번)
-----
1. 문제
<div align="center">
<img src="https://github.com/user-attachments/assets/58c19e12-8aba-45da-a040-8c17a987bb00">
</div>

2. 입력
   - 1번째 줄에 지도의 세로 크기 N과 가로 크기 M이 주어짐 (1 ≤ N, M ≤ 10, 3 ≤ N X M ≤ 100)
   - 2번쨰 줄부터 N개의 줄에 지도 정보가 주어짐 : 각 줄은 M개의 수로 이루어져 있고, 수는 0 또는 1 (0은 바다, 1은 땅을 의미)
   - 섬의 개수는 2 이상 6 이하임

3. 출력 : 모든 섬을 연결하는 다리 길이의 최솟값을 출력, 모든 섬을 연결할 수 없다면 -1을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/161de155-71db-45b9-a363-1fcddce87e12">
</div>

4. 1단계 : 문제 분석하기
   - 데이터의 크기는 매우 작은 편이라 시간 복잡도 제약은 크지 않음
   - 단, 몇 개의 단계로 나눠 생각해야 함
     + 먼저 주어진 지도에서 섬으로 표현된 각각의 섬마다 다르게 표현해야 함
     + 그 이후 각 섬의 모든 위치에서 다른 섬으로 연결할 수 있는 엣지가 있는지 확인해 엣지 리스트를 만듬
     + 이후에는 최소 최소 신장 트리를 적용하면 문제를 해결할 수 있음

5. 2단계
   - 지도의 정보를 2차원 배열에 저장하고, 섬으로 표시된 모든 점에서 BFS를 실행해 섬을 구분 (상 / 하 / 좌 / 우 네 방향으로 탐색) : 방문한 적이 없고, 바다가 아닐 때는 같은 섬으로 인식
<div align="center">
<img src="https://github.com/user-attachments/assets/f4cc19f9-2c55-4219-aeac-a92474702f3c">
</div>

   - 모든 섬에서 상하좌우 다리를 지어 다른 섬으로 연결할 수 있는지 확인
     + 연결할 곳이 현재 섬이면 탐색 중단, 바다라면 계속 수행
     + 다른 섬을 만났을 떄, 다리 길이가 2 이상이면 이 다리를 엣지 리스트에 추가
<div align="center">
<img src="https://github.com/user-attachments/assets/d7a97bbe-2b87-49ef-bdb1-233a16ef28d3">
</div>

   - 전 단계에서 수집한 모든 엣지를 오름차순으로 정렬해 최소 신장 트리 알고리즘 수행 : 알고리즘이 끝나면 사용한 엣지의 합 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/0842086a-8a5e-4580-96a9-00d68b957e36">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/2c4ce858-33d5-47f6-a111-54f91aab4b6c">
</div>

7. 코드
```java
package graph.mst;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class MakeBridge {
    public static int[] dr = { -1, 0, 1, 0 }; // 좌, 우
    public static int[] dc = { 0, 1, 0, -1 }; // 상, 하

    public static int[] parent;
    public static int[][] map;

    public static int N, M, sNum;
    public static boolean[][] visited;
    public static ArrayList<ArrayList<int[]>> sumList;
    public static ArrayList<int[]> mList;
    public static PriorityQueue<bEdge> queue;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken()); // 가로 크기
        M = Integer.parseInt(st.nextToken()); // 세로 크기

        map = new int[N][M];
        visited = new boolean[N][M];

        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < M; j++) {
                map[i][j] = Integer.parseInt(st.nextToken()); // 맵 정보 저장
            }
        }

        sNum = 1;
        sumList = new ArrayList<>();

        for (int i = 0; i < N; i++) { // 각 자리에서 BFS 탐색으로 섬들 분리
            for (int j = 0; j < M; j++) {
                if(map[i][j] != 0 && visited[i][j] != true) {
                    BFS(i, j);
                    sNum++;
                    sumList.add(mList);
                }
            }
        }

        queue = new PriorityQueue<>();

        // 섬의 각 지점에서 만들 수 있는 모든 엣지 저장
        for (int i = 0; i < sumList.size(); i++) {
            ArrayList<int[]> now = sumList.get(i);

            for(int j = 0; j < now.size(); j++) {
                int r = now.get(j)[0];
                int c = now.get(j)[1];

                int now_S = map[r][c];

                for(int d = 0; d < 4; d++) { // 네 방향 검색하기
                    int tempR = dr[d];
                    int tempC = dc[d];
                    int blength = 0;

                    while(r + tempR >= 0 && r + tempR < N && c + tempC >= 0 && c + tempC < M) {
                        if (map[r + tempR][c + tempC] == now_S) { // 같은 섬이면 엣지를 만들 수 없음
                            break;
                        } else if (map[r + tempR][c + tempC] != 0) { // 같은 섬이 아니고 바다도 아니면,
                            if (blength > 1) { // 다른 섬 : 길이가 1 초과할 때, 엣지로 더하기
                                queue.add(new bEdge(now_S, map[r + tempR][c + tempC], blength));
                            }
                            break;
                        } else { // 바다이면 다리 길이 연장
                            blength++;
                        }

                        if (tempR < 0) {
                            tempR--;
                        } else if (tempR > 0) {
                            tempR++;
                        } else if (tempC < 0) {
                            tempC--;
                        } else if (tempC > 0) {
                            tempC++;
                        }
                    }
                }
            }
        }
        parent = new int[sNum];
        for (int i = 0; i < parent.length; i++) {
            parent[i] = i;
        }

        int useEdge = 0;
        int result = 0;

        while(!queue.isEmpty()) { // 최소 신장 트리 알고리즘 수행
            bEdge now = queue.poll();

            if(find(now.s) != find(now.e)) { // 같은 부모가 아니면 연결}
                union(now.s, now.e);
                result = result + now.v;
                useEdge++;
            }
        }

        if(useEdge == sNum - 2) {
            System.out.println(result);
        } else {
            System.out.println(-1);
        }
    }

    private static void union(int a, int b) { // union 연산 : 대표 노드끼리 연결하기
        a = find(a);
        b = find(b);

        if (a != b) {
            parent[b] = a;
        }
    }

    private static int find(int a) { // find 연산
        if (a == parent[a]) {
            return a;
        } else {
            return parent[a] = find(parent[a]);
        }
    }


    private static void BFS(int i, int j) {
        Queue<int[]> queue = new LinkedList<>();
        mList = new ArrayList<>();
        int[] start = { i, j };

        queue.add(start);
        mList.add(start);

        visited[i][j] = true;
        map[i][j] = sNum;

        while(!queue.isEmpty()) {
            int[] now = queue.poll();

            int r = now[0];
            int c = now[1];

            for (int d = 0; d < 4; d++) { // 네 방향 검색하기
                int tempR = dr[d];
                int tempC = dc[d];

                while(r + tempR >= 0 && r + tempR < N && c + tempC >= 0 && c + tempC < M) {
                    // 현재 방문한 적이 없고, 바다가 아니면 같은 섬으로 취급
                    if(visited[r + tempR][c + tempC] == false && map[r + tempR][c + tempC] != 0) {
                        addNode(r + tempR, c + tempC, queue);
                    } else {
                        break;
                    }

                    if (tempR < 0) tempR--;
                    else if (tempR > 0) tempR++;
                    else if (tempC < 0) tempC--;
                    else if (tempC > 0) tempC++;
                }
            }
        }
    }

    // 특정 위치에 섬 정보를 넣어주는 함수
    private static void addNode(int i, int j, Queue<int[]> queue) {
        map[i][j] = sNum;
        visited[i][j] = true;
        int[] temp = { i, j };

        mList.add(temp);
        queue.add(temp);
    }
}

class bEdge implements Comparable<bEdge> {
    int s, e, v;

    public bEdge(int s, int e, int v) {
        this.s = s;
        this.e = e;
        this.v = v;
    }

    @Override
    public int compareTo(bEdge o) {
        return this.v - o.v;
    }
}
```
