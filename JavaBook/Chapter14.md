# 14장 Lambda 와 Stream

## 람다식
람다식의 도입으로 객체지향언어인 자바를 함수형 언어도 될수있도록 해줬다. 메서드를 하나의 식으로 표현한 것이다.
메서드를 람다식으로 표현하면 메서드의 이름과 반환값이 얻어지므로 익명함수 라고도 한다.


- 람다식 자체만으로 메서드이 역할을 할 수 있음
- 메서드의 매개변수로 전달할 수 있음
- 메서드의 결과로 반환될 수 있음  

**람다식으로 인해 메서드를 변수 처럼 다루는 것이 가능**

<br>

### 람다식 작성하기 

<br>

#### 기본형
```
    (매개변수 선언) -> {
         ...
    }
```

- 예시

    ```
        (int a, int b) -> {
            return a > b ? a : b; 
        }
        
        →
        
        (int a, int b) -> {
            return a > b ? a : b;
        }
        
    ```

<br>

#### 특징

<br>

-  혹은 return 문 대신 expression 으로 사용할 수도 있다.
    ``` 
        (int a, int b) -> a > b ? a: b
    ```

<br>

- 매개변수의 타입은 추론이 가능할 경우 생략이 가능한데 둘 중 하나만 생략하는 것은 불가능하다.
    ```
        (a, b) -> a > b ? a : b
    ```
  
<br>

- 매개변수가 하나뿐이면 괄호() 생략 가능하다.
    ```
        a -> a*a     //OK
        int a -> a*a //에러
    ```

<br>

- 중괄호{} 안의 문장이 하나 일때는 중괄호{} 생략가능하지만 return문일경우 중괄호{} 생략 불가능하다.

    ```
        (String name, int i) -> System.out.println(name+"="+i)
    ```

<br>

### 함수형 인터페이스
람다식은 메서드와 동등한 것이 아니라 익명클래스의 객체와 동등하다. 참조변수의 타입은 클래스 또는 
인터페이스여야 하며 람다식과 동등한 메서드가 정의되어 있는 것이어야 한다.


기존의 인터페이스를 구현한 객체의 생성은 

```
    
      interface MyFunction  {
        public abstract int max(int a, int b);
      }
      
      
    // MyFunction 인터페이스를 구현한 익명클래스 객체 생성
      MyFunction f = new MyFunction() {
        public int max (int a, int b);
          return a > b ? a : b;
        }
      }
      
      //익명 객체의 메서드 호출
      int big = f.max(5, 3);  
  
```

<br>

이런 식으로 이루어지지만 람다식을 사용하면 좀더 간편하게 생성할 수 있다.

```

      MyFunction f = (int a, int b) -> a > b ? a : b;
      int big = f.max(5, 3); // 익명객체의 메서드를 호출
      
```

이것은 람다식도 실제로는 익명 객체이고, MyFunction 인터페이스를 구현한 익명 객체의 메서드 
max()와 람다식의 매개변수의 타입과 개수, 반환값이 일치하기 때문에 가능한 것이다. 



단, 함수형 인터페이스에는 람다식과 인터페이스가 1:1로 연결되기 때문에 **오직 하나의 추상 
메서드만 정의되어 있어야 한다.** 하지만 static 메서드와 default 메서드의 개수에는 제약이 없다. 
**`@FunctionalInterface` 를 붙이면 컴파일러가 함수형 인터페이스를 올바르게 정의하였는지 확인해준다.**  


<br>


함수형 인터페이스로 람다식을 참조할 수 있지만, 람다식의 타입이 함수형 인터페이스의 타입과 일치하는 것은 아니다.  람다식은 익명 객체이고 
익명 객체는 타입이 없다. 정확히는 타입이 있지만 컴파일러가 임의로 이름을 정하기 때문에 알 수 없다. 그러므로 아래와 같이 형변환이 필요하다.

```
    MyFunction f = (MyFunction) (()->{});
```

람다식은 MyFunction인터페이스를 직접 구현하지 않았지만, 이 인터페이스를 구현한 클래스의 객체와 완전히 동일하기 때문에 위와 같은 
형변환을 허용한다. 그리고 이 형변환은 생략 가능하다.

람다식은 이름이 없을 뿐 분명히 객체인데도, Object 타입으로 형변환 할 수 없다. 람다식은 오직 함수형 인터페이스로만 형변환이 가능하다. 
굳이 Object 타입으로 형변환하려면 아래와 같이 먼저 함수형 인터페이스로 변환해야 한다.


```
    Object obj = (Object)(MyFunction)(()->{});
    String str = (Object)(MyFunction)(()->{})).toString();
```

