# 2주차

## 목차

8. 참조 자료형에 대해서 더 자세히 알아봅시다
9. 자바를 배우면 패키지와 접근 제어자는 꼭 알아야 해요

## 8. 참조 자료형에 대해서 더 자세히 알아봅시다

### 정의

- 기본 자료형: byte, short, int, long, float, double, char, boolean
- 참조 자료형: 나머지 다!

둘의 차이점은 new를 사용해서 객체를 생성하는지 여부. (유일한 예외는 String으로, String은 new 없이도 객체를 생성할 수 있다.)

참조 자료형에 사용할 수 있는 연산자는 값을 할당하기 위한 등호 (=) 뿐이다.

new 뒤에 오는 것은 생성자다.

### 기본 생성자

- 기본 생성자는 클래스를 만들 때, 따로 지정하지 않아도 기본으로 만들어진다.
  - 단, 다른 생성자가 존재한다면, 기본 생성자는 만들어지지 않는다.
- 기본 생성자는 아무런 매개 변수를 받지 않는다.
- 아래와 같은 형식을 가진다.

```java
public Class DefConstructor {

	// 생성자
	public DefConstructor() {

	}

}
```

- 아래와 같은 경우에는 컴파일 에러가 발생한다.

```java
public class DefConstructor {

	// argument가 존재하는 생성자
	public DefConstructor(String input) {

	}

	public static void main(String args[]) {

		DefConstrctor defCon = new DefConStructor();
		// 컴파일 에러가 발생한다.
	}
}
```

### 생성자는 왜 필요할까?

- 생성자의 역할: 객체 (또는 인스턴스)를 생성함
- 생성자는 리턴 타입이 없고 (클래스의 객체를 리턴한다), 클래스와 동일하게 이름을 가지는 것 외에는 메소드와 유사하다.
- 생성자는 통상적으로 클래스 내에서 가장 위에 선언한다.
- 생성자가 반드시 필요한가? 그렇지는 않다. 생성자 없이도 객체를 얻을 수 있는 클래스도 존재한다.

### 생성자는 몇 개까지 만들 수 있을까?

- 생성자의 개수는 상관이 없다.
- 다만 유지 보수 관점에서, 꼭 필요한 생성자만 최소로 생성하자.
- Dto를 이용한 예시

```java
public Class PostsDto {

	public String postId;
	public String title;
	public String authorId;
	public Date createDate;
	public Date lastEditDate;
	public String content;
	public List<String> tags;

	// constructors
	public PostsDto() {
			// 아무것도 모를 때
	}

	public PostsDto(String postId) {
		// postId만 알 때
		this.postId = postId;
	}

	public PostsDto(String postId, String title) {
		// postId, title을 알 때
		this.postId = postId;
		this.title = title;
	}
}

@Controller
public Class PostsController {

	public PostsDto postsDto() {

		PostsDto dto = new PostDto();

		return dto;
	}

}
```

- this: 객체의 변수와 매개 변수의 이름이 동일할 때, 변수를 구분하기 위해 사용
- 위처럼 상황마다 필요한 생성자를 사용하여 객체를 생성할 수 있다.

### 이 객체의 변수와 매개 변수를 구분하기 위한 this

- this: ‘이 객체' 라는 뜻을 가진다.
- 만약 위의 생성자에서 this가 없었다면?

```java
public Class PostsDto {

	public String postId;

	public PostsDto(String postId) {

		postId = postId; // javac는 변수를 구분할 수 없다.
	}

}
```

- 해결책
  1. 변수의 이름을 다르게 한다. - 코드를 읽다 보면 헷갈리기 마련
  2. this 예약어를 사용한다.

```java
public Class PostsDto {

	public String postId;

	public PostsDto(String postId) {

		this.postId = postId; // 멤버변수 postId에 매개변수로 받은 postId의 값을 할당해 준다.
	}

}
```

### 메소드 Overloading

