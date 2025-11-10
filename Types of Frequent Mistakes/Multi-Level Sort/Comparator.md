-----
### Comparator 인터페이스
-----
1. 예) 2가지 조건을 기준으로 정렬하는데, 이번에는 수학 점수를 우선 기준으로 하고 수학 점수가 같을 경우 영어 점수로 정렬
```java
package frequentMistake;

import java.util.Comparator;

public class ScoreComparator implements Comparator<Score> {
    @Override
    public int compare(Score o1, Score o2) { // Comparator 인터페이스의 compare() 함수를 정렬 기준에 따라 구현
        if(o1.math == o2.math) return o2.english - o1.english;
        return o2.math - o1.math;
    }
}
```
```java
package frequentMistake;

import java.util.ArrayList;
import java.util.Collections;

public class ScoreComparatorMain {
    public static void main(String[] args) {
        ArrayList<Score> myarr = new ArrayList<Score>();

        myarr.add(new Score(80, 100));
        myarr.add(new Score(100, 50));
        myarr.add(new Score(70, 100));
        myarr.add(new Score(80, 90));

        Collections.sort(myarr, new ScoreComparator());
        for (Score s : myarr) {
            System.out.println(s.toString());
        }
    }
}
```
   - 실행 결과
```
Score{english=80, math=100}
Score{english=70, math=100}
Score{english=80, math=90}
Score{english=100, math=50}
```

2. 수학 점수를 기준으로 내림차순 정렬하고, 수학 점수가 같은 경우에는 영어 점수를 기준으로 내림차순 정렬된 결과 확인 가능

3. Comparable과 Comparator 비교
   - 두 인터페이스 모두 다중 조건 정렬에 활용할 수 있지만, 사용 방식과 유연성에는 다음과 같은 차이점 존재
<div align="center">
<img src="https://github.com/user-attachments/assets/7262bdf3-630e-4f85-96ef-8765f09bc82b">
</div>