<br>

### java.util.function 패키지
자주 쓰이는 형식의 메서드를 함수형 인터페이스로 정의해놓은 패키지이다.

|함수형 인터페이스     |                  메서드|                                              설명|
|-----------------|----------------------|-------------------------------------------------|
java.lang.Runnable|void run()            |매개변수도 없고, 반환값도 없음
Supplier          |T get()	             |매개변수는 없고, 반환값만 있음
Consumer          |void accept(T t)      |Supplier와 반대로 매개변수만 있고, 반환값이 없음
Function          |R apply(T t)          |일반적인 함수. 하나의 매개변수를 받아서 결과를 반환
Predicate         |boolean test(T t)     |조건식을 표현하는데 사용. 매개변수는 하나, 반환 타입은 boolean
BiConsumer        |void accept(T t, U u) |두개의 매개변수만 있고, 반환값이 없음
BiPredicate       |boolean test(T t, U u)|조건식을 표현하는데 사용됨. 매개변수는 둘, 반환값은 boolean
BiFunction        |R apply(T t, U u)     |두개의 매개변수를 받아서 하나의 결과를 반환
UnaryOperator     |T apply(T t)          |Function 의 자손, Function 과 달리 매개변수와 결과의 타입이 같다. 
BinaryOperator    |T apply(T t, T t)     |BiFunction 의 자손, BiFunction 과 달리 매개변수와 결과의 타입이 같다.
<br>

- `Predicate` 는 리턴값이 `boolean` 이라는 점만 함수와 다르다. 
- 매개변수가 2개인 함수형 인터페이스는 이름 앞에 ‘Bi’가 붙는다.
- `Supplier` 는 매개변수는 없고 반환값만 존재하는데 메서드는 두 개의 값을 반환할 수 없으므로 `BiSupplier` 가 없다.
- 두개 이상의 매개변수를 갖는 함수형 인터페이스가 필요하다면 직접 만들어서 써야한다.
  ex) 
  ```
       @FunctionalInterface
       interface TriFunction<T, U, V, R>{
            R apply(T t, U u, V v);
       } 
  
  ```

<br>

### Function 의 합성과 Predicate 의 결합

#### Function 의 합성
두 람다식을 합성해서 새로운 람다식을 만들 수 있다. 함수 f, g가 있을 때 
- `f.andThen(g)` f를 먼저 적용하고 g 적용 
- `f.compose(g)` g를 먼저 적용하고 f 적용.

    ```
        //문자열을 숫자로 변환하는 함수 
        Function<String, Integer> f = (s) -> Integer.parseInt(s, 16);
        
        //숫자를 2진 문자열로 변환하는 함수
        Function<Integer, String> g = (i) -> Integer.toBinaryString(i);
        
        //두 함수를 합성
        Function<String, String> h = f.andThen(g);
        Function<Integer, Integer> h = f.compose(g);
    
    ```

<br>

#### Predicate 의 결합
여러 `Predicate` 를 `and()`, `or()`, `negate()` 으로 연결해서 하나의 새로운 `Predicate`를 만들 수 있다.  
두 대상을 비교할때는 static 메서드인 `isEqual()` 를 사용해서 비교할 수 있다. `isEqual()` 의 매개변수로 비교대상을 하나 
지정하고, 또 다른 비교대상은 `test()` 의 매개변수로 지정한다.


<br>

### 메서드 참조
람다식이 하나의 메서드만 호출하는 경우, 메서드 참조를 통해 더 간결하게 표현할 수 있다. 생략된 부분은 컴파일러가 우변 혹은 좌변에서 
알아낼 수 있다.

**기존 방법** : `Function<String, Integer> f = (String s) -> Integer.parseInt(s);`  
**메서드 참조** : `Funcation<String, Integer> f = Integer::parseInt;`

ex) 
```
    //기존
    BoFunction<String, String, Boolean> f = (s1, s2) -> s1.equals(s2);
    
    //메서드 참조
    BoFunction<String, String, Boolean> f = String::equals;
    
```
두개의 `String` 을 받아서 `Boolean` 을 반환하는 클래스는 다른 클래스에도 존재할 수 있으므로 클래스 이름은 반드시 필요하다.


메서드 참조를 할 수 있는 다른경우는 이미 생성된 객체의 메서드를 람다식에서 사용한 경우에는 클래스 이름 대신 그 객체의 참조변수를 적어서
사용할 수 있다.
```

  MyClass obj = new MyClass();
  Function<String, Boolean> f = (x) -> obj.equals(x); 
  Function<String, Boolean> f2 = obj::equals;
   
```

