# [Lambda Expression](https://www.youtube.com/watch?v=3wnmgM4qK30&t=201s)

## 1. 람다식이란?
- 함수(method)를 간단한 '식(expression)'으로 표현하는 방법
- 익명 함수(이름이 없는 함수, anonymous function) : 반환타입과 이름 생략
- 구체적으로 익명 객체

</br>

## 2. 람다식 장단점

### 2.1 람다식 장점
- 코드의 간결성 : 람다를 사용하면 불필요한 반복문의 삭제가 가능하며 복잡한 식을 단순하게 표현 가능
- 지연연산 수행 : 람다는 지연연상을 수행 함으로써 불필요한 연산을 최소화 가능
- 병렬처리 가능 : Multi-thread를 활용하여 병렬처리 가능
 
### 2.2 람다식 단점
- 람다식의 호출이 까다로움
- 단순 호출의 경우에 속도 저하
- 불필요하게 너무 사용할 경우에 가독성 저하
- 재사용 불가
- 디버깅 시 함수 콜 스택 추적 어려움

</br>

## 3. 람다식 작성
### 3.1 람다식 작성 방법
- Method의 이름과 반환타입을 제거하고 '->'를 블록{ } 앞에 추가
```Java
int max(int a, int b) {
  return a > b ? a : b;
}

(int a, int b) -> {
  return a > b? a : b;
}
```
- 반환값이 있는 경우, 식이나 값만 적고 return문 생략 가능(
- 세미콜론 생략
```Java
(int a, int b) -> a > b ? a : b
```

- 매개변수의 타입이 추론 가능하면 생략 가능 (대부분의 경우 생략 가능)
```Java
(a, b) -> a > b  ? a : b
```

### 3.2 람다식 작성 주의사항
- 매개변수가 하나인 경우, 괄호( ) 생략 가능(타입이 없을 경우에만)
```Java
(a) -> a * a 
a -> a * a // OK

(int a) -> a * a
int a -> a * a // Error, 타입이 있을 경우 생략 불가
```

- 블록 안의 문장이 하나뿐 일 때, 괄호{ } 생략 가능
- 세미콜론 생략
```Java
(int i) -> {
 System.out.println(i); 
}

(int i) -> System.out.println(i)
```

### 3.3 람다식 작성 예시
```Java
new Thread(new Runnable() {
   @Override
   public void run() { 
      System.out.println("Hello World!"); 
   }
}).start();

new Thread(()->{
      System.out.println("Hello World!");
}).start();
```
 
</br> 

## [4. 함수형 인터페이스](https://khj93.tistory.com/entry/JAVA-%EB%9E%8C%EB%8B%A4%EC%8B%9DRambda%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%82%AC%EC%9A%A9%EB%B2%95)

- **@FunctionalInterface** : Functional Interface는 일반적으로 '구현해야 할 추상 메소드가 하나만 정의된 인터페이스'를 지칭
- Java 컴파일러는 이렇게 명시된 함수형 인터페이스에 두 개 이상의 메소드가 선언되면 오류 발생
``` Java
@FunctionalInterface
public interface Math {
    public int Calc(int first, int second);
}

// 오류 발생
@FunctionalInterface
public interface Math {
    public int Calc(int first, int second);
    public int Calc2(int first, int second);
}
```

### 4.1 함수형 인터페이스 람다 사용 방법
- 함수형 인터페이스 선언
```Java
@FunctionalInterface
interface Math {
    public int Calc(int first, int second);
}
```

- 추상 메소드 구현 및 함수형 인터페이스 사용
```Java
public static void main(String[] args){

   Math plusLambda = (first, second) -> first + second;
   System.out.println(plusLambda.Calc(4, 2));
   // 실행결과 : 6

   Math minusLambda = (first, second) -> first - second;
   System.out.println(minusLambda.Calc(4, 2));
   // 실행결과 : 2
}
```

</br>

## 5. Stream API
- 다양한 데이터를 표준화된 방법으로 다루기 위한 라이브러리
- Java8부터 추가
```Java
example.stream().filter(x -> x < 2).count()

// stream : 스트림 생성
// filter : 중간 연산(스트림 변환), 연속 수행 가능
// count : 최중 연산, 마지막에 단 한 번만 사용 가능
```

### 5.1 Stream 특징
- 데이터 변경X
- 1회용
- [지연 연산 수행](https://ko.wikipedia.org/wiki/%EB%8A%90%EA%B8%8B%ED%95%9C_%EA%B3%84%EC%82%B0%EB%B2%95)
- 병렬 실행 가능

### 5.2 Stream 종류
|Stram 종류|설명|
|:---:|:---:|
| Stream &lt;T&gt; |범용 Stream|
|IntStream|값 타입이 Int인 Stream|
|LongStream|값 타입이 Long인 Stream|
|DoubleStream|값 타입이 Double인 Stream|

### 5.3 Stream의 중간 연산 명령어
|Stram 중간 연산 종류|설명|
|:---:|:---:|
|distinct()|Stream의 요소 중복 제거|
|sorted()|Stream 요소 정렬|
|filter(Predicate &lt;T&gt; predicate)|조건에 충족하는 요소를 Stream으로 생성|
|limit(long maxSize)|maxSize 까지의 요소를 Stream으로 생성|
|skip(long n)|처음 n개의 요소를 제외하는 stream 생성|
|peek(Consumer &lt;T&gt; action)|T타입 요소에 맞는 작업 수행|
|flatMap(Function &lt;T, stream&lt;? extends R&gt;&gt; Tmapper)|T타입 요소를 1:N의 R타입 요소로 변환하여 스트림 생성|
|map(Function &lt;? super T, ? extends R&gt; mapper)|입력 T타입을 R타입 요소로 변환한 스트림 생성|
|mapToInt(), mapToLong(), mapToDobule()|map Type이 숫자가 아닌 경우 변환하여 사용|

### 5.4 Stream의 최종 연산 명령어
|Stram 최종 연산 종류|설명|
|:---:|:---:|
|void forEach(Consumer <? super T> action)|Stream 의 각 요소에 지정된 작업 수행|
|long count()|Stream 의 요소 개수|
|Optional < T > sum (Comparator <? super T> comparator)|Stream 의 요소 합|
|Optional < T > max (Comparator <? super T> comparator)|Stream 요소의 최대 값|
|Optional < T > min (Comparator <? super T> comparator)|Stream 요소의 최소 값|
|Optional < T > findAny()|Stream 요소의 랜덤 요소|
|Optional < T > findFirst()|Stream 의 첫 번째 요소|
|boolean allMatch(Pradicate < T > p)|Stream 의 값이 모두 만족하는지 boolean 반환|
|boolean anyMatch(Pradicate < T > p)|Stream 의 값이 하나라도 만족하는지 boolean 반환|
|boolean noneMatch(Pradicate < T > p)|Stream 의 값이 하나라도 만족하지 않는지 boolean 반환|
|Object[] toArray()|Stream 의 모든 요소를 배열로 반환|
|reduce 연산|Stream의 요소를 하나씩 줄여가며 계산|
|collector 연산|Stream의 요소를 수집하여 요소를 그룹화하거나 결과를 담아 반환|

### 5.5 Stream 사용 예제
- [직접 메소드 참조](https://myhappyman.tistory.com/65)
- 1부터 10사이에서 짝수 출력
```Java
IntStream.range(1, 11).filter(i -> i % 2 == 0)
      .forEach(System.out::println); 

// 실행결과
// 2
// 4
// 6
// 8
// 10
```

</br>

- 0 ~ 1000까지의 값 중 500이상이며 짝수이면서 5의 배수인 수의 합 출력
```Java
System.out.println(
      IntStream.range(0, 1001)
            .skip(500)
            .filter(i -> i % 2 == 0)
            .filter(i -> i % 5 == 0)
            .sum()
);

// 실행결과 : 38250
```

</br>

![image](https://user-images.githubusercontent.com/50551349/125161851-a64d0280-e1bf-11eb-8b18-ab7ea9ca0813.png)

```java
class Solution {
    boolean solution(String s) {
        boolean answer = true;
        s = s.toLowerCase();

        answer = s.chars().filter(e -> 'p' == e).count() == s.chars().filter(e -> 'y' == e).count();

        return answer;
    }
}
```

</br>

![image](https://user-images.githubusercontent.com/50551349/125161662-8e28b380-e1be-11eb-8ac4-3cf5dd5469e6.png)

```java
import java.util.*;

class Solution {
    public int[] solution(int[] arr, int divisor) {
        int[] answer = {};
        
        answer = Arrays.stream(arr).filter(element -> element % divisor == 0).toArray();
        Arrays.sort(answer);
        
        if(answer.length == 0) {
            answer = new int[]{-1};
        }
        
        return answer;
    }
}
```

</br>

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {2, 4, 6};

        boolean result = Arrays.stream(arr)
                .allMatch(a -> a % 2 == 0);
        System.out.println("2의 배수? " + result);

        result = Arrays.stream(arr)
                .anyMatch(a -> a % 3 == 0);
        System.out.println("하나라도 3의 배수? " + result);

        result = Arrays.stream(arr)
                .noneMatch(a -> a % 3 == 0);
        System.out.println("3의 배수 없음? " + result);
    }
}

// 실행결과 
// 2의 배수? true
// 하나라도 3의 배수? true
// 3의 배수 없음? false
```

</br>

![image](https://user-images.githubusercontent.com/50551349/125424847-85e0e4cc-9379-4252-b204-3b419f80d40b.png)

```Java
class Solution {
    public boolean solution(String s) {
        boolean answer = false;
        
        if(s.length() == 4 || s.length() == 6) {
            answer = s.chars().allMatch( Character::isDigit );
        }
        
        return answer;
    }
}
```
