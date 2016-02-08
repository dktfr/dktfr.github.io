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
``` java
public class A {
	public static A getInstance() {
		return new A();
	}
}
```
* 장점
	* 생성자와는 달리 메서드의 이름을 지정해 줄 수 있어서 코드의 가독성이 올라간다.
	* 생성자처럼 매번 새로운 객체를 생성할 필요는 없다. 동일한 객체의 요청이 잦은 경우에는 객체를 생성해두고 캐싱한 후에 필요할 때마다 캐싱된 객체를 반환한다.
	* 생성자와는 달리 return 타입이 자유로워서 유연한 코드의 작성이 가능하다.
	* 형인자 자료형(Parameterized Type) 객체를 만들 때 편하다.
		* 형인자 자료형 갹채 : 제네릭 타입을 선언한 객체
		* ex 생성자를 이용한 생성
``` java
Map<String, Object> map = new HashMap<String, Object>();		// 생성자 호출 시 Type을 반드시 명시해줘야 한다.
```
		* ex Static Factory 메서드를 이용한 생성
``` java
public static <K, V> HashMap<K, V> newInstance() {
	return new HashMap<K, V>();
}

Map<String, Object> map = HashMap.newInstance();
```
		* JDK 1.7부터는 생성자로 객체 생성시 Type을 생략 가능하다. ex) `Map<String, Object> map = new HashMap<>();`
* 단점
	* 모든 생성자를 private으로 감추고 Static Factory 메서드만 지원할 경우 서브클래스를 생성할 수 없다.
	* Static Factory 메서드가 다른 일반 Static 메서드와 확연히 구분되지 않는다.
