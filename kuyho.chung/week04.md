# 4주차

## 목차

12. 모든 클래스의 부모 클래스는 Object예요
13. 인터페이스와 추상 클래스, enum

## 12. 모든 클래스의 부모 클래스는 Object예요

Java 내에 존재하는 모든 클래스는 부모 클래스가 있다. Object라고 한다.

```java
public class InheritanceObject {
    psvm(String[] args) throws Exception {
        InheritanceObject obj = new InheritanceObject();
        sout(obj.toString());
    }
}
```

위 코드는 아무런 상속을 받지 않았지만, Object 클래스를 상속받는 것과 같다.  
클래스 내에 다른 메소드가 전혀 정의되지 않았음에도, obj는 toString() 메소드를 사용할 수 있고, 위 코드는 정상 동작한다!

앞서 상속을 다룰 때, Child는 Parent를 상속받았다. Parent는 아무것도 상속을 받지 않은 것처럼 보이지만, 사실은 Object를 상속 받았다.  
Java는 한 번에 이중 상속을 받는 것을 지원하지는 않지만, 여러 단계를 거쳐 상속을 받을 수 있다. 즉, Child는 Parent를 상속을 받았는데, Parent는 Object를 상속 받았으므로, Child 역시 Object를 상속 받은 것이 된다.

### 왜 모든 클래스는 Object를 상속받는가?

Object를 통해서 Java에서의 클래스의 기본 행동을 정의하기 때문이다. '클래스라면 이 정도는 정의되어 있어야 하고, 처리되어 있어야 한다'는 관점에서 등장한 것이 Object이다.

Object에서 기본적으로 제공하는 메소드는 크게 '객체를 처리하기 위한 메소드' 와 '쓰레드를 위한 메소드'로 나뉜다.

객체를 처리하기 위한 메소드는 아래와 같다.
|메소드|설명|
|---|---|
|protected Object clone()|객체의 복사본을 만들어 리턴한다.|
|public boolean equals(Object obj)|현재 객체와 매개 변수로 받는 객체가 같은지 비교한다.|
|protected void finalize()|현재 객체가 더 이상 필요 없어졌을 때, 가비지 컬렉터에 의해 이 메소드가 호출된다.|
|public Class<?> getClass()|현재 객체의 Class 클래스의 객체를 리턴한다.|
|public int hashCode()|객체에 대한 해시 코드 (16진수로 제공되는 객체의 메모리 주소)를 리턴한다.|
|public String toString()|객체를 문자열로 표현하는 값을 리턴한다.|

쓰레드 처리를 위한 메소드는 아래와 같다.
|메소드|설명|
|---|---|
|public void notify()|이 객체의 모니터에 대기하고 있는 단일 스레드를 깨운다.|
|public void notifyAll()|이 객체의 모니터에 대기하고 있는 모든 스레드를 꺠운다.|
|public void wait()|다른 스레드가 현재 객체에 대한 notify() 또는 notifyAll() 메소드를 호출할 때까지 현재 스레드가 대기하고 있도록 한다.|
|public void wait(long timeout)|wait()과 동일하지만, 매개 변수에 지정된 시간(ms)만큼 대기하고, 해당 시간이 지나면 스레드가 꺠어난다.|
|public void wiat(long timeout, int nanos)|wait()과 동일하지만, 시간을 나노초까지 지정할 수 있다.|

스레드 처리를 위한 메소드는 나중에 더 자세히 다루도록 한다.

### toString() 메소드

앞서 언급한 객체를 처리하기 위한 메소드는 Java 개발자라면 필수적으로 잘 이해하고 사용할 수 있어야 한다. 자주 사용하는 순서는 (책에 의하면) 다음과 같다.

- toString()
- equals()
- hashCode()
- getClass()
- clone()
- finalize()

Object에서 제공하는 메소드 중에서 가장 널리 쓰이는 메소드가 바로 toString() 메소드이다. 해당 클래스가 어떤 객체인지 쉽게 나타낼 수 있다.  
다음의 경우에 toString()이 자동으로 호출된다.

- System.out.println() 메소드에 매개 변수로 들어가는 경우
- 객체에 대하여 더하기 연산을 하는 경우

