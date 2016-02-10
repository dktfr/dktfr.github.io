---
layout: post
title:  "Effective Java - Chapter 1 객체의 생성과 삭제"
date:   2016-02-07 18:52:21
category: tech
comments: true
---
# 객체의 생성과 삭제

## 1.생성자 대신 Static Factory 메서드를 이용할 수 있는지 고려하라
* Static Factory 메서드 : 생성자 대신 객체를 생성해주는 static 메서드.
* ex

{% highlight java %}
public class A {
	public static A getInstance() {
		return new A();
	}
}
{% endhighlight %}

* 장점
	* 생성자와는 달리 메서드의 이름을 지정해 줄 수 있어서 코드의 가독성이 올라간다.
	* 생성자처럼 매번 새로운 객체를 생성할 필요는 없다. 동일한 객체의 요청이 잦은 경우에는 객체를 생성해두고 캐싱한 후에 필요할 때마다 캐싱된 객체를 반환한다.
	* 생성자와는 달리 return 타입이 자유로워서 유연한 코드의 작성이 가능하다.
	* 형인자 자료형(Parameterized Type) 객체를 만들 때 편하다.
		* 형인자 자료형 갹채 : 제네릭 타입을 선언한 객체
		* ex1 생성자를 이용한 생성
		* ex2 Static Factory 메서드를 이용한 생성
		* ex3 JDK 1.7부터는 생성자로 객체 생성시 Type을 생략 가능하다.

{% highlight java %}
// ex1 생성자 호출 시 Type을 반드시 명시해줘야 한다.
Map<String, Object> map = new HashMap<String, Object>();

// ex2 Static Factory 메서드를 이용한 생성
public static <K, V> HashMap<K, V> newInstance() {
	return new HashMap<K, V>();
}
Map<String, Object> map = HashMap.newInstance();

// ex3 JDK 1.7
Map<String, Object> map = new HashMap<>();
{% endhighlight %}

* 단점
	* 모든 생성자를 private으로 감추고 Static Factory 메서드만 지원할 경우 서브클래스를 생성할 수 없다.
	* Static Factory 메서드가 다른 일반 Static 메서드와 확연히 구분되지 않는다.

## 2.생성자 인자가 많을 때는 Builder 패턴 적용을 고려하라
* Static Factory 메서드, 생성자는 선택적 인자가 많은 상황에 적절한 대응이 어렵다.
* 대안
	1. 점층적 생성자 패턴
		* 모든 선택적 인자에 대한 생성자를 준비하여 필요한 생성자를 호출하도록 하는 방법.
		* 인자가 많아지면 코드 가독성이 떨어진다.

	2. 자바빈 패턴
		* 인자없는 생성자로 객체를 생성한 뒤 setter 메서드로 각 인자를 넣는 방법.
		* 1회성 함수 호출로 객체 생성을 완료할 수 없으므로 객체 일관성이 일시적으로 깨진다.
		* 변경 불가능(immutable) 객체를 만들 수 없는 단점이 있다.

	3. Builder 패턴
		* 별도의 빌더 객체가 선택적 인자들을 받아서 객체를 생성하도록 하는 방법

{% highlight java %}
// Builder 패턴 예시
public class A {
	private final int requiredField1;
	private final int requiredField2;
	private final int optionalField1;
	private final int optionalField2;
	private final int optionalField3;

	public static class Builder {
		// 필수 멤버
		private final int requiredField1;
		private final int requiredField2;
		// 선택 멤버
		private int optionalField1 = 0;
		private int optionalField2 = 0;
		private int optionalField3 = 0;

		public Builder(int required1, int required2) {
			this.requiredField1 = required1;
			this.requiredField2 = required2;
		}

		public Builder optionalField1(int optional1) {
			this.optionalField1 = optional1;
			return this;
		}
		public Builder optionalField2(int optional2) {
			this.optionalField2 = optional2;
			return this;
		}
		public Builder optionalField3(int optional3) {
			this.optionalField3 = optional3;
			return this;
		}

		public A build() {
			return new A(this);
		}
	}

	public A(Builder builder) {
		this.requiredField1 = builder.requiredField1;
		this.requiredField2 = builder.requiredField2;
		this.optionalField1 = builder.optionalField1;
		this.optionalField2 = builder.optionalField2;
		this.optionalField3 = builder.optionalField3;
	}
}

// 이용
A obj = new A.Builder(1, 2).optionalField1(3).optionalField2(4).optionalField3(5).build();
{% endhighlight %}

* Builder 패턴 장점
	* 인자에 불변식을 적용할 수 있다.
	* 빌더 객체는 여러개의 가변인자를 받을 수 있다.
	* Static Factory 메서드나 생성자보다 유연하다.
* Builder 패턴 단점
	* 객체를 생성하기 위해 빌더 객체를 생성하는 오버헤드가 있다.
	* 빌더 패턴은 많은 코드를 요구하기 때문에 인자가 충분히 많은 상황에서 사용해야 한다.

