-----
### 2차원 ArrayList
-----
1. 코딩 테스트에는 그래프 관련 알고리즘이 자주 등장 : 이 때 그래프 구조를 표현하는데 많이 사용하는 것이 이차원 ArrayList

2. 이차원 ArrayList 선언과 초기화 : 그래프의 엣지를 표현한 클래스
```java
package frequentMistake.arraylist;

public class Edge {
    int endNode;
    int value;
}
```

2. 해당 클래스를 자료형으로 하는 이차원 ArrayList를 초기화하는 방법
```java
ArrayList<Edge> list[] = new ArrayList[10];
```
   - 크기가 10이고, 자료형이 ArrayList인 배열을 선언한 코드
   - 보통 이렇게 선언하면 초기화가 완료되었다고 생각하는 경우가 많은데, 하지만 아직 10개의 ArrayList 각각에 다음과 같은 생성 작업(메모리 할당)을 추가로 해야함
```java
for (int i = 0; i < 10; i++) {
    list[i] = new ArrayList<Edge>();
}
```
   - 초기화가 완료되었으므로, 이렇게 생성한 이차원 ArrayList에 그래프 데이터를 저장

3. 그래프 데이터 저장
   - 다음과 같이 간단한 그래프가 있다고 가정
<div align="center">
<img src="https://github.com/user-attachments/assets/d74c96d1-3012-4262-a67f-8b8f97a97689">
</div>

   - 그래프는 코딩 테스트에서 다음과 같은 입력값으로 주어짐
<div align="center">
<img src="https://github.com/user-attachments/assets/11c3b5e3-0cad-47f1-b671-4951cfa2eea0">
</div>

   - 이러한 그래프 데이터를 이차원 ArrayList 자료구조를 이용해 저장
```java
for (int i = 0; i < E; i++) { // 저장할 엣지 개수만큼 반복
    st = new StringTokenizer(br.readLine());
    int s = Integer.parseInt(st.nextToken());
    int e = Integer.par
    list[s].add(new Edge(e, v)); // 이차원 ArrayList에 저장
}
```

   - 이렇게 저장하면 실제 ArraayList에는 데이터가 다음과 같이 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/dca2c912-f890-4f1c-9645-a89c70cb5918">
</div>

4. 그래프 데이터 가져오기
   - 필요한 데이터를 가져오는 코드 : 1번 노드에서 시작되는 에지 데이터를 가져오는 코드
```java
for (int i = 0; i < list[1].size(); i++) {
    Edge tmp = list[1].get(i);
    int next = tmp.endNode;
    int value = tmp.value;
}
```
   - 이 코드를 사용하면 예제 그래프 데이터에서 다음과 같은 2개의 에지 데이터를 가져올 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/8cd82bea-9462-4346-a712-f58c1425a8e8">
</div>