```java
public class ToString {
    psvm(String args[]) {
        ToString thisObject = new ToString();
        thisObject.toStringMethod(thisObject);
    }

    public void toStringMethod(Object obj) {
        sout(obj);                  // 객체를 그대로 출력
        sout(obj.toString());       // toString()을 통해 출력
        sout("plus " + obj);        // obj를 피연산자로 하여출력
    }

    // 결과
    // c.inheritance.ToString@1240e19d
    // c.inheritance.ToString@1240e19d
    // plus c.inheritance.ToString@1240e19d
}
```

객체를 그냥 출력하는 것과 toString()을 통해 출력하는 내용물은 같은 것을 확인할 수 있다. 또, 두 문자열을 합치는 + 연산 시에도 toString()이 자동으로 호출되여 하나의 문자열로 구성된 후 출력된다.

### toString()을 통해 출력된 객체의 내용

앞서 출력된 내용 (c.inheritance.ToString@1240e19d)은 무엇일까?
Object 클래스에 구현되어 있는 toString() 메소드는 [다음과 같다](<https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Object.html#toString()>).

```java
getClass().getName() + '@' + Integer.toHexString(hashCode())
```

앞에서부터 차근차근 살펴보자.  
`getClass().getName()`는 현재 클래스의 패키지 이름과 클래스 이름을 호출한다. 위 내용에서는 `c.inheritance.InheritanceObject`가 여기에 속한다.  
다음으로 `'@'` 문자를 붙이는데, 단순한 구분자 역할을 한다.  
마지막 부분에는, 앞서 다루었던 Object의 객체를 다루는 메소드 중 `hashCode()`가 등장하였다. 객체의 해시값을 호출하는 부분인데, `hashCode()`는 Integer 형태의 값을 리턴하고, `toHexString()`을 통해 16진수로 변환한다.

여기서 등장하는 해시값은 객체를 구별하는 역할을 하지만, 우리가 보기에는 그다지 유용한 결과가 아니다.
우리가 toString() 메소드를 제대로 사용하려면, 이를 Overriding을 통해 확장하여 사용해야 한다.

```java
public class ToString {
    psvm(String args[]) {
        ToString thisObject = new ToString();
        thisObject.toStringMethod(thisObject);
    }

    public String toString() {
        return "ToString class";
    }
}
```

위처럼 `toString()` 메소드를 Overriding을 통해 재정의하여 사용하였다.  
위처럼 ToString 클래스를 작성한 후 아까처럼 세 가지 경우에 대해 쳑하면 아래와 같이 나온다.

```
// ToString class
// ToString class
// plus ToString class
```

항상 toString() 메소드를 Overriding 하여 사용할 필요는 없지만, 필요한 경우에는 사용하도록 하자.  
예를 들어 아래와 같은 Dto를 정의했다면,

```java
public class MemberDTO {
    public String name;
    public String phone;
    public String email;
}
```

DTO 내의 멤버 변수룰 각각 출력하려면, 매우 번거롭다.  
하지만 toString()을 아래와 같이 Overriding 한다면?

```java
public Class MemberDTO {
    ...
    public String toString() {
        return "Name=" + name + " Phone=" + phone+ " Email=" + email;
    }
}
```

매우 편리하게 사용할 수 있다.  
IDE가 편리하게 도와줄 테니, 가능하면 `toString()` 메소드를 Overriding하여 사용하자.

### 객체 비교는 '=='? equals()를 사용하자

기본적으로 Java를 포함한 다양한 언어에서는 '==' 연산자를 통해 두 피연산자가 같은지 비교한다. 결과는 true 혹은 false가 된다.  
Java에서 알아두어야 할 점은, '==' 연산자는 기본 자료형에서만 사용할 수 있다. 즉, 참조 자료형에서는 사용하면 안 된다. 이는 String 역시 포함이다.

아래와 같은 코드가 있을 때, 어떤 내용이 출력될까?

```java
public class Equals {
    psvm(String args[]) {
        Equals thisObject = new Equals();
        thisObject.equalMethod();
    }

    public void equalMethod() {
        MemberDTO obj1 = new MemberDTO("Katfun");
        MemberDTO obj2 = new MemberDTO("Katfun");

        if (obj1 == obj2) {
            soutv("obj1 and obj2 are equal.");
        }
        else {
            soutv("obj1 and obj2 are different.");
        }
    }
}
```

출력되는 내용은 아래와 같다.

```
obj1 and obj2 are different.
```

왜 다를까? 앞서 언급한 바와 같이, '==' 연산자는 기본 자료형에만 사용할 수 있다.  
각 객체는 각자의 생성자를 통해 만들어졌다. 두 객체의 멤버 변수의 값은 서로 동일하지만, 두 객체는 주소값이 다르므로 false를 얻는다.

그렇다면 equals()를 사용하면 두 객체가 같을까?

```java
        ...
        if (obj1.equals(obj2)) {
            soutv("obj1 and obj2 are equal.");
        }
        else {
            soutv("obj1 and obj2 are different.");
        }
    }
}
```

아쉽게도 이번에도 아래와 같이 출력된다.

```
obj1 and obj2 are different.
```

왜 이런 결과가 나오냐 하면, 아직 equals() 메소드를 Overriding 하지 않았기 때문이다.  
equals() 메소드는 Java 17 기준 [링크와 같이 정의되어 있다](<https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Object.html#equals(java.lang.Object)>).  
해당 메소드를 IDE를 통해 Overriding하여 사용하면, 두 객채의 동일 여부를 비교할 수 있다.  
한 가지 짚고 넘어갈 것은, toString() 메소드를 Overriding 할 때는 hashCode() 메소드 역시 Overriding 해야 한다.

### hashCode 메소드

hashCode() 메소드는 기본적으로 객체의 메모리 주소를 16진수로 리턴한다.  
만약 두 객체가 동일하다면, 두 객체의 메모리 주소 값, 즉 hashCode() 값은 반드시 동일해야 한다.  
따라서 앞서 말한 바와 같이, equals() 메소드를 Overriding 할 때는, 비교하는 두 객체의 hashCode() 값이 동일하도록 hashCode() 메소드 역시 Overriding 해야 한다는 것이다.  
다만 hashCode() 메소드를 Overriding 할 때 [지켜야 할 제약](<https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Object.html#hashCode()>)이 있다.  
따라서 가급적이면 equals() 또는 hashCode() 메소드는 직접 작성하지 말고, IDE에서 제공하는 기능을 사용하도록 하자.

## 13. 인터페이스와 추상 클래스, enum

### 메소드 내용이 없는 interface

지금까지 Java 파일은 `.java` 확장자를 가졌다. 이런 파일을 컴파일 하면 `.class` 파일을 얻었고, 이것이 JRE를 통해 실행되었다.  
하지만 `.java` 파일로부터만 `.class` 파일을 생성할 수 있는 것은 아니다. Java에는 `interface` 클래스와 `abstract` 클래스가 존재한다.  
시스템을 개발할 때의 절차를 잘 이해하면 이러한 클래스를 활용하여 프로그램을 효율적으로 작성할 수 있다.

프로그램 개발의 일반적인 절차는 아래와 같다.

- 분석
- 설계
- 개발 및 테스트
- 시스템 릴리즈

물론 이러한 절차가 절대적인 것은 아니다. 프로그램 개발에는 다양한 방법론이 존재하며, 위의 4단계는 최대한 각 단계를 대중적으로 표현한 것이다.

#### 분석

시스템을 개발하기 위해서는 주어진 요구 사항을 분석해야 한다. SI/SM에서는 고객이 될 것이고, 자체 서비스 기업에서는 기획자가 이러한 역할을 수행한다. 시스템이 어떻게 개발되어야 하며, 요구 조건은 무엇인지 정의하고, 현실적인 여건을 분석한다.

#### 설계

앞서 분석 단계에서 분석한 내용을 토대로, 개발을 어떻게 진행할 것인지 설계한다.  
DB 테이블의 구조와 관계, 메소드의 구성 등을 설계한다.

#### 개발 및 테스트

앞서 분석하고 설계한 내용을 바탕으로 실제 개발을 한다. 시스템에서 필요로 하는 기능을 만들고, 정상적으로 동작하는지 검증한다.

#### 시스템 릴리즈

개발한 내용을 실제 사용자가 사용할 수 있도록 제공한다. 이후 발생하는 결함이나 개선 사항 등은 운영/유지보수 단계를 거치며 개선한다.

이러한 과정을 겪을 때, 분석하고 설계하는 과정은 단순히 문서에 작성하는 과정으로 끝나지 않는다. 만약 변수가 새로 추가되거나, 설계한 메소드 내용이 변경된다면? 문서에만 기록한다면, 나중에 문서도 수정해야 한다.  
interface라는 개념은 이 때문에 등장했다. 실제 개발을 진행하기 이전에 메소드의 이름, 매개 변수 등을 미리 정해둘 수 있다. 실제 개발 단계에는 해당 interface의 구현체만 작성하면 된다.

interface나 abstract를 사용해야 하는 이유는 이뿐만이 아니다.  
우리가 TV 리모콘을 보면, '리모콘이구나!' 하고 바로 인식할 수 있다. 실제로 내부가 어떻게 구현되어 있는지는 잘 모르지만, 어떻게 사용하면 좋을지 바로 파악할 수 있다.  
interface나 abstract가 이런 역할을 한다.

```java
public boolean equals (Object a, Object b);
```

위와 같은 코드가 있다면, 이름과 매개 변수만 보고서도 이 메소드가 어떤 역할을 하는지 여럼풋이 짐작할 수 있다. 개발자는 이 인터페이스를 호출하여 의도대로 사용하고, 이 인터페이스의 실제 구현체는 (직접 들여다 보고 이해하면 더 좋지만) 확인하지 않아도 사용할 수 있다.

좋은 예가 바로 DAO이다. DB에 요청을 보내고 원하는 결과를 받게 되는데, 이 역할을 하는 메소드가 DBMS에 영향을 받을까?

```java
public List<Map<String, String> > PostsDAO (List<String> userId);
```

게시글의 고유 ID list를 넘겨 주고, DB로부터 해당 글들에 대한 정보의 list를 받아 오는 interface이다. 내부적으로 이 메소드가 Oracle에 접근하든, PostgreSQL에 접근하든, 사용자는 상관하지 않는다. 그저 적당한 요청을 보내고 원하는 결과를 받으면 된다. 이것이 interface의 장점이다.

#### 요약

interface와 abstract를 사용해야 하는 이유는 아래와 같다.

- 설계 시에 interface를 작성해 두면, 개발할 때 기능 구현에만 집중할 수 있다.
- 여러 명이 개발할 때, 메소드와 변수의 이름의 파편화를 최소화할 수 있다.
- 공통적인 interface와 abstract 클래스를 선언해 두면, 선언과 구현을 구분할 수 있다.

### 인터페이스를 직접 만들어보자

인터페이스의 예시는 아래와 같다. 실제 코드는 작성하지 않지만, 어떤 변수나 메소드가 있었는지 정의할 때 사용한다.

```java
package c.service;

import c.model.PostsDAO;

public interface PostsManager {
    public boolean getPost(String postId);
    public boolean addPost(PostsDAO posts);
    public boolean updatePost(PostsDAO posts);
    public boolean deletePost(String postId);
}
```

앞서 다룬 클래스와 가장 다른 점은, `public class ...`로 시작하는 것이 아니라 `public interface...`로 시작한다는 점이다. 해당 클래스가 interface임을 나타내는 것이다.  
다음으로는, 구현 내용이 없다. 앞서 언급한 바와 같이, **실제 코드는 작성하지 않는다**.  
위 내용이 기존의 클래스와 가장 다른 점이다. 위처럼 단순히 필요한 멤버 변수 및 메소드를 정의하고, 필요한 경우 해당 메소드를 호출하게 된다.

그렇다면 실제 구현은 어떻게 하면 될까?  
아래와 같은 클래스를 작성해 볼 수 있다.

```java
package c.service;

public class PostsManagerImpl implements PostsManager {

    @Override
    public boolean getPost(String postId) {

    }

    @Override
    public boolean addPost(PostsDAO posts) {

    }

    @Override
    public boolean updatePost(PostsDAO posts) {

    }

    @Override
    public boolean deletePost(String postId) {

    }
}
```

interface에서는 선언을 하고, impl 클래스에서는 구현을 하였다 (선언과 구현의 분리).  
보통 interface의 구현체는 interface의 클래스명 + Impl 와 같은 식으로 알아보기 쉽게 이름을 정한다 (필수적이지는 않다).  
중요한 점은 `public class PostsManagerImpl implements PostsManager` 라는 부분이다. PostsManager라는 interface를 콕 집어서, 해당 인터페이스를 PostsManagerImpl이라는 이름을 통해 구현하겠다는 선언이다.  
구현체는 interface의 모든 메소드를 구현해야 성공적으로 컴파일된다. 메소드명, 리턴 타입과 멤버 변수가 모두 interface와 동일해야 하며, 위에 @Override라는 어노테이션이 붙는다 (나중에 자세히 다룰 것).

### 일부 완성되어 있는 abstract 클래스

### final 키워드

### enum 클래스라는 상수의 집합도 있다면

### enum을 보다 제대로 사용하기

### enum 클래스의 부모는 무조건 java.lang.Enum이다
