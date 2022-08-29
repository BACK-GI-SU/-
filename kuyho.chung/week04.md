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