|종류 |람다 |메서드 참조 |
|---|---|---|
|static 메서드참조   |(x) -> ClassName.method(x)| ClassName::method|
|인스턴스 메서드참조    |(obj, x) -> obj.method(x)| ClassName::method|
|특정 객체 인스턴스 참조|(x) -> obj.method(x)| obj::method|

> 하나의 메서드만 호출하는 람다식은 `클래스이름::메서드이름` 또는 `참조변수::메서드이름`으로 바꿀 수 있다.


<br>


생성자를 호출하는 람다식도 메서드 참조로 변환 가능하다. 매개변수가 있는 생성자라면 매개변수의 개수에 따라 알맞은 
함수형 인터페이스를 사용해야한다.

```
  Supplier<MyClass> s = () -> new MyClass();  // 람다식
  Supplier<MyClass> s = MyClass::new; // 메서드 참조
```

<br>

배열을 생성할 때는 다음과 같이 작성한다.

```
  Function<Integer, int[]> f = x -> new int[x]; // 람다식
  Function<Integer, int[]> f2 = int[]::new; // 메서드 참조
```

<br>



```
  Function<Integer, int[]> f = x -> new int[x]; // 람다식
Function<Integer, int[]> f2 = int[]::new; // 메서드 참조
```

<br>

## 스트림
스트림은 기존의 `Collection` 이나 `Iterator` 를 사용해서 데이터를 다룰때 많은 부분을 구현해야 했던 점을 개선하기 위해 나온 인터페이스다. 스트림을 
사용하면 데이터 소스가 무엇이든 간에 같은 방식으로 다룰수 있으므로 코드의 재사용성이 높아지는 장점이 있다. 

<br>

```
        String[] arr = {"red", "yellow", "green", "blue", "black"};
        List<String>  list = Arrays.asList(arr);
```

<br>

위와 같은 배열이 있을때 스트림의 생성은 다음과 같다.

```
        Stream<String> streamList = list.stream();
        Stream<String> streamArr = Arrays.stream(arr);        
```
<br>

### 특징

1. **`Stream` 은 데이터를 변경하지 않는다.**
   

2. **`Stream` 은 일회용이다.**  
    필요하다면 읽은 결과를 컬렉션이나 배열에 담아서 저장하자.
   

3. **`Stream` 은 작업을 내부 반복으로 처리한다.**  
   작업을 내부반복으로 처리하기 때문에 코드가 간결해지는데, 이는 반복문을 메서드의 내부에 숨길 수 있다는 것을 의미한다.
   
**ex)**
```
        Integer[] nums = {1, 2, 3, 4, 5, 6};
        List<Integer> sortedNums = Arrays.stream(nums).sorted().collect(Collectors.toList());

        sortedNums.forEach(num -> System.out.println("숫자 : " + num));
        
        
        //-----------------
        
        숫자 : 1
        숫자 : 2
        숫자 : 3
        숫자 : 4
        숫자 : 5
        숫자 : 6

```


## 다양한 스트림 만들기

### 컬렉션

스트림은 `Collection` 패키지에 정의되어 있으므로 컬렉션 클래스들은 `Collection.stream()` 을 통해서 스트림을 생성할 수 있다.

```
    //기본형
    Stream<T> Collection.stream();
    
    //List
    List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
    Stream<Integer> intStream = list.stream();

```

각 요소들에 대한 작업을 할때는 `foreach()` 를  사용할 수 있다.
```
    intStream.forEach(System.out::println);
  
```

<br>

### 배열

배열의 스트림 생성은 `Stream` 과 `Arrays` 에 정의되어있다.

``` 
    //기본형
    Stream<String> stream = Stream.of();    
    Stream<String> stream = Arrays.stream(T[]);
    
    //예시
    Stream<String> streamArr = Arrays.stream(new String[]{"red", "yellow", "green", "blue", "black"});
    Stream<String> streamArr = Stream.of("red", "yellow", "green", "blue", "black");
    
    //intStream
    IntStream intStream = IntStream.of(1, 2, 3, 4);
    
    int[] nums = {1, 2, 3, 4};
    IntStream intStream = IntStream.of(nums);
    IntStream intStream = Arrays.stream(nums);
    
```

<br>

- `IntStream` 과  `LongStream` 은  `range()` 와 `rangeClosed()` 메서드를 통해 **특정 범위의 연속된 정수**로 스트림을 만들 수 있다.
  - `range()` 는 마지막 원소 포함 ❌ - `rangeClosed()`는 포함 ⭕️


