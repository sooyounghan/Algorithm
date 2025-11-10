-----
### 정렬 기초
-----
1. 코딩 테스트는 기본적으로 다양한 데이터를 효과적으로 다루는 것에서 시작
   - 데이터를 효과적으로 다루는 방법 중 특히 정렬은 수 많은 알고리즘 문제의 출발점
   - 예를 들어, 이진 탐색과 같은 특정 알고리즘은 정렬된 데이터에만 적용할 수 있음
   - 정렬은 가장 기본적이고 필수적인 요소

2. 오름차순 정렬
   - 데이터를 가장 작은 값부터 시작하여 점차 큰 값으로 나열하는 과정을 의미
   - 즉, 정렬된 결과는 가장 작은 값이 첫 번째 위치에, 그 다음 작은 값이 두 번째 위치에 오는 방식
   - 자바에서는 Arrays.sort() 함수를 사용해 오름차순 정렬 구현 가능
```java
package frequentMistake;

import java.util.Arrays;

public class AscendingSort {
    public static void main(String[] args) {
        int[] A = {5, 3, 4, 2, 1};
        
        Arrays.sort(A); // 오름차순 정렬

        System.out.println(Arrays.toString(A));
    }
}
```
   - 실행 결과
```
[1, 2, 3, 4, 5]
```
   - Arrays.sort() 함수를 통해 A 배열의 값이 오름차순으로 정확히 정렬

3. 내림차순 정렬
   - 데이터를 가장 큰 값부터 시작해 점차 작은 값으로 나열하는 과정
   - 즉, 오름차순 정렬과 반대로 가장 큰 값이 첫 번째 위치에, 그 다음 큰 값이 두 번재 위치에 오는 방식
   - 자바에서는 다음과 같이 배열의 기본 자료형을 클래스로 선언하여 내림차순을 쉽게 구현 가능
```java
package frequentMistake;

import java.util.Arrays;
import java.util.Collections;

public class DescendingSort1 {
    public static void main(String[] args) {
        Integer[] A = {5, 3, 2, 4, 1};

        Arrays.sort(A, Collections.reverseOrder()); // 내림차순 정렬

        System.out.println(Arrays.toString(A));
    }
}
```
   - 실행 결과
```
[5, 4, 3, 2, 1]
```

4. 배열을 클래스형으로 선언하면 Collections.reverseOrder()를 이용해 정렬 기준을 지정 가능
   - 실제 실행 결과를 보면 내림차순 정렬된 것이 확인 가능
   - 하지만, 코딩 테스트에서는 클래스형을 배열의 자료형으로 사용하는 경우가 많지 않으므로, 다른 방법도 고려해야 함
   - 예) 부호를 임시로 반전시키는 아이디어를 활용해 내림차순 정렬 구현
```java
package frequentMistake;

import java.util.Arrays;
import java.util.Collections;

public class DescendingSort2 {
    public static void main(String[] args) {
        int[] A = {5, 3, 2, 4, 1};

        negate(A); // 부호 반전
        Arrays.sort(A); // 오름차순 정렬
        negate(A); // 부호 반전 
        
        System.out.println(Arrays.toString(A));
    }

    static void negate(int[] temp) {
        for (int i = 0; i < temp.length; i++) {
            temp[i] *= -1;
        }
    }
}
```
   - 실행 결과
```
[5, 4, 3, 2, 1]
```

   - 이 방식은 먼저 모든 데이터를 음수로 바꾼 뒤 Arrays.sort() 함수로 오름차순 정렬을 수행하고, 다시 부호를 되돌려 원하는 내림차순 형태로 만드는 기법
   - 실행 결과를 보면 정상으로 내림차순 정렬된 것 확인 가능
   - 이 방법은 간단하면서도 효과적인 내림차순 정렬 아이디어로 유용하게 사용 가능
