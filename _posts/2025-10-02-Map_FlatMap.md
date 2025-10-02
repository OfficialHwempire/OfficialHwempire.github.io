---
date: 2025-10-02
layout: post
title: JAVA Stream API Map과 FlatMap 의 차이점
categories: [개발 공부]
tags: [JAVA]

---

# JAVA Stream API Map과 FlatMap 의 차이점

## 0. Map과 FlatMap
Java stream API 에는 대표적으로 두가지 Map 함수가 있습니다. Map, FlatMap 입니다.
Map, FlatMap 이 뭔지 그리고 각자 어떤 상황에서 사용하는 게 유용할까요?

```java

import java.util.List;
import java.util.Map;

List<String> animals = List.of("cat", "dog", "mouse", "rabbit");

public List<String> getAnimals() {
  animals.stream()
   .map(String::toUpperCase)
   .collect(Collectors.toList());
}

```
## 1. JAVA Stream Map

![image](/assets/img/stream_map_docs.png)

Java API 문서 내용입니다. 막상 저렇게 보면 뭘하는 친구인지 알기 어렵습니다. 
특정 오브젝트를 받아 다른 오브젝트를 반환하는 mapper 함수를 매개 변수로 받습니다. Stream의 각 요소에 mapper 함수를 적용해
새로운 오브젝트들을 반환 후 반환된 오브젝트의 stream을 반환합니다.

쉽게 말해 스트림의 요소를 1:1 대응하는 요소들로 전부 바꾼 스트림을 반환하는 것입니다.  


## 2. Map 코드 예시

```java
import java.util.List;
import java.util.function.Function;

public class Mapper {
    private final List<Integer> numLis = List.of(1, 2, 3, 4, 5);

    public void action() {

        numLis.stream().map(new Function<Integer, Integer>() {
            @Override
            public Integer apply(Integer integer) {
                return integer * 2;
            }
        });  ///  ===> 결과 : 2, 4, 6, 8, 10
    
    }

}
```

자바 스트림 map 코드입니다. map 함수 안에 Function 의 apply 함수를 변형해 원하는 매핑 함수를 구현할 수 있습니다.<br>
물론 대부분은 저렇게 직접 구현하지 않고 람다식 을 삽입합니다.

```java
 
 numLis.stream().map(x->x*2);

```



## 3. JAVA Stream FlatMap

![image](/assets/img/flatMap_docs.png)


JAVA API 문서 내용입니다.

map 은 우리가 흔히 생각하는 map 함수이고 스트림 API라 스트림을 반환합니다. 오브젝트를 받아 스트림으로 반환해 준 후 반환한 스트림을 Mapper 클래스로 <br>
매핑하여 스트림으로 반환합니다. 1:N 매핑이 가능합니다.



## 4. FlatMap 코드 예시

```java

import java.util.List;
import java.util.stream.Stream;

public class FlatMapper {
  private final List<Integer> numLis = List.of(1, 2, 3, 4, 5);

  public void action() {
    numLis.stream()
      .flatMap(x -> Stream.of(2*x)); /////  ===> 결과 : 2, 4, 6, 8, 10
  }
}
```

Map 함수와 달리 매게변수에 스트림을 반환하는 함수를 넣어줘야 합니다. 


## 5. Map vs FlatMap

그렇다면 Map과 FlatMap의 차이점이 무엇일까요?
Map 은 1:1 매핑 기능으로 단순한 자료 구조를 처리하는데 적합합니다. 근본적으로 자료 구조체를 변경하지 않습니다<br>
FlatMap 은 중첩 구조체에 있는 자료를 평탄화해서 단일 스트림으로 변환하는데 적합합니다.

간단하게 말하면 Map 은 상자를 다른 상자로 바꾸는 것이고, FlatMap은 상자안의 상자를 열어 그 안의 초콜릿을
나열한 것입니다.