앞에서 한 클래스 내에 같은 이름을 가지는 생성자를 여러 개 만들 수 있었다 (물론 받는 매개 변수는 각자 다르다).

메소드도 마찬가지로, 같은 이름을 가지고 매개 변수만 다르게 하여 여러 개를 만들 수 있다.

```java
public Class MethodOverloading {

	psvm(String args[]) {
		MethodOverloading overloading = new MethodOverloading();
	}

	// 아래의 메소드는 모두 다른 메소드이다.

	public void print(int name) {
	}

	public void print(String name) {
	}

	public void print(int intName, String stringName) {
	}

	public void print(String stringName, int intName) {
	}
}
```

- 즉, 매개변수가 다르면 별개의 메소드로 인식한다.
- 매개변수의 종류와 이름이 같아도, 순서가 다르면 별개의 메소드로 인식한다.
- 좋은 예시: System.out.println()
  - int를 던져 줘도, String을 던져 줘도, 알아서 잘 출력해 준다. 이것이 overloading의 장점!

### 메소드에서 값 넘겨주기

메소드의 수행과 종료 - 메소드가 종료되는 조건은 아래와 같다.

- 메소드 내의 모든 명령 (문장)이 실행되었을 때
- return 문장에 도달했을 때
- 예외 (Exception)가 발생 (throw)했을 때

메소드의 리턴 타입은 선언 시에 “자료형” 및 void 중 하나를 지정하게 된다.

```java
public Class MethodReturns {

	public String name(String input) {
		return input;
	}

	public int id(int input) {
		return input;
	}
	public void dump() {
		return;
	}
}
```

- void를 제외하고, 예상한 자료형의 값을 리턴하지 않는 경우에는 컴파일 에러가 발생한다.
- void에서 return을 만나면 메소드가 종료된다.

### Static 메소드와 일반 메소드의 차이

Static은 객체를 새로 생성하지 않아도 메소드를 호출할 수 있게 해 준다.

Static 메소드는 “클래스 변수”에만 사용할 수 있다.

단, 함부로 사용하면 안 된다! 클래스 변수가 되면, 모든 객체에서 하나의 값만을 바라보게 된다.

### Static 블록

어떤 클래스의 객체가 생성되면서, 딱 한 번만 호출되어야 하는 코드가 있다면, static 블록을 사용해보자.

```java
static {

}
```

이 static 블록은 객체가 생성되기 전에 딱 한 번만 호출되고, 그 이후에는 호출하려고 해도 호출할 수 없다.

클래스 내에 선언되어 있어야 하며, 메소드 내에서는 선언할 수 없다.

### Pass by Value, Pass by Reference

- Pass by Value: 오리지널 값을 복사해서 전달한다. 오리지널 값에는 영향을 미치지 않는다.
- Pass by Reference: 오리지널 값의 주소를 전달한다.

```java
public class ReferencePass {

	psvm(String args[]) {
		ReferencePass reference = new ReferencePass();
		reference.callBassByReference;
	}

	public void callPassByReference() {
		printf("member.name: " + member.name);
	}
}
```

### 매개 변수를 지정하는 특이한 방법

매개 변수의 개수가 매번 다르고, 호출할 때마다 바뀐다면?

```java
public class MethodVarags {

	public void calculateNumbers(int...numbers) {
		int total = 0;

		for (int number : numbers) {
			total += number;
		}
		printf(total);
	}
}

// 호출 시
varags.calculateNumbers(1);
varags.calculateNumbers(1,2);
varags.calculateNumbers(1,2,3);
varags.calculateNumbers(1,2,3,4);
```

위 방법은 하나의 메소드에서 한 번만 사용 가능하고, 여러 매개 변수가 있다면 가장 마지막 순서로 선언해야 한다. 그렇지 않는다면 컴파일 에러가 발생한다.

## 9. 자바를 배우면 패키지와 접근 제어자는 꼭 알아야 해요

### 정의

Java의 package란, 작성한 클래스를 구분 짓는 개념이다 (폴더와 유사).

