# 3주차

## 목차
10. 자바는 상속이라는 것이 있어요
11. 매번 만들기 귀찮은데 누가 만들어 놓은거 쓸 수 없나요?

## 10. 자바는 상속이라는 것이 있어요
### 정의 (자바에서 상속이란?)

아래와 같은 코드 예시를 보자.

```java
public Class Parent {
	public Parent() {
		printf("parent constructor");
	}

	public void printName() {
		printf("parent printName");
	}
}

public Class Child extends Parent {
	
	public Child(){
		printf("child constructor");
	}
}
```


Child 클래스가 Parent 클래스를 상속받았다.

상속을 받는다는 것은, 부모 클래스의 구성 요소 (멤버 변수, 메소드 등)을 그대로 자식 클래스도 사용할 수 있게 되는 것을 의미한다. (public, protected만 사용 가능)

상속 받은 Child에서 printName()을 실행한다면?

> “parent printName”

상속으로부터 얻는 장점은 아래와 같다.

-   코드의 재사용성을 높일 수 있다 (같은 일을 이중, 삼중으로 하지 않아도 된다)
-   설계상의 이점을 가질 수 있다 (설계의 난이도, 시간 등)

### 상속과 생성자

부모 클래스에 기본 생성자가 없다면?

Parent 클래스를 상속 받은 Child 클래스는 생성자가 호출될 때 Parent() 라는 기본 생성자를 찾는다.

만약 부모 클래스에 기본 생성자가 없다면, 컴파일 에러가 발생한다.

아래와 같이 해결할 수 있다.

-   부모 클래스에 (매개 변수가 없는) 기본 생성자를 만든다.
-   자식 클래스에서 부모 클래스의 생성자를 명시적으로 지정하는 super()를 사용한다.

super()란?

부모 클래스의 생성자를 호출한다는 것을 의미한다.

```java
public Class Child extends Parent {

	public Child() {
		super("Child");
		printf("Child Constructor");
	}

}
```

Parent 클래스의 생성자 중 String 타입을 매개 변수로 받을 수 있는 생성자를 찾는다.

### 메소드 Overriding

정의: 부모 클래스에 존재하는 메소드를 자식 클래스가 같은 이름으로 재정의하여 사용하는 것

-   “동일하게 선언되어 있다" - “동일한 시그니처 (signature)를 가진다”
-   단, 리턴 타입을 마음대로 바꿀 수는 없다. 메소드 내부의 구현 내용만 바꿀 수 있다고 생각하면 된다.
-   부모 클래스의 접근 제어자를 자식 클래스에서 확대하는 것은 문제가 없다. 축소하는 것은 문제가 된다.

### 참조 자료형의 형 변환
한 줄 요약: 부모 형태의 참조 자료형에 자식 객체를 할당할 수 있고, 자식 형태의 참조 자료형에 부모 객체를 할당할 수 없다.
```java
ParentCasting obj = new ChildCasting();
```
```
ChildCasting obj2 = new ParentCasting();
```
부모가 사용하는 기능은 자식이 다 가지고 있다.
자식이 사용하는 기능은 부모가 가지고 있지 않을 수도 있다.
따라서 자바 컴파일러 단에서, 자식 객체를 생성할 때 부모 생성자를 사용할 수 없다고 컴파일 에러를 발생시킨다.

기본 자료형의 형변환에서도,
```java
int intValue = 10;
long longValue = 10l;

long casted1 = intValue;
int casted2 = (int)longValue;
```

자료형의 범위가 더 넓은 쪽을 작은 쪽에 할당할 때 (이 경우에는 long이 더 넓다), 위처럼 개발자가 직접 형변환을 명시해 주어야 한다.

따라서 참조 자료형과 상속 관계의 클래스 역시 마찬가지이다.
```java
ParentCasting parent = new ParentCasting();
ChildCasting child = new ChildCasting();

ParentCasting parent2 = child;
ChildCasting child2 = (ChildCasting)parent;
```

위처럼 코드를 작성하면, 컴파일 에러는 벗어나지만 여전히 안 된다.

```java
ChildCasting child = new ChildCasting();

ParentCasting parent2 = child;
ChildCasting child2 = (ChildCasting)parent2;
```

무슨 차이인가?
앞에서는 parent를 ParentCasting의 객체로 초기화했다. 따라서 parent는 ParentCasting 클래스의 객체고, 따라서 위처럼 형변환을 해도 사용할 수 없다.
밑에서는 parent2를 child로 초기화 했는데, 이 부분은 겉으로는 ParentCasting의 객체 같이 보여도, 사실은 ChildCasting의 객체이기 때문에 위처럼 사용하는데 문제가 없다.

### Polymorphism
다형성이라고도 한다.
Overriding과 형변환 개념을 이해했다면, polymorphism도 이해하기 쉽다.

```java
public class ClassOther Extends Parent {

	public ChildOther() {
	
	}

	public void printName() {
		printf("ChildOther - printName()");
	}

}
```

위처럼 Parent의 자식 클래스가 또 생겼다.
이렇게 자식 클래스를 원하는 만큼 많이 만들 수 있다.

이들을 전부 아래와 같이 선언한다면?
```
Parent parent1 = new Parent();
Parent parent2 = new Child();
Parent parent3 = new ChildOther();

parent1.printName();
parent2.printName();
parent3.printName();

```

전부 동일하게 Parent로 선언했지만, 필요한 구현체 (자신 및 자식 클래스)를 갖다 붙이면 그대로 쓸 수 있다.

> 형변환 관련해서 추가 내용 정리가 필요함. 파악한 내용이 다소 부실함.

## 11. 매번 만들기 귀찮은데 누가 만들어 놓은 거 쓸 수 없나요?
사람들이 미리 만들어 놓은 클래스는 아주 많다.
이들을 참조하는 문서를 API (Application Programming Interface) 라고 한다.
HTML 형식으로 구성되어 있다.
API에 명시되도록 주석을 잘 달아 주고, javadoc이라는 명령어를 실행해 주면 API 문서가 생성된다.

### 클래스 및 인터페이스의 상세 정보 화면


### Deprecated
메소드 중에 @Deprecated라는 어노테이션이 붙어 있거나, 선언되어 있는 경우가 있다.
보통 해당 메소드가 오래 되어 보안상 취약하거나, 더 나은 대체품이 있는 경우, 하지만 하위 호환성을 위해 당장 해당 메소드를 제거할 수는 없는 경우에 Deprecated를 사용한다.
실행하면 Error가 발생하지는 않지만, Warning이 발생한다. 가능하면 쓰지 말라는 내용이다.
이런 Warning마저도 없으면 Deprecated를 달아 둬도 못 보게 된다.

### Header와 Footer에 있는 링크
