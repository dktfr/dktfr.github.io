---
layout: post
title:  "안드로이드 공식문서 번역 2_테스트 시작하기"
date:   2016-03-24 22:52:21
category: tech
comments: true
---
# 안드로이드 공식문서 번역 2 - 테스트 시작하
이 글은 [안드로이드 테스트 시작하기](http://developer.android.com/training/testing/start/index.html)에 있는 내용을 번역한 것이다.

## 테스트 시작하기
개발자는 안드로이드 앱 개발의 일부로써 테스트를 작성하고 수행해야 한다. 잘 작성된 테스트는 개발자가 버그를 일찍 잡아낼 수 있도록 하고, 코드에 자신감을 가질 수 있도록 만들어 준다.
앱에서 특정 동작을 검증하고, 각기 다른 디바이스에서 일관성을 유지하려면 개발자는 테스트 케이스를 작성해야 한다.
이 강의에서는 JUnit4 프레임워크와 구글로부터 제공되는 Test API와 Tool을 이용하여 테스트를 작성하는 방법을 알아볼 것이다.

### 테스트 환경 구축하기
테스트를 작성하고 수행하기 전에, 테스트 개발 환경을 구축해야 한다. 안드로이드 스튜디오는 안드로이드 앱의 테스트 케이스를 작성하기 위한 GUI 통합 개발환경을 제공한다.
강의를 계속 진행하기 위해 다음의 2가지 개발툴을 다운로드 받도록 한다.
* 안드로이드 스튜디오
* Android Support Library(SDK Manager 내에서)

테스트하고자 하는 목적에 맞게, 테스트 코드의 위치와 프로젝트 의존성을 변경해주어야 한다.(다음 섹션에서 설명할 것이다)

#### 로컬 Unit 테스트를 위한 프로젝트 설정 변경
로컬 Unit 테스트는 안드로이드 프레임워크나 디바이스에 접근할 필요 없이 로컬 머신에서 수행하는 테스트이다.
로컬 Unit 테스트를 위해서는 안드로이드 프로젝트에서 `src/test/java` 디렉터리에 로컬 Unit 테스트 소스코드를 저장해야 한다.
프로덕션 코드처럼, Build 타입이나 기호에 따라 로컬 Unit 테스트를 생성할 수 있다. 생성된 각 Unit 테스는 프로덕션 코드의 소스트리 경로와 연관되도록 해야 한다.

|프로덕션 코드 경로|로컬 Unit 테스트 경로|
|-|-|
|src/main/java.Foo.java|src/test/java/FooTest.java|
|src/debug/java/Foo.java|src/testDebug/java/FooTest.java|
|src/myFlavor/java/Foo.java|src/testMyFlavor/java/FooTest.java|

JUnit4 프레임워크에서 제공되는 표준 API를 이용하기 위해 프로젝트의 테스팅 의존성을 설정해줘야 한다. 수행하려는 테스트가 안드로이드 의존성과 상호작용할 필요가 있다면 Mockito 라이프러리를 추천한다.

앱 모듈의 `build.gradle` 파일을 열고, 프로젝트의 의존성을 다음과 같이 추가한다.

```
dependencies {
	// Required -- JUnit 4 framework
	testCompile 'junit:junit:4.12'
	// Optional -- Mockito framwork
	testCompile 'org.mockito:mockito-core:1.10.19'
}
```

#### Instrumented 테스트를 위한 프로젝트 설정 변경
Instrumented 테스트는 안드로이드 에뮬레이터나 실 기기에서 수행하는 테스트이다. 이 테스트는 테스트 대상 앱의 `Context` 같은 Instrumentaton 정보에 접근해야 한다.
Instrumented 테스트는 Unit 테스트, UI or 앱 컴포넌트의 통합테스틀 이용될 수 있다.

Instrumented 테스트를 위해서는 안드로이드 프로젝트에서 `src/androidTest/java` 디렉터리에 Instrumented 테스트 소스코드를 저장해야 한다.
Instrumented 테스트 코드를 빠르게 빌드하고 수행시켜줄 API를 포함한 `Android Testing Support Library`를 다운로드 받는다.
이 라이브러리는 JUnit4 test runner(`AndroidJUnitRunner`)와 기능 UI 테스트 API(Espresso, UI Automator)를 포함하고 있다.

테스트 라이브러리의 test runner와 API를 이용하기 위해 프로젝트의 안드로이드 테스팅 의존성을 설정해줘야 한다. 다양한 matcher API를 지원하여 유연한 Assertion 을 생성할 수 있는 `Hamcrest` 라이브러리를 추천한다.

앱 모듈의 `build.gradle` 파일을 열고, 프로젝트의 의존성을 다음과 같이 추가한다.

```
dependencies {
	androidTestCompile 'com.android.support:support-annotations:23.0.1'
	androidTestCompile 'com.android.support:test:runner:0.4.1'
	androidTestCompile 'com.android.support.test:rules:0.4.1'
	// Optional -- Hamcrest library
	androidTestCompile 'org.hamcrest:hamcrest-library:1.3'
	// Optional -- UI testing with Espresso
	androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.1'
	// Optional -- UI testing with UI Automator
	androidTestCompile 'com.android.support.test.uiautomator:uiautomator-v18:2.1.1'
}
```

### 테스트 빌드 & 수행
테스트는 안드로이드 앱을 실행하는 것과 비슷한 방식으로 수행할 수 있다.

#### 로컬 Unit 테스트 수행
안드로이드 Gradle 플러그인은 기본경로(`src/test/java`)에 있는 테스트 코드를 컴파일하고, 테스트 앱을 빌드하고, 기본 test runner 클래스를 이용해서 빌드된 테스트 앱을 실행한다.
안드로이드 스튜디오에서 로컬 Unit 테스트를 수행하려면
1. **Project** 윈도우에서 프로젝트를 우클릭하고 동기화한다.
2. **Build Varients** 윈도우를 열고 test artifact를 Unit Tests로 수정한다.
3. **Project** 윈도우에서 테스트 클래스 or 디렉터리를 우클릭하여 Run Tests를 수행한다.

안드로이드 스튜디오는 **Run** 윈도우에서 Unit 테스트 수행결과를 보여준다.

#### Instrumented 테스트 수행
안드로이드 Gradle 플러그인은 기본경로(`src/androidTest/java`)에 있는 테스트 코드를 컴파일하고, 테스트 APK와 프로덕션 APK를 빌드하고, 두 개의 APK를 현재 연결된 실 기기나 에뮬레이터에 설치하여 테스트를 실행한다.
프로젝트의 기본 test instrumentation runner로 `AndroidJUnitRunner`를 명시해 주어야 한다. `build/gradle` 파일에 다음과 같이 추가해 준다.

```
android {
	defaultConfig {
		testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
	}
}
```

안드로이드 스튜디오에서 Instrumented 테스트를 수행하려면
1. **Build Variants** 윈도우를 열고 test artifact를 Android Instrumentation Tests로 수정한다.
2. **Project** 윈도우에서 테스트 클래스 or 디렉터리를 우클릭하여 Run Tests를 수행한다.

안드로이드 스튜디오는 **Run** 윈도우에서 Instrumented 테스트 수행결과를 보여준다.