트리 형태로 계층 관계를 나타낸다.

장점:

- 이름이 중복됨으로써 생기는 문제점을 방지한다
- 각 클래스의 역할을 나타낸다

자바 패키지의 선언

- 소스의 가장 첫 줄에 있어야 한다. 그렇지 않은 경우 컴파일 에러가 발생한다.
- 패키지 선언은 소스 하나당 한 번만 가능하다.
- 패키지 이름과 위치한 폴더 이름이 같아야 한다. 그렇지 않은 경우 해당 패키지의 위치를 찾지 못해 컴파일 에러가 발생한다.

### 패키지 이름은 이렇게 지어요

기본적으로 정해져 있는 규칙

| 패키지 시작 이름 | 내용                                       |
| ---------------- | ------------------------------------------ |
| java             | 자바 기본 패키지 (Java 벤더에서 개발)      |
| javax            | 자바 확장 패키지 (Java 벤더에서 개발)      |
| org              | 일반적으로 비영리단체 (오픈 소스)의 패키지 |
| com              | 일반적으로 영리단체 (기업)의 패키지        |

- 패키지 이름은 모두 소문자로 지정해야 한다. 반드시는 아니지만 암묵적으로 지켜지는 룰이다.
- 자바의 예약어를 사용하면 절대 안 된다. (com.int.util) 와 같은 형식

상위 패키지 이름이 정해지면, 하위 패키지 이름을 정하면 된다.

### import를 이용하여 다른 패키지에 접근하기

자바에서는 통상적으로, 같은 패키지 내에 있는 클래스들과 java.lang 내의 클래스에만 접근할 수 있다.

다른 패키지에 있는 클래스에 접근하려면, import를 통해 해당 패키지를 불러올 필요가 있다.

```java
import com.tistory.katfun.crud.comments.dto.CommentsSaveDto;

@RestController
public class CommentsController {

		// 댓글 추가
    @PostMapping("/posts/{postId}/comments/{commentId}")
    public Long addComment(
            @RequestBody CommentsSaveRequestDto requestDto,
            @PathVariable Long postId,
            @PathVariable Long commentId) {
        return commentsService.saveComment(requestDto);
    }

}
```

특정 패키지 내의 모든 클래스를 import 한다면, import c.javapackage.sub.\* 와 같은 식으로 지정할 수 있다.

패키지를 import 할 때는, 해당 패키지 내의 클래스만 import한다. 해당 패키지 내에 있는 하위 패키지 내에 있는 클래스는 import하지 않는다.

### import를 이용하여 다른 패키지에 접근하기 - import static

JDK 5에서 추가되었다.

static한 변수와 static 메소드를 사용할 때 필요하다.

### 자바의 접근 제어자

Java에는 4개의 접근 제어자가 있다.

- public: 누구나 접근할 수 있다.
- protected: 같은 패키지 내에 있거나 상속받은 경우에만 접근할 수 있다.
- package-private: 아무런 접근 제어자를 적지 않을 때와 같다. 같은 패키지 내에 있을 경우에만 접근할 수 있다.
- private: 같은 클래스 내에서만 접근할 수 있다.

접근 제어자는 여러 개발자가 함께 개발을 진행할 때, 필요한 만큼만 기능을 노출하도록 제어하는 역할을 한다.

```java
public Class Example {

	private int count;

	public void setCount(int count) {
		this.count = count;
	}

	public int getCount() {
		return this.count;
	}

}
```

### 클래스 접근 제어자 선언할 때의 유의점

```java
package c.javapackage;

public Class Example {

}

class PublicSecondClass {

}
```

위와 같은 경우에는 컴파일 시 문제가 발생하지 않는다.

```java
package c.javapackage;

public Class Example {

}

public class PublicSecondClass {

}
```

위와 같은 경우에는 컴파일 에러가 발생한다.

public으로 선언된 클래스의 이름은 해당 소스 파일의 이름과 동일해야 한다.
