---
title: Java 선택적 매개변수 (Optional Parameter)
date: 2023-04-20 03:00:00 +0900
categories: [Java]
tags: [Java, Optional, Parameter]     # TAG names should always be lowercase
---
# 자바에서의 선택적 매개변수 (Optional Parameter) 
#### 1. 오버로딩 (OverLoading)<br>
#### 2. Varargs<br>
#### 3. Optional<br>
#### 4. Build Pattern<br><br>



### 1. 오버로딩 (Overloading)
```java
class Overloading{
  static void optionalParameter(int a, int b){return (a + b);}

  static void optionalParameter(int a, int b, int c){return (a + b + c);}
}

class TestOverloading{
  public static void main(String[] args){
    System.out.println(Overloading.optionalParameter(1,2)+"\n");
    System.out.println(Overloading.optionalParameter(1,2,3)+"\n");
  }
}
```
결과
```
3
6
```

### 2. Varargs (After JDK 5)<br><br>
0 or 여러개의 매개변수를 전달 가능<br>
임시적으로 배열타입으로 선언됨<br>
전달 하려는 인수의 개수를 모를떄<br>
무제한의 인수를 전달하고자 할때<br>
<b>제네릭과 같이 쓰면 타입 안정성이 깨질 수 있음
```java
class Varargs{
    //Wrong Syntax, Must provide only one varargs as parameter in method
    static void varargsParameters(int... a, String... string){}
    static void varargsParameter(int... a){
        System.out.printf("Number of Arguments : %d\n", a.length);
        for(int element : a){
            System.out.printf("Element : %d\n", element);
        }
    }

    //Wrong Syntax, Varargs parameter must be last parameter
    static void varargsParameters(int... a, String string){}
    static void varargsParameters(String string, int... a){
        System.out.printf("Value of string : %s\n", string);
        System.out.printf("Number of Arguments : %d\n", a.length);
        for(int element : a){
            System.out.printf("Element : %d\n", element);
        }
    }


}
class TestVarargs{
  public static void main(String[] args) {
      Varargs.varargsParameter();
      Varargs.varargsParameter(1);
      Varargs.varargsParameter(1,2,3,4,5);
      Varargs.varargsParameters("A", 1,2,3);
  }
}
```
결과
```
Number of Arguments : 0

Number of Arguments : 1
Element : 1

Number of Arguments : 5
Element : 1
Element : 2
Element : 3
Element : 4
Element : 5

Value of string : A
Element : 1
Element : 2
Element : 3
```

[Varargs](https://www.baeldung.com/java-varargs)<br><br>
[@SafeVarargs](https://docs.oracle.com/javase/8/docs/api/java/lang/SafeVarargs.html)

#### 3. Optional<br><br>

Null 될 가능성이 있는 객체를 감싸는 래퍼 클래스

```java
class Electronics {
    public User findByUserId(String userId) {
        Optional<User> userOpt = userList
                .stream().filter(user -> userId.equals(user.getUserId()))
                .findFirst();
        if (userOpt.isPresent()) {
            return userOpt.get();
        }
        System.out.printf("'%s' Not Found\n", userId);
        return Electronic(-1);
    }
}
```

`isPresent()`<br>
`ofNullable()`<br>
`orElse()`<br>
`orElseGet()`<br>
등으로 Null 컨트롤<br><br>

### 4. Build Pattern
```java
User user = new User("Tester01","Tester01PW","010-1234-1234"...);
```
생성자를 여러개 생성<br>
매개변수 순서 중요<br>
가독성 ⬇️️️⬇️️<br><br>
자바 빈즈 패턴
```java
User user = new User();
user.setUserId = "Tester01";
user.setUserPassword = "Tester01PW";
...
```
매개 변수가 없는 생성자를 이용해 생성, setter 로 수정<br>
매서드 호출 多<br>
생성자를 쓰지 못하여 객체의 일관성이 깨질 가능성이 있음<br><br>
Build 패턴
```java
public class User {
    private String userId;
    private String userPassword;
    private String userPhoneNumber;
    private String userEmail;
    private LocalDate userBirthDate;
    private Electronics electronicDevices;
    private Long registerTime;

    private User(Builder builder) {
        this.userId = builder.userId;
        this.userPassword = builder.userPassword;
        this.userPhoneNumber = builder.userPhoneNumber;
        this.userEmail = builder.userEmail;
        this.userBirthDate = builder.userBirthDate;
        this.electronicDevices = builder.electronicDevices;
        this.registerTime = System.currentTimeMillis();
    }

    public static class Builder {
        private String userId;
        private String userPassword;
        private String userPhoneNumber;
        private String userEmail;
        private LocalDate userBirthDate;
        private Electronics electronicDevices;

        public Builder userId(String userId) {
            this.userId = userId;
            return this;
        }

        public Builder userPassword(String userPassword) {
            this.userPassword = userPassword;
            return this;
        }

        public Builder userPhoneNumber(String userPhoneNumber) {
            this.userPhoneNumber = userPhoneNumber;
            return this;
        }

        public Builder userEmail(String userEmail) {
            this.userEmail = userEmail;
            return this;
        }

        public Builder userBirthDate(LocalDate userBirthDate) {
            this.userBirthDate = userBirthDate;
            return this;
        }
        /*public Builder registerTime (Long registerTime){
            this.registerTime = registerTime;
            return this;
        }*/

        public User build() {
            return new User(this);
        }
    }
}
```

```java
User userA = new User.Builder()
        .userId("Tester01")
        .userPassword("Tester01PW")
        .userPhoneNumber(010-1234-1234)
        .userEmail("testeremail@test.com")
        .userBirthDate("86-01-01")
        .build();

User userB = new User.Builder()
        .userId("Tester02")
        .userPassword("Tester02PW")
        .userBirthDate("97-12-31")
        .build();

System.out.println(userA.toString());
System.out.println(userB.toString());
```
결과
```
    UserA [userId=Tester01, ... userPhoneNumber=010-1234-1234, userEmail=testeremail@test.com, ...]
    UserB [userId=Tester02, ... userPhoneNumber=null, userEmail=null, ...]
```
매개변수가 많을때 쓰면 유리, 선택적 매개변수 <br>
생성자를 여러개 만들지 않아도 됨<br>
코스트가 많이듬 -> Lombok @Builder<br>
