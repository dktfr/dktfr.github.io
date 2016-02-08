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