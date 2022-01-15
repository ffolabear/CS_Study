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

```

    // 예를 들어 max() 메서드가 정의된 Myfunction 인터페이스 정의
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
    
    
    // 위의 익명 객체를 람다식으로 대체
      MyFunction f = (int a, int b) -> a > b ? a : b;
      int big = f.max(5, 3);


```



<br><br>