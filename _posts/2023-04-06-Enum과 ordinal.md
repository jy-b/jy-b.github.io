---
title: Enum과 ordinal
date: 2023-04-06 01:00:00 +0900
categories: [Java]
tags: [Java, Enum, Ordinal]     # TAG names should always be lowercase
---


Java의 Enum 클래스는 ordinal 이라는 메소드를 지원하며<br>
해당 Enum의 상수가 정의된 순서를 반환한다. (0부터 시작)<br>

이를 이용하여 아래와 같이 인원수를 구할 수 있다<br>
```java
public enum GameMode {
    //Trio.getNumberOfPlayer = 3
    Solo(), Duo(), Trio(), Quartet();
    
    private final int numberOfPlayer;

    public int getNumberOfPlayer(){
        return ordinal() + 1;
    }
}
```

하지만 상수의 선언 순서가 재정의 될 시 반환하는 값이 달라지며 중복 값을 가질 수 없기에 부적절 하다
```java
public enum GameMode {
  /*
    Trio.getNumberOfPlayer = 4... expect 3
    SoloVsSolo.getNumberOfPlayer = 5... expect 2
  */
  Solo(), Duo(), Quartet(), Trio(), SoloVsSolo(), DuoVsDuo();

  private final int numberOfPlayer;

  public int getNumberOfPlayer(){
    return ordinal() + 1;
  }
}
```
Java Document의 Enum-Ordinal 항목에 따르면<br><br>
***Most programmers will have no use for this method.***<br>
It is designed for use by sophisticated enum-based data structures, such as EnumSet and EnumMap.<br><br>
대부분의 프로그래머는 이 메소드를 사용할 일이 없으며<br>
EnumSet과 EnumMap과 같은 데이터 구조에 쓸 목적으로 디자인 되었다고 한다.<br>

따라서 Enum의 상수에 연결된 값은 Ordinal 메소드가 아닌 인스턴스 필드에 저장 하여 사용하여야 한다
```java
public enum GameMode {
  /*
    Trio.getNumberOfPlayer = 3
    SoloVsSolo.getNumberOfPlayer = 2
  */
  Solo(1), Duo(2), Quartet(4), Trio(3), SoloVsSolo(2), DuoVsDuo(4);

  private final int numberOfPlayer;

  GameMode(int numberOfPlayer){
     this.numberOfPlayer = numberOfPlayer;
  }
  
  public int getNumberOfPlayer() {
    return numberOfPlayer;
  }
}
```

참조 문서<br>
[Java Document / Enum](https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/Enum.html)<br>
[Effective Java / ordinal 메서드 대신 인스턴스 필드를 사용해라](https://dahye-jeong.gitbook.io/java/java/effective_java/2021-06-06-use-instant-field)
[Effective Java / ordinal 인덱싱 대신 EnumMap](https://dahye-jeong.gitbook.io/java/java/effective_java/2021-06-06-use-enummap)

기타 참고<br>
[우아한 형제 Tech Blog / Enum 활용](https://techblog.woowahan.com/2527/)
