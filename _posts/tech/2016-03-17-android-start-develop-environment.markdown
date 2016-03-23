---
layout: post
title:  "안드로이드 공식문서 번역 1_개발환경 구축 및 프로젝트 생성"
date:   2016-03-17 22:52:21
category: tech
comments: true
---
# 안드로이드 공식문서 번역 1 - 개발환경 구축
이 글은 [안드로이드 개발자 사이트](http://developer.android.com "안드로이드 개발자 사이트")에 있는 내용을 번역한 것이다.

## 첫번째 앱 개발하기
이 페이지에서 여러분은 첫번째 안드로이드 앱 프로젝트를 생성하고 실행하는 방법을 배우게 됩니다.
그리고 안드로이드 앱 디자인의 기초, UI제작과 처리방법도 배울 수 있습니다.
 
### 개발환경 구축하기
1. 안드로이드 스튜디오를 받아서 컴퓨터에 설치합니다. [안드로이드 스튜디오 설치하기](http://developer.android.com/sdk/index.html "안드로이드 스튜디오 설치")
![안드로이드 스튜디오 다운로드]({{ site.url }}/images/download.png)
2. 설치한 안드로이드 스튜디오를 실행한 후에, 도구모음 아이콘에서 SDK Manager를 실행합니다. 그리고 SDK Manager에서 최신 SDK 도구와 플랫폼을 다운로드 받습니다.
`Note : 모든 개발문서는 안드로이드 스튜디오로 작성되어 있지만, 간혹 어느 부분은 필요에 따라 SDK 도구를 커맨드라인(윈도우즈의 cmd)에서 실행합니다.`
![SDK 매니저 실행]({{ site.url }}/images/sdk_download.png)
![SDK 다운로드]({{ site.url }}/images/sdk.pn

### 안드로이드 프로젝트 생성하기
이번 강의에서는 안드로이드 스튜디오 혹은 커맨드 화면에서 SDK 도구를 이용하여 새로운 안드로이드 프로젝트를 생성하는 방법을 배우게 됩니다.
안드로이드 프로젝트는 여러분이 만들 안드로이드 앱을 구성하는 모든 소스코드들이 포함됩니다.

​`Note : 이 이후의 모든 과정들을 진행하기 위해서는, SDK 도구와 안드로이드 스튜디오가 설치되어 있어야 합니다.
(안드로이드 스튜디오는 선택사항이지만 여러가지 기능이 있어서 개발시간을 엄청 단축시켜주기 때문에 사용하는걸 권장합니다.)`

#### 안드로이드 스튜디오로 프로젝트 생성하기​
1. 안드로이드 스튜디오에서 새로운 프로젝트를 생성합니다.
	* 안드로이드 프로젝트를 처음 실행해서 이전에 진행하던 프로젝트가 없으면 Welcome 스크린이 뜨는데, New Project를 클릭합니다.
	* 이전에 진행하던 프로젝트가 있으면, File 메뉴에서 New Project를 선택합니다.
2. 아래 그림과 같이 Configure your new project 화면이 나오면 빈 칸의 내용을 다 채우고 Next를 클릭합니다.
![프로젝트 마법사]({{ site.url }}/images/configure.png)
	* Application Name : 사용자에게 보여줄 앱의 이름입니다. 이 강의에서는 "My First App" 이라고 적었습니다.
	* Company Domain : package 이름에 추가될 식별자로 사용됩니다. 안드로이드 스튜디오는 이 식별자를 가지고 프로젝트를 구분합니다.
	* Package Name : 프로젝트들을 완전히 식별하는 이름입니다. 안드로이드는 이 이름으로 앱을 구분하기 때문에 반드시 유니크 해야 합니다. 작명규칙은 기본적으로 Java의 관례대로 하지만 나중에 앱 이름이나 회사 도메인에 무관하게 변경할 수 있습니다.
	* Project Location : 생성한 프로젝트가 위치할 경로입니다.
3. 어떤 기기에서 돌아가는 앱을 만들 것인지 선택합니다. 이 프로젝트에서는 Phone & Tablet을 선택합니다.
4. Minimum SDK는 이 앱이 지원할 최소 버전을 나타냅니다. 낮은 안드로이드 버전을 선택하면 마켓에서 더 많은 기기들에 노출되지만 그만큼 구형 버전에 없는 기능들에 대해 고려할 게 많아집니다. 반대로 높은 안드로이드 버전을  택하면 고려할 사항은 줄어들지만 하위 버전의 기기에서는 마켓에 노출되지 않습니다. (이 프로젝트에서는 API 8: Android 2.2(Froyo)를 선택했습니다.)
5. TV, Wear, Glass 등이 선택되어 있다면 선택을 해제하고 Next를 클릭합니다.
6. Add an Activity to Mobile 화면에서 Blank Activity를 선택하고 Next를 눌러줍니다.
![디바이스 설정]({{ site.url }}/images/device.png)
7. Customize the Activity 화면에서 Activity Name을 MyActivity, Layout Name을 activity_my, Title을 MyActivity, Menu Resource Name을 menu_my로 바꿔줍니다.(Activity Name을 바꾸면 나머지는 자동적으로 바뀌게 됩니다.)
![디바이스 설정]({{ site.url }}/images/activity.png)
8. Finish 버튼을 클릭하면 새 프로젝트가 생성됩니다.

이 과정까지 마치고 나면 프로그래밍에 익숙한 "Hello World" 앱이 생성됩니다. 이 프로젝트에는 많은 폴더와 파일들이 있는데 이 중에서 중요한 몇가지 파일만 살펴보겠습니다.
![디바이스 설정]({{ site.url }}/images/project.png)


* `app/src/main/res/layout/activity_my.xml` : ​이 파일은 안드로이드 스튜디오로 프로젝트를 생성할 때 추가되는 xml 파일로 Activity의 화면 배치를 담당합니다. 안드로이드 스튜디오에서는 이 xml 파일을 미리 화면에 보여주고 있으며 처음 생성된 xml파일에서는 "Hello World" 라는 글자를 보여줍니다.
* `app/src/main/java/com.mycompany.myfirstapp/MyActivity.java` : ​​이 파일은 프로젝트를 처음 생성할 때 추가되는 java 파일이며 Activity라는 클래스를 상속받은 MyActivity 클래스 코드가 저장되어 있습니다. 앱을 Build 하면 이 클래스가 구동되고, layout xml파일을 로드하여 화면에 보여줍니다.
* `app/src/main/AndroidManifest.xml` : ​​이 파일은 안드로이드 앱의 모든 특성을 기술하고 있으며, 각 Component를 제어하고 Component를 추가해주는 파일입니다.
* `app/build.gradle` : ​​안드로이드 스튜디오는 앱을 컴파일&빌드 하기 위해 Gradle이라는 시스템을 사용합니다. build.gradle이라는 파일이 프로젝트의 각 모듈에도 들어있고, 전체 프로젝트에도 존재합니다. 일반적으로, 개발자는 각 모듈에 들어있는 gradle파일에만 신경쓰면 됩니다. 다음은 gradle 안의 기본적인 빌드 셋팅값들을 설명합니다.
	* compliedSdkVersion은 platform버전과 다르게, 여러분의 앱이 컴파일되는 버전을 의미합니다. 이 값은 기본적으로 SDK Manager에서 이용가능한 최신 버전으로 설정됩니다.
	* applicationId​은 여러분의 앱을 구분해줄 식별자 package name 입니다.
	* minSdkVersion은 이 앱이 구동할 최소 버전입니다.
	* targetSdkVersion은​ 이 앱이 구동할 수 있는 최대 버전입니다. 일반적으로 새로운 API 버전이 출시되면 이 값을 최신 버전으로 설정하여 테스트 해 볼 수 있습니다.

* `/res` : 하위 폴더에는 앱에서 사용할 Resource들이 포함되어 있습니다.
	* `drawable<density>/` : ​이 폴더는 다양한 화면 스크린 density에 대응한 drawable 객체(Bitmap 같은)를 저장합니다.
	* `layout/` : ​이 폴더는 위의 activity_my.xml과 같은 앱의 UI와 관련된 xml파일들이 저장됩니다.
	* `menu/` : ​이 폴더는 앱의 메뉴 아이템들이 저장됩니다.
	* `values/` : ​이 폴더는 Resource들의 collection들이 포함된 xml파일이 저장됩니다.(color, string 등등)