## 3.싱글턴 패턴을 사용할 때, private 생성자나 enum 자료형으로 설계하라
* 싱글턴 패턴의 단점
	* 싱글턴 객체를 Mock으로 대체하기가 어렵기 때문에 테스트가 어렵다.
	* 생성자를 private으로 만들더라도 리플렉션 API를 이용하면 private 생성자를 호출할 수 있다.
	* 직렬화에 대한 처리를 해주어야 한다.

{% highlight java %}
//Private으로 선언된 생성자 호출하는 예제
public class PrivateInvoker {
	public static void main(String[] args) throws Exception {
		//리플렉션 setAccessible 메서드를 통해 private으로 선언된 생성자 호출 권한을 획득
		Constructor<?> con = Private.class.getDeclaredConstructor()[0];
		con.setAccessible(true);
		Private p = (Private) con.newInstance();
	}
}

class Private {
	private Private() {
		System.out.println("이것은 private 생성자 입니다.");
	}
}
{% endhighlight %}

* 싱글턴 패턴 구현방법 3가지
	1. private 생성자 + public static 필드
	2. private 생성자 + static factory 메서드
	3. enum 자료형(JDK 1.5부터 가능)

{% highlight java %}
// 1.private 생성자 + public static 필드
public class SingletonClass {
	public static final SingletonClass INSTANCE = new SingletonClass();
	private SingletonClass() {
		...
	}
	public void leaveTheBuilding() {
		...
	}
}

// 2.private 생성자 + static factory 메서드
public class SingletonClass {
	private static final SingletonClass INSTANCE = new SingletonClass();
	public static SingletonClass getInstance() {
		return INSTANCE;
	}
	private SingletonClass() {
		...
	}
	public void leaveTheBuilding() {
		...
	}
}

// 3.enum 자료형
public enum SingletonEnum {
	INSTANCE;
	
	public void leaveTheBuilding() {
		...
	}
}
{% endhighlight %}

## 4.객체 생성이 필요 없을 때는 private 생성자를 사용하라
* static field들이 모여있는 클래스를 설계할 경우에는 객체를 생성할 필요가 없다.
* 객체 생성을 못하게 하려면 private 생성자를 클래스에 명시적으로 선언해 준다.
* 필요없는 생성자를 선언한 것이므로 Javadoc 등으로 주석처리를 해두면 좋다.

{% highlight java %}
public class UtilityClass {
	// 기본 생성자가 자동 생성되지 않도록 하여 객체 생성 방지
	private UtilityClass() {
		throw new AssertionError();
	}
}
{% endhighlight %}

## 5.불필요한 객체는 만들지 말라
* 기능적으로 동일한 객체는 필요할 때마다 만드는 것보다 캐싱하여 재사용하는 편이 낫다.
	* Static 초기화 블록을 이용할 것을 고려한다.
* Wrapper 클래스의 Auto Boxing기능을 이용하면 객체가 추가로 생기게 된다.
* 생성비용이 싼 객체라면 코드 가독성을 고려하여 Object Pool 사용을 자제한다.

## 6.유효기간이 지난 객체의 참조는 폐기하라
* 자바의 메모리 누수 : 객체의 참조가 해제되지 않아서 가비지 컬렉터가 해당 객체를 수집하지 못하여 메모리에 남아있는 현상
	* 자바에서 한 객체가 누수될 경우, 그 객체뿐만 아니라 그 객체가 참조하고 있는 모든 객체가 함께 누수된다.
* 해결방법
	1. 쓸 일 없는 객체의 참조는 무조건 null로 만든다.
	2. 해당 객체의 scope를 최대한 좁게 만들어서, 그 객체가 스스로 scope 밖으로 벗어나게 설계한다.
	3. WeakReference를 이용하거나, 내부적으로 WeakReference를 이용해 구현된 API를 이용한다.

## 7.finalize() 사용을 피하라
* Java의 finalize는 C++의 destructor와 성격이 다르다.
	* C++ Destructor : 객체에 할당된 메모리를 해제하는 역할. 생성자의 정 반대
	* Java finalize : 객체가 GC에 의해 수집되기 직전에 마지막으로 호출될 메서드
* finalize를 사용하면 안되는 이유
	1. 언제 실행될 지 예측할 수 없다. 심지어 실행되지 않을 수도 있다.
	2. 프로그램의 성능이 심각하게 저하된다.
* finalize를 사용하기 적합한 곳
	1. 명시적 종료 메서드(close() 등) 호출을 잊을 경우를 대비할 경우
	2. 자바의 native 메서드를 통해 기능 수행을 위임하는 네이티브 객체의 경우
* finalize를 사용해야할 상황이라면 상위 객체의 finalize를 반드시 호출해준다.