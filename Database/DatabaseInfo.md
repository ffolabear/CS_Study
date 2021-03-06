# 데이터 베이스의 등장 배경

<div align="center">
<img src="https://user-images.githubusercontent.com/65614734/141927688-c0c68d9a-4dac-4363-9b1f-c5991245669e.png">
   <p>데이터 베이스가 나오기 전에 사용되었던 파일 처리 시스템 (File Processing System)</p>
    
</div>

<br>

데이터 베이스가 나오기전에는 데이터들을 파일로 저장했는데 여러가지 문제점이 있었다.

1. **데이터 종속의 문제 (Data Dependency Issue)**
    
    데이터가 **특정 사용자, HW, SW에서만 사용되는 제한**이 생긴다. 만약 모든 데이터들이 hwp 파일로 저장된다면? word 를 사용사람은 열람할 수 없다.


2. **데이터 중복의 문제 (Data Redundancy Issue)**
    
    동일한 데이터가 여러 번 생성될 수 있는 문제다. 기존에 저장해놓은 데이터가 있는지 찾아보려면 모든 파일을 탐색해랴하기때문에 중복여부를 찾기 어렵고 데이터의 유지보수가 어렵다.


3. **무결성 훼손의 문제 (Integrity Issue)**
    
    어떤 데이터를 수정해야될때 모든 파일이 수정된 정보를 반영해야한다. 하지만 모든 파일이 수정되지않았다면? 어떤 파일은 올바른 정보를, 어떤 파일은 잘못된 정보를 보여줘서 오류가 생길수 있다.  

   <br>

> 💡 데이터의 정확성과 일관성을 유지하고 보증하는 것을 데이터 무결성이라고 함

   <br>

4. **동시 접근의 문제 (Concurrent Sharing Issue)**

    파일 처리 시스템에서 다수의 사용자가 동시 접근하면 데이터 관리의 일관성이 떨어진다. 예를 들어 동시간 대에 사용자 A와 사용자 B가 데이터 1에 접근해서 작업한다고 하자. 이때 동시 작업한 것이 반영되어 서로 공유가 되어야하는데, 파일 처리 시스템을 이러한 기능을 적절히 제공하지 못한다.

<br><br>

### 그래서 데이터베이스가 등장하게 되었음!

<br><br>

<div align="center">
    <img src="https://user-images.githubusercontent.com/65614734/141927697-e89bbe8a-920a-4707-958b-f843870fad4e.png" width="70%">
<p>데이터 베이스를 관리하는 DBMS</p>
</div>



### 데이터베이스의 특징   

<br>

1. **데이터의 독립성**
    - **물리적 독립성** : 데이터베이스 성능 향상을 위해 데이터 파일을 늘리거나 새롭게 추가하더라도 관련 프로그램을 수정할 필요가 없다.
    - **논리적 독립성** : 데이터베이스는 논리적인 구조로 다양한 응용 프로그램의 논리적 요구를 만족시켜줄 수 있다.


2. **데이터의 무결성**

   여러 경로를 통해 잘못된 데이터가 발생하는 경우의 수를 방지하는 기능으로 데이터의 유효성 검사를 통해 데이터의 무결성을 구현하게 된다.


3. **데이터의 보안성**

   인가된 사용자들만 데이터베이스나 데이터베이스 내의 자원에 접근할 수 있도록 접근권한을 설정함으로써 모든 데이터에 보안을 구현할 수 있다.


4. **데이터의 일관성**

   연관된 정보를 논리적인 구조로 관리함으로써 어떤 하나의 데이터만 변경했을 경우 발생할 수 있는 데이터의 불일치성을 배제할 수 있다. 또한 작업 중 일부 데이터만 변경되어 나머지 데이터와 일치하지 않는 경우의 수를 배제할 수 있다.


5. **데이터 중복 최소화**

   데이터베이스는 데이터를 통합해서 관리함으로써 파일 시스템의 단점 중 하나인 자료의 중복과 데이터의 중복성 문제를 해결할 수 있다.


<br><br>


### 데이터베이스의 구성요소 

<br>

- 구성요소: 행(튜플), 열(속성)

<div align="center">
   <img src="https://user-images.githubusercontent.com/65614734/141927715-9824d268-2ca7-4b11-bad7-2e425e6dd373.png" width="700px">
</div>
  
- 스키마: 데이터베이스의 구조와 제약 조건에 관한 전반적인 명세를 기술한 메타데이터의 집합
  <br><br>
    
   
   <div float="left">
   <img src="https://user-images.githubusercontent.com/65614734/141940790-80d1eab5-6c5d-48f2-9f2c-269dcfa1cfde.png" width="40%">
    <img src="https://user-images.githubusercontent.com/65614734/141940795-f08a4839-64e4-4eeb-ac35-ebdd95b2f461.png" width="50%">
   </div>
   <div align="center">
      <p>&lt; Oracle 데이터베이스의 사원 테이블 스키마 &gt;</p>
   </div>
   
   
   <br><br>
    
    
    - **외부 스키마(External Schema) = 사용자 뷰(View)**
    <br><br>
    - **개념 스키마(Conceptual Schema) = 전체적인 뷰(View)**
        - 데이터에 대한 접근 권한, 보안 정책, 무결성 규칙을 포함
        - 데이터를 통합한 조직 전체의 데이터베이스 구조를 논리적으로 정의한 것
    <br><br>   
    - **내부 스키마(Internal Schema) = 저장 스키마(Storage Schema)**
        - 설계자, 개발자의 관점에서 바라보는 스키마
     
     <br>
     

- 키: 테이블에서 특정 행을 유일하게 식별할 수 있게 하는 특징, 열 혹은 복수의 열 모음
    - 테이블의 각 행에는 기본키가(PK, Primary Key) **반드시** 있어야 한다.

      <br>
   
- 외부키
    - 이용하여 다른 테이블과 링크할 수 있다.
    - 그 값이 다른 테이블의 키 열의 값과 같은 열
    - 참조무결성: 모든 외부 키 값이 참조하는 테이블의 값으로 존재하는 경우

      <br>

    <div align="center">
      <img src="https://user-images.githubusercontent.com/65614734/141943538-c9ac604d-9e9b-41f0-9bc7-362497b1c6dc.png" width="60%">
      <p>&lt; Oracle 사원 테이블의 각 테이블을 참조할 수 있는 외래키와 기본키 &gt;</p>
   </div>
   

### 데이터 베이스의 성능

데이터베이스의 성능 이슈는 디스크 I/O 를 어떻게 줄이느냐에서 시작된다. 현재 탐색하는 데이터에서 다음 데이터를 읽는 성늘을 개선하는 것이 중요하다. 이때 순차적으로 읽는 것이 랜덤으로 읽는 것이 더 빠른데 대부분의 작업이 랜덤으로 읽는 작업이므로 SQL 튜닝을 통해 그 기능을 보완한다.
    

#### SQL?
> 💡 Structured Query Language, 데이터베이스에서 데이터의 관리를 위해 설계된 프로그래밍 언어


<br><br>  

이미지출처  
- https://docs.oracle.com/cd/B12037_01/server.101/b10771.pdf
- https://www.oracletutorial.com/getting-started/oracle-sample-database/
- https://93jpark.tistory.com/23
- https://lucy-the-marketer.kr/ko/growth/what-is-database/

<br><br> 
