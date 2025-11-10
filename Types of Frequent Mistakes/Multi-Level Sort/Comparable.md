-----
### 다중 조건 정렬
-----
1. 여러 기준에 따라 데이터를 정렬해야 하는 상황이 등장
   - 예를 들어 성적을 정렬할 때, 먼저 영어 점수를 기준으로 하되, 영어 점수가 같을 경우에는 수학 점수를 기준으로 정렬
   - 이럴 때, 다중 조건 정렬을 사용하면 여러 기준을 동시에 적용하여 원하는 순서대로 데이터를 정렬할 수 있음
   - 자바에서는 Comparator과 Comparable 인터페이스를 사용하여 다중 조건 정렬을 구현할 수 있음
  
2. Comparable 인터페이스
   - 영어 점수를 기준으로 하고, 영어 점수가 같을 경우 수학 점수로 정렬
```java
package frequentMistake;

public class Score implements Comparable<Score>{
    int english;
    int math;

    public Score(int english, int math) {
        this.english = english;
        this.math = math;
    }

    @Override
    public String toString() {
        return "Score{" +
                "english=" + english +
                ", math=" + math +
                '}';
    }

    @Override
    public int compareTo(Score o) { // Comparable 인터페이스의 compareTo() 함수를 정렬 기준에 따라 구현
        if(this.english == o.english) return o.math - this.math;
        return o.english - this.english;
    }
}
```
```java
package frequentMistake;

import java.util.ArrayList;
import java.util.Collections;

public class ScoreMain {
    public static void main(String[] args) {
        ArrayList<Score> myArr = new ArrayList<Score>();
        
        myArr.add(new Score(80, 100));
        myArr.add(new Score(100, 50));
        myArr.add(new Score(70, 100));
        myArr.add(new Score(80, 90));

        Collections.sort(myArr);
        for (Score s : myArr) {
            System.out.println(s.toString());
        }
    }
}
```
  - 실행 결과
```
Score{english=100, math=50}
Score{english=80, math=100}
Score{english=80, math=90}
Score{english=70, math=100}
```
   - 실행 결과를 보면, Score 리스트가 영어 점수를 기준으로 내림차순 정렬
   - 또한, 영어 점수가 같을 경우 수학 점수를 기준으로 내림차순 정렬