- `Stream.empty()` 를 통해서 빈 스트림도 만들 수 있다.
- 타입이 같은 두 스트림은 `concat()` 메서드ㄹ를 사용해서 하나도 연결할 수 있다.

<br>


## 스트림의 연산
스트림으로 읽은 데이터들을 내부에 있는 연산으로 인해서 다양한 작업들을 할 수 있다. 연산은 크게 두 종류가 있다.

- 중간 연산 : 연산 결과가 스트림인 연산. 스트림에 연속해서 중간 연산할 수 있음  
  - `distinct()` : 중복을 제거
  - `filter()` : 조건에 안 맞는 요소 제외
  - `limit()` : 스트림의 일부를 잘라낸다.
  - `skip()`: 스트림의 일부를 건너뛴다.
  - `peek()`: 스트림의 요소에 작업수행.
  - `sorted()`: 스트림의 요소를 정렬한다.
  - `map()`: 스트림의 요소를 변환한다.
  
  <br>

- 최종 연산 : 연산 결과가 스트림이 아닌 연산, 스트림의 요소를 소모하므로 단 한번만 가능
  - `foreach()` : 각 요소에 지정된 작업 수행
  - `count()` : 스트림의 요소의 개수 반환
  - `max()`, `min()`: 스트림의 최대/최소값을 반환
  - `findAny()`, `findFirst()` : 스트림의 요소 하나를 반환
  - `allMatch()`, `anyMatch()`, `noneMatch()` : 주어진 조건을 모든 요소가 만족시키는지 아닌지
  - `toArray()` : 스트림의 모든 요소를 배열로 반환
  - `reduce()` : 스트림의 요소를 하나씩 줄여 가면서 계산한다.
  - `collect()` : 스트림의 요소를 수집한다. (주로 그룹화하거나 분할한 결과를 컬렉션에 담을때) 

  <br>

- 스트림의 연산은 최종 연산이 수행되기 전까지는 중간 연산이 수행되지 않는다  →  **지연연산**   
- 일반적으로 `stream`.`중간연산`.`중간연산`.`중간연산`.`중간연산`.......`최종 연산` 의 형태이다.
- 기본적으로 스트림은 `wrapper class` 를 타입으로 받지만 `IntStream`, `LongStream` 등등을 사용해서 원시 타입을 넣을 수도 있다.


<br>


## 스트림의 중간연산

### 스트림 자르기
  - `skip(long n)` : n 만큼의 요소를 건너뛴다.
  - `limit(long n)` : 스트림의 요소를 n개로 제한한다.
    > `stream.skip(3).limit(5)` 를 하면 4벙 요소부터 8번 요소를 가진 스트림을 반환한다.
    > 

<br>

### 스트림 요소 걸러내기
  - `filter()` : 중복된 요소를 제거한다.
    ```
        IntStream intStream = IntStream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        intStream.filter(i -> i > 5).forEach(System.out::print);
    
        //678910
        //5 이상인 숫자만 출력되었다. 

    ```    
    `.filter().filter().filter() ...` 처럼 여러 조건을 추가할 수 있다.
    
    <br>


  - `distict()` : 주어진 조건에 맞지 않는 요소를 걸러낸다.
    ```
        IntStream intStream = IntStream.of(1, 2, 3, 4, 3, 2, 1, 1, 1);
        intStream.distinct().forEach(System.out::print);
        
        //1234
        //중복된 원소를 제거한 값이 출력된다.
    
    ```

  <br>

  - `sorted()` : 지정된 `Comparator` 로 정렬한다.
    
    ```
        Stream<String> strStream = Stream.of("aaa", "aab", "bbb", "ccc", "ddd");
    
        //1    
        strStream.sorted().forEach(System.out::println);
    
        //2
        strStream.sorted(Comparator.naturalOrder()).forEach(System.out::println);
    
        //3
        strStream.sorted(String::compareTo).forEach(System.out::println);
    
        //4
        strStream.sorted((s1, s2) -> s1.compareTo(s2)).forEach(System.out::println);
        
    
        //aaa
        //aab
        //bbb
        //ccc
        //ddd
    
    ```

    - 역순은  `strStream.sorted(Comparator.reverseOrder())`

    <br>
    
  - `map()` : 스트림의 요소에 저장된 값 중 원하는 값을 뽑아내거나 변환할때 사용한다.
    ```
    
        Stream<String> strStream = Stream.of("aaa", "aab", "bbb", "ccc", "ddd");
        strStream.map(String::toUpperCase).forEach(System.out::println);
    
        //AAA
        //AAB
        //BBB
        //CCC
        //DDD
    
    ```
    
  <br>
    
  - `peek()` : 연산과 연산 사이에 올바르게 처리되었는지 확인하고 싶을때 사용
    ```
        IntStream is = IntStream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        is.filter(i -> i > 2)
          .peek(i -> System.out.println("peek : " + i))
          .filter(i -> i == 9).forEach(System.out::println);
      
    
        //peek : 3
        //peek : 4
        //peek : 5
        //peek : 6
        //peek : 7
        //peek : 8
        //peek : 9
        //9
        //peek : 10
          
    ```
    
    <br>
    
  - `mapToInt()`, `mapToLong()`, `mapToDouble()` : 일반적인 Stream 객체를 원시 스트림으로 바꾸거나 그 반대로 하는 
    작업이 필요한 경우 사용한다. 원시 객체를 일반적인 스트림으로 변환할때는 `mapToObj()` 를 사용한다. 
    
    ```
    
      Double[] nums = {1.0, 2.0, 3.0, 4.0};
      Stream.of(nums).mapToInt(Double::intValue).forEach(System.out::println);
      
      //1
      //2
      //3
      //4
    
    ```

    <br>
    
    > Stream<String> -> IntStream 변환할때 -> mapToInt(Integer::parseInt)
    > Stream<Integer> -> IntStream 변환할때 -> mapToInt(Integer::intValue)

    <br>
    
- `flatMap()` : 스트림의 요소가 배열이거나 `map()` 의 결과가 배열인 경우 `Stream<T>` 의 형태로 만들때 사용한다.  
  `map()` 을 사용하면 배열들의 스트림으로 만들지만 `flatMap()`은 스트림의 스트림으로 만들어 준다.
  
  ```
     //map
     Stream<Stream<String>> strStream = strArrStrm.map(Arrays::stream);
      
     //flatmap
     Stream<String> strStream = strArrStrm.flatmap(Arrays::stream);
   
  ```

<br>

### 스트림의 최종 연산
최종 연산은 스트림의 요소를 사용해서 만들기 때문에 최종연산후 스트림은 닫히고 다시 사용할 수 없다. 

#### `forEach()`
- `peek()` 와 다르게 스트림의 요소를 소모한다.
- 반환 타입이 `void`이므로 요소를 출력하는 용도로 주로 사용한다.

<br>

#### 조건 검사
- `allMatch()`, `anyMatch()`, `noneMatch()`, `findFirst()`, `findAny()`
- 지정된 조건들에 대해 모든요소가 일치하는지, 일부가 일치하지 않는지 등을 확인하는데 사용한다.
- 매개변수로 `predicate` 을, 리턴값이 `boolean` 이다.


<br>

#### 통계
- `count()`, `sum()`, `average()`, `max()`, `min()`
- `IntStream` 같은 기본형 스트림에 대해서 통계 정보를 얻을 수 있는 메서드이다.
- 기본형 스트림이 아닌 경우에는 `max()`, `min()`, `count()` 만 존재한다.

<br>

#### `reduce()`
- 스트림의 요소를 줄여나가면서 연산을 수행학 결과를 반환하는 메서드이다.
- 두요소를 가지고 연산한 결과를 가지고 다음 요소와 연산한다. 

<br>

#### `collect()`
- 스트림의 요소를 수집할때 어떻게 수집할지를 정의하는 메서드이다. 
- 스트림을 컬렉션과 배열로 변환할때는 `toList()`, `toSet()`, `toMap()`, `toCollection()`, `toArray()` 를 사용한다.
- 통계정보와 리듀싱도 가능하며 스트림의 모든 요소를 하나의 문자열로 반환하는 `joining()` 이 존재한다. 
- 스트림의 요소를 특정 기준으로 그룹화 하는 `groupingBy()`, `partitioningBy()` 이 존재한다. 
  - `groupingBy()` 는 스트림의 요소를 `Function()`으로, `partitioningBy()`은 `Predicate()`로 분류한다는 점만 다르다.

<br>

## Collector 구현하기
컬렉터를 작성한다는 것은 `Collector` 인터페이스를 구현하는 것과 같다. 구현해야 하는 메서듣 5개가 있다.

- `supplier()` : 작업 결과를 저장할 공간을 제공
- `accumulator()` : 스트림의 요쇼를 수집할 방법을 제공
- `combiner()` : 두 저장공간을 병합할 방법을 제공
- `finisher()` : 결과를 최종적으로 변환할 방법을 제공

위의 4가지 메서드는 반환 타입이 함수형 인터페이스 이므로 4개의 람다식을 작성해야한다. 마지막 메서드인 `Characteristics()` 는 
컬렉터의 특성이 담긴 set 을 반환한다. 

<br><br>


