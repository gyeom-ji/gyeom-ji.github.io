---
title: "iOS 면접 준비 - Swift"
author: gyeomji
date: 2024-10-10 10:00:00 +0900
categories: [Interview]
tags: [Swift]
pin: false
math: true
mermaid: true
---

## ✏️ 싱글톤 패턴(Singleton Pattern)이란 무엇이며, 어떤 경우에 사용하나요?

---

- 싱글톤 패턴이란 <span style="color:#9fb584">**오직 하나의 객체(인스턴스)만 생성되도록 보장**</span>하는 디자인 패턴이다.
  - 즉, 생성자를 통해서 여러 번 호출이 되더라도 인스턴스를 새로 생성하지 않고 최초 호출 시에 만들어두었던 인스턴스를 재활용하는 패턴이다.
- 주로 프로그램 내에서 <span style="color:#9fb584">**하나로 공유 해야하는 객체가 존재할 때**</span> 해당 객체를 싱글톤으로 구현하여 모든 유저 또는 프로그램들이 해당 객체를 공유하며 사용하도록 한다.
  - 프로그램 내에서 하나의 객체만 존재해야 한다.
  - 프로그램 내 여러 부분에서 해당 객체를 공유하여 사용해야한다.
- DBCP(DataBase Connection Pool)처럼 공통된 객체를 여러개 생성해서 사용해야하는 상황에서 많이 사용한다.
  - 쓰레드풀, 캐시, 대화상자, 사용자 설정, 레지스트리 설정, 로그 기록 객체 등
<br/>

### 장점

- 객체가 하나만 존재하기 때문에 <span style="color:#9fb584">**메모리 낭비를 줄일 수 있다.**</span>
- 생성된 인스턴스를 사용할 때 <span style="color:#9fb584">**이미 생성된 인스턴스를 활용해 속도 측면에 이점**</span>이 있다.
- 어디서든 같은 인스턴스를 사용할 수 있기 때문에 <span style="color:#9fb584">**데이터 일관성이 유지**</span>된다.
- <span style="color:#9fb584">**전역에서 접근하기 쉽다.**</span>

<br/>

### 단점

- 전역 접근이 쉽기 때문에 객체가 여러 곳에서 사용될 수 있다. ➡️ <span style="color:#9fb584">**코드가 복잡해질 수 있다.**</span>
- 싱글톤 패턴을 사용하는 경우 클래스의 객체를 미리 생성한 뒤에 필요한 경우 정적 메서드를 이용하기 때문에 <span style="color:#9fb584">**클래스 사이에 의존성이 높아지게 된다**</span>는 문제점이 있다.
  - 싱글톤의 인스턴스가 변경 되면 해당 인스턴스를 참조하는 모든 클래스들을 수정해야 하는 문제가 발생한다.
  - 싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유시킬 경우 다른 클래스의 인스턴스들 간에 결합도가 높아져 "개방-폐쇄 원칙" 을 위배하게 된다.
    - 객체 지향 설계 원칙이 어긋난다.
- 싱글톤 패턴은 기본 생성자를 private로 만들었기 때문에 <span style="color:#9fb584">**상속을 통한 자식 클래스를 만들 수 없다.**</span>
- <span style="color:#9fb584">**테스트하기 어렵다.**</span>
  - 싱글톤 인스턴스는 자원을 공유하고 있기 때문에 테스트가 격리된 환경에서 수행되려면 매번 인스턴스의 상태를 초기화시켜주어야 한다.
  - 그렇지 않으면 어플리케이션 전역에서 상태를 공유하기 때문에 테스트가 온전하게 수행되지 못한다.
- 멀티스레드 환경에서는 <span style="color:#9fb584">**동기화 문제가 생길 수 있다.**</span>
  - 멀티스레드 환경에서 동기화 문제 : 여러 스레드가 동시에 싱글톤 객체를 생성하려고 할 때 발생하는 문제이다. 여러 스레드가 동시에 싱글톤을 생성하면 경우에 따라 인스턴스가 2, 3개 생성될 수 있다.

<br/>

## ✏️ Swift에서 싱글톤 패턴을 구현할 때 멀티스레드에 대해서 어떻게 고려해야 하나요?

---

- Singleton을 생성하는 것은 Multi Threading 환경에서 Thread-Safe하지 않다.
  > Thread-Safe : 멀티스레드 환경에서 어떤 함수나 객체가 여러 스레드에 의해 동시에 접근되더라도 문제없이 각 스레드에서 원하는 결과가 도출된다는 것을 의미한다. 
  - Objective-C에서는 앱이 실행된 후 단 한번만 실행되도록 dispatch_once를 사용하여 싱글톤을 만들어야 한다.
- Swift에서는 `static let`을 사용하면 Tread-Safe하게 싱글톤을 구현할 수 있다.
- `static`을 사용해 <span style="color:#9fb584">**타입 프로퍼티**</span>로 인스턴스를 생성할 경우 사용 시점에 초기화(lazy)된다.
- Singleton Instance가 최초 생성되기 전까진 메모리에 올라가지 않고, Dispatch_once도 자동 적용된다.
- 따라서 별도의 작업 없이도 인스턴스가 여러 개 생성되지 않는 Tread-Safe한 방법이 된다.

<br/>


## ✏️ 데이터베이스의 종류와 iOS에서 주로 사용되는 데이터베이스에 대해 설명해주세요.

---

### 데이터베이스의 종류

- 데이터베이스는 크게 `관계형 데이터베이스`와 `비관계형 데이터베이스`로 나눌 수 있다.
- 관계형 데이터베이스(RDBMS) 
  - 데이터가 행과 열로 구성된 2차원 테이블 형태로 저장된다.
  - SQL을 사용하여 데이터를 조작한다.
- 비관계형 데이터베이스(NoSQL)
  - 키-값으로 데이터를 저장한다.
  - JSON 등의 문서 형태로 데이터를 저장한다.

<br/>

### iOS에서 주로 사용되는 데이터베이스

- `SQLite`, `Core Data`, `Realm`

#### SQLite

- 세계에서 가장 많이 사용되는 관계형 데이터 베이스 시스템으로, 데이터를 테이블과 행으로 저장하며, SQL 쿼리를 통해 데이터에 접근한다.
- 안드로이드, iOS간 공유가 가능하다.
- 특정한 외형, 서버가 필요없는 SQL 데이터 베이스 엔진을 구현한다.
- 서버로부터 독립적이며 설정이 간편하다.
- 여러 프로세스와 스레드로부터 접근이 안전하다.
- 데이터를 직접 SQL 쿼리로 관리하고 싶거나 Core Data의 복잡한 설정을 피하고자 할 때 사용한다.
- 앱 내에서 작은 데이터베이스를 관리할 때 유용하다.
  - 복잡한 객체 관계 없이 단순히 데이터를 저장하고 쿼리할 때 유용하다.
- SQL 기반 쿼리: SQL 쿼리를 사용하여 데이터를 저장하고 조회하며, 익숙한 SQL 명령어를 활용할 수 있다.
- 간단하고 가벼움: SQLite는 앱 내부에 데이터베이스 파일을 생성하여 데이터를 저장하므로, 가벼운 데이터베이스를 사용하기에 적합하다.
- 제어가 용이함: SQL을 통해 직접 데이터를 관리하므로, 보다 세밀한 데이터베이스 제어가 필요할 때 유용하다.

<br/>

#### Core Data

- iOS 플랫폼 단에서 지원하는 객체 관계 데이터베이스 프레임워크이다.
  - 단순한 데이터베이스라기보다는 데이터를 객체로 관리하고, 다양한 관계를 설정하며, 애플리케이션 전체에서 객체의 상태를 쉽게 관리하도록 돕는 프레임워크이다.
  - 앱 내부의 상태나 관계가 복잡한 데이터를 관리할 때 유용하다.
    - 소셜 미디어 앱에서 사용자 정보, 게시물, 댓글 등의 관계를 관리하는 경우에 적합하다.
- SQLite를 기반으로 하여 복잡한 데이터 구조를 쉽게 관리할 수 있다.
- 객체 형식으로 저장하고 관리하기 때문에 사용하기 용이하다.
- 속도가 빠르지만 Thread-safe하지 않다.
- 위젯 등을 개발할 때 데이터 연동이 편리하다.
- SQLite보다 많은 메모리를 사용하지만, 더 빠르다.
- 데이터 모델링: Core Data는 데이터를 객체(엔티티)로 모델링하여, 관계형 데이터베이스뿐 아니라 객체 간의 관계를 쉽게 설정할 수 있다.
- 자동 관리: 데이터 변경 시 상태를 자동으로 관리하고, 이를 메모리에서 쉽게 다룰 수 있도록 도와준다.
- 성능 최적화: 불러올 데이터만 로딩하는 지연 로딩(Lazy Loading), 메모리에서 효율적으로 객체를 관리하는 캐싱 등의 최적화 기능을 제공한다.

<br/>

#### Realm

- 오픈 소스 라이브러리, 모바일에 최적화된 데이터베이스 라이브러리이다.
  - iOS, Android 간 데이터베이스 공유가 가능하다.
- ORM이 아니기 때문에 백그라운드에서 적용되는 SQL 쿼리문을 실행하지 않는다.
  - ORM은 데이터를 접근할 때도 쿼리문 작업이 필요하다.
  - 이는 CPU 사이클과 디스크 시간을 소비하여 속도가 느려질 수 있다.
- 객체 중심의 데이터베이스이다.
  - 객체를 직접 디스크에 유지한다.
  - 복잡한 엔티티에 대한 매핑을 처리하지 않아도 되므로 메모리 상의 객체를 디스크로 빠르게 가져올 수 있다.
- 메인 스레드에서 데이터 읽기/쓰기 작업이 가능하다.
  - 메인 스레드에서 작업하다가 갑자기 앱이 터지는 경우를 줄여준다.
- 다른 DB들보다 속도가 빠르다.


> ORM (Object Relational Mapping) : 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간에 데이터를 변환하는 프로그래밍 기술이다. 즉, 객체와 관계형 데이터베이스의 데이터를 매핑하는 것을 의미한다. 객체(Object)와 DB의 테이블을 자동으로 연결(Mapping)시켜 RDB 테이블을 객체 지향적으로 사용하게 해준다. ORM을 사용하면 SQL 쿼리 없이 데이터를 데이터베이스에 저장하고 관리할 수 있다. 코드의 가독성을 높이고, 유지 보수를 용이하게 하며, 데이터베이스와의 결합도를 낮추는 등 여러 장점을 제공한다.


<br/>

## ✏️ Swift에서 옵셔널(Optional)이란 무엇이며, 언제 사용해야 하나요?

---

### Optional

- 값이 있을 수도 있고 없을 수도 있는 변수나 상수(=nil값)를 다룰 때 사용한다.
- 값이 null인 변수에 접근하면 프로그램이 종료된다.
- 옵셔널을 사용하면 값이 없는 변수에 접근해도 프로그램이 종료되지 않는다.
  - nil에 대한 컴파일 에러를 통해 개발자는 nil에 대해 명확한 예외처리가 강제되며, 런타임에 nil로 인한 문제를 컴파일 단계에서 예방할 수 있다.
- nil 값이 들어갈 수 있는 변수 타입 뒤에 `?`를 붙인다.
- 옵셔널 값은 일반 타입과 결합하거나 연산할 수 없기 때문에 옵셔널 값을 사용하려면 옵셔널 바인딩을 해야 한다.

``` swift
var num: Int? // nil
var optionalNum: Int? = 1
print(optionalNum) // Optional(1)

```

<br/>

## ✏️ 옵셔널 바인딩과 강제 언래핑의 차이점은 무엇인가요?

---

#### 강제 언래핑 `!` 

- 옵셔널이 nil일 경우 런타임 오류로 이어질 수 있다.
- 매우 위험하기 때문에 <span style="color:#9fb584">**반드시 옵셔널 값이 있다고 보장할 수 있는 경우**</span>에만 사용해야 한다.

``` swift
// 강제 해제
var num: Int?
print(num!) // nil 에러

num = 1
print(num!) // 1
```

<br/>

#### 옵셔널 바인딩

- 옵셔널에 <span style="color:#9fb584">**값이 있는지 확인하고 값이 있다면 저장된 값을 안전하게 추출하기 위해 새로운 상수나 변수에 할당하여 사용하는 방법**</span>이다.
- if 옵셔널 바인딩 : if 블록 내에서만 사용 가능 (nil일 때 아닐 때 둘 다 취급)
- guard 옵셔널 바인딩 : guard문 다음 함수 전체에서 사용 가능 (nil일 때만 취급)

``` swift
// 비강제 해제
if let result = num {
    print(result) // nil 아닐 때
} else {
    print("nil") // nil 일 때
}


func test() {
    let num : Int? = 1
    guard let result = num else {return} // nil 일 때
    print(result)
}

test()
```

<br/>

## ✏️ 옵셔널 체이닝의 동작 원리는 무엇이며, 어떻게 사용하나요?

---

#### 옵셔널 체이닝

- 옵셔널을 연쇄적으로 사용하는 것으로 `dot(.)`을 통해 프로퍼티나 매서드에 옵셔널이 하나라도 붙어 있으면 옵셔널 체이닝이라 한다.
  - gyeomji.room?.number ➡️ `.` 기준 옵셔널이 껴있는 구조이다.
  - `?` 앞 대상이 nil이면 `?.` 뒤의 number는 nil을 반환한다.
- 여러 중첩된 옵셔널 값이 있을 때, 각 값이 nil인지 확인하며 안전하게 호출하거나 접근하는 방식이다.
- 옵셔널이 nil이면 체이닝 전체가 nil을 반환하며, 에러 없이 종료된다.
  - 옵셔널 체이닝은 강제 언래핑의 위험 없이 옵셔널 값을 처리한다.

<br/>

## ✏️ 암시적 언래핑 옵셔널(Implicitly Unwrapped Optional)은 어떤 경우에 사용해야 하나요?

---

### 암시적 언래핑 옵셔널

- 언래핑하지 않고도 사용할 수 있는 옵셔널이다.
- 옵셔널 바인딩이나 nil 병합 연산을 통해 값이 있을 때와 없을 때 처리를 분기해서 사용할 수 있다.

``` swift
var age: Int! = nil // 암시적으로 언래핑한 옵셔널
var name: String? = nil // 일반 옵셔널

// 옵셔널 바인딩과 암시적으로 언래핑한 옵셔널을 사용한 옵셔널 처리
func test() {
    var age: Int! = nil
    guard let age = age else { // nil
        print("nil")
        return
    }
    print("age : \(age)")
}

// nil 병합 연산과 암시적으로 언래핑한 옵셔널을 사용한 옵셔널 처리
print(age ?? "nil")
var newAge = (age ?? 0) + 2
print(newAge) // 2

```

- 일반 옵셔널과 암시적으로 언래핑한 옵셔널은 값에 접근하여 사용하는 방식에서 차이가 있다.
- 암시적으로 언래핑한 옵셔널은 값에 접근할 때마다 옵셔널 값을 강제 언래핑을 하는 것과 같다.
  - 때문에 옵셔널 바인딩이나 nil 병합 연산을 하지 않아도 옵셔널이 벗겨진 상태로 사용이 가능하다.
- nil이 담긴 옵셔널을 강제 언래핑 할 경우 프로그램이 비정상적으로 종료된다.
  - nil이 담겨있는 암시적으로 언래핑한 옵셔널을 사용하면, nil이 담긴 옵셔널을 강제 언래핑하는 것과 마찬가지기 때문에 runtime에 프로그램이 비정상적으로 종료된다.

``` swift
// case 1
var age: Int! = 1 
print(age + 10) // 11

// case 1 일반 옵셔널 선언 + 강제 언래핑
var age: Int? = 1
print(age! + 10) // 11

// case 2 age가 nil일 경우
var age: Int! = nil // error
print(age + 10) 

// case 2 일반 옵셔널 선언 + 강제 언래핑
var age: Int? = nil // error
print(age! + 10)
```

<br/>

#### 암시적 언래핑 옵셔널 특징

- 일반 옵셔널과 동일하게 값의 부재를 나타낼 수 있다. (nil 혹은 값 할당이 가능하다)
- 일반 옵셔널과 동일하게 옵셔널 언래핑 처리 과정을 거친 후에 값을 사용할 수도 있다.
- 일반 옵셔널과는 다르게 옵셔널 언래핑 처리 과정을 거치지 않고도 값에 접근이 가능하다.
- 만약 nil이 들어있는 암시적으로 언래핑한 옵셔널에 접근하면 runtime에 프로그램이 비정상적으로 종료된다.


<br/>

#### 암시적 언래핑 옵셔널 사용

- 옵셔널에 접근할 때마다 옵셔널 언래핑을 하면 코드가 길어지고 가독성이 떨어질 수 있다.
- 옵셔널에 값이 무조건 들어있다는 것을 확신할 때 암시적 언래핑 옵셔널을 사용하여 옵셔널 언래핑 처리 과정을 생략해 가독성과 편리성을 높일 수 있다.
- 옵셔널에 예시 : IBOutlet
  - 스토리보드에서 뷰객체가 생성되기 전에는 IBOutlet은 nil이 들어있다. 
  - 하지만 뷰 객체가 생성되고 난 뒤로부터는 해당 뷰컨트롤러에 항상 값이 들어있을 것이기 때문에 암시적으로 언래핑한 옵셔널으로 선언되어 있다.

<br/>

#### nil 병합 연산자 `??`

- 병합 연산을 수행하여 수행하여 인스턴스의 Optional wrapping된 값 또는 기본값을 반환한다.   
- 언래핑하고자 하는 옵셔널 값을 `??` 연산자 좌측에 놓고, 우측에 대신할 값을 적으면 된다.
- 옵셔널에 값이 있으면, 옵셔널이 언래핑 된 채로 사용할 수 있고, 만약 값이 없고 nil이라면 ?? 연산자 우측에 적은 대체값을 사용한다.

``` swift
var age: Int? = nil
print(age ?? 0) // 0
```

<br/>

## ✏️ iOS 앱의 생명주기(App Life Cycle)에 대해 설명해주세요.

---

### App Life Cycle

- 앱 생명 주기는 앱의 실행/종료 및 앱이 Foreground/Background 상태에 있을 때, 시스템이 발생시키는 event에 의해 앱 상태가 전환되는 일련의 과정이다.
- App의 현재 상태에 따라 할 수 있는 것과 할 수 없는 것이 결정된다.
- Foreground App은 사용자의 주의를 끌기 때문에 CPU를 포함한 시스템 리소스보다 우선순위가 높다.
- 반대로 Background App은 offscreen이기 때문에 가능한 적은 작업을 수행해야하고, 되도록 아무것도 수행하지 않아야 한다.
- 또한 앱의 상태가 변경될 때 마다 그에 맞는 동작을 조정해야 한다.
- iOS 12와 이전 버전은 전체 Life Cycle을 UIApplicationDelegate 객체에서 관리했다. 
- 따라서 해당 protocol을 채택하는 `AppDelegate`에서 대응되는 method를 구현한다.
- iOS 13과 이후 버전은 Scene 개념이 도입되면서 하나의 앱이 여러 UI Life Cycle을 가질 수 있게 되었다. 
- 각각의 Scene에서 관리하는 UI Life Cycle은 UISceneDelegate에서 관리하고, 해당 protocol을 채택하고 있는 `SceneDelegate`가 각각의 Scene에서 관리하는 UI Life Cycle에 대응되는 method들을 구현힌다.
- 정리하면, <span style="color:#9fb584">**앱 실행 및 종료와 관련된 Process Life Cycle은 AppDelegate**</span>에서, <span style="color:#9fb584">**앱이 Foreground와 Background 상태에 있을 때 상태 전환과 관련된 UI Life Cycle은 SceneDelegate**</span>에서 관리한다. 
- SceneDelegate를 사용하지 않도록 설정하면 예전처럼 AppDelegate가 모든 Life Cycle에 대한 관리 책임을 갖지만, 그렇지 않으면 UI Life Cycle은 SceneDelegate를 통해 관리해야 힌다.

<br/>

#### AppDelegate

- 앱시작과 같은 애플리케이션 수준의 이벤트 처리를 담당한다.
- 앱의 중앙 데이터 구조 초기화하기
- 앱의 장면 구성하기
- 메모리 부족 경고, 다운로드 완료 알림 등과 같은 앱 외부에서 발생하는 알림에 응답하기
- 앱 자체를 대상으로 하고 앱의 장면, 뷰 또는 뷰 컨트롤러에 국한되지 않는 이벤트에 응답하기
- Apple 푸시 알림 서비스와 같은 출시 시 필요한 서비스에 등록하기

<br/>

#### SceneDelegate

- 기존 AppDelegate에서 총괄하던 기능이 분리된 것이다.
  - iPad-OS 에 도입된 새로운 다중 창 지원기능이 적용되었기 때문에 기능이 분리되었다.
- UISceneSession의 장면 생성, 파괴 및 상태 복원과 같은 장면의 생명주기 이벤트를 담당한다.
- 앱이 Foreground와 Background 상태에 있을 때 상태 전환

<br/>

## ✏️ 앱의 각 상태(Not Running, Inactive, Active, Background, Suspended)에서 가능한 작업은 무엇인가요?

---

### App State

#### 1. Not Running

- 앱이 아직 실행되지 않았거나, 완전히 종료되어 동작하지 않는 상태이다.
- 이 상태에서는 아무런 코드도 실행되지 않는다.

#### 2. Foreground - Inactive

- 앱이 실행중이지만 사용자로부터 event를 받을 수 없는 상태이다.
- multitasking window로 진입하거나 app 실행중 전화, 알림 등에 의해 app을 사용할 수 없게 되는 경우 이 상태로 진입한다.

#### 3. Foreground - Active

- 앱이 실제 실행 중이고 사용자 event를 받아 상호작용할 수 있는 상태이다.
- Inactive를 거쳐 Active 상태가 된다.

> 앱이 Active -> InActive로 가는 예시<br/>
> 배터리 부족 알림이 뜰 경우<br/>
> 외부 알림이 올 경우

<br/>

#### 4. Backgound - Running

- 홈화면으로 나가거나 다른 앱으로 전환되어 현재 앱이 실질적인 동작을 하지 않는 상태이다.
- 사용자가 app을 사용하지 않는 동안 서버와 데이터를 동기화하거나 타이머가 실행되어야 하는 등의 작업을 할 수 있다.
- 음악 재생, 위치 추적과 같은 백그라운드 동작을 할 수 있다.

#### 5. Backgound - Suspended

- 앱을 다시 실행 했을 때 최근 작업을 빠르게 로드하기 위해 메모리에 관련 데이터만 저장되어 있는 상태이다.
- Backgound 상태에 진입했을 때 다른 작업을 하지 않으면 Suspended 상태로 진입한다. (보통 2~3초 이내)
- Suspended 상태의 앱은 iOS 메모리가 부족해지면 가장 먼저 메모리에서 해제된다.
- 앱을 종료시킨 적 없어도 앱을 다시 실행하려 하면 처음부터 다시 실행된다.


<br/>

## ✏️ 상태 변화에 따라 호출되는 AppDelegate 또는 SceneDelegate 메서드는 무엇인가요?

---

### 1. application(_:willFinishLaunchingWithOptions:)

- 앱이 실행되기 직전에 호출되며, 앱의 초기화를 수행하는 데 사용된다.


### 2. application(_:didFinishLaunchingWithOptions:)

- 앱이 실행된 직후에 호출된다.
- 앱이 실행되고 앱을 화면에 띄우기 위한 모든 설정이 완료된 뒤, 실제로 화면에 나타나기 직전에 호출된다. 
- UIWindow를 생성하는 등의 작업을 할 수 있다. 
- Storyboard를 사용하면 storyboard에서 entry point를 찾아 내부적으로 UIWindow를 생성한다.


### 3. applicationDidBecomeActive(_:):

- 앱이 Inactive에서 Active 상태로 전환될 때 실행되며, 사용자와 상호작용하기 직전에 수행할 작업을 여기에 추가할 수 있다.


### 4. applicationWillResignActive(_:):

- 앱이 Inactive 상태로 전환되기 전에 실행되며, 백그라운드로 전환되거나 종료될 때 필요한 작업을 수행할 수 있다.


### 5. applicationDidEnterBackground(_:):

- 앱이 백그라운드 상태로 전환된 직후에 호출되며, 백그라운드 동작을 설정하거나 상태를 저장하는 데 사용된다.


### 6. applicationWillEnterForeground(_:):

- 앱이 전면으로 나타나기 직전에 호출된다.
- 앱이 백그라운드에서 전면으로 나타나기 전에 필요한 작업을 수행할 수 있다.


### 7. applicationWillTerminate(_:):

- 앱이 종료될 때 호출된다.
- 시스템에 의해 예기치 못한 상황에서 종료될 때는 호출되지 않는다.
- 앱이 종료되기 전에 필요한 정리 작업을 수행할 수 있다.


### 8. application(_:didReceiveRemoteNotification:fetchCompletionHandler:):

- 원격 알림을 수신할 때 호출된다.
- 이 메서드를 사용하여 원격 알림을 처리하고 필요한 동작을 수행할 수 있다.


<br/>

### Respond to App - Based Life Cycle

#### 1. 앱 실행 : Not Running - Inactive - Active

- 앱이 실행되면 Not Running에서 Inactive를 거쳐 Active로 상태가 전환된다.
- 이 때, AppDelegate는 시스템에 다음 method를 호출하도록 요청한다.
- `application(_:didFinishLaunchingWithOptions:)` :  Not Running ➡️ Inactive
- `applicationDidBecomeActive(_:)` :  Inactive ➡️ Active

<br/>

#### 2️. 앱 실행 후 홈 화면으로 나가면 : Active - Inactive - Background(-Suspended)

- 앱 실행 후 홈 화면으로 나가면 Active에서 Inactive를 거쳐 Background로 상태가 전환된다. 
- 이 때, AppDelegate는 시스템에 다음 method를 호출하도록 요청한다.
- `applicationWillResignActive(_:)` :  Active ➡️ Inactive
- `applicationDidEnterBackground(_:)` :  Inactive ➡️ Background

<br/>

#### 3. Background 상태에 있는 앱을 다시 실행하면 : Background - Inactive - Active 

- Background 상태에 있는 앱을 다시 실행하면 InActive를 거쳐 Active 상태로 전환된다.
- 이 때, AppDelegate는 시스템에 다음 method를 호출하도록 요청한다.
- `applicationWillEnterForeground(_:)` :  Background ➡️ Inactive
- `applicationDidBecomeActive(_:)` :  Inactive ➡️ Active

<br/>

#### 4. 앱 종료 : (Some State) - Background or Suspended - Not Running

- 사용자가 직접 앱을 종료시키는 경우, 앱은 다시 Not Runnig 상태로 돌아간다. 
- 이 때, AppDelegate는 시스템에 다음 method를 호출하도록 요청한다.
- `applicationWillTerminate(_:)`

<br/>

### Respond to Scene - Based Life Cycle

- iOS 13 이후의 앱에서 Scene을 사용한다면 각각의 분리된 Scene의 UI Life Cycle event들은 SceneDelegate로 전달된다.
- UI Life Cycle의 책임이 Scene으로 옮겨가면서, AppDelegate의 method 이름이 application에서 scene으로 바뀌기만 했을 뿐 맡은 역할과 호출되는 시점은 동일하다.


#### 1. 앱 실행 

- 앱이 실행되면 앱을 화면에 보여주기 위한 모든 설정이 끝나고 다음 method가 호출된다.
- `application(_:didFinishLaunchingWithOptions:)` 

<br/>

#### 2️. Scene 연결

- 앱이 실행되면 UIKit에 Scene을 연결해야 한다.
- Scene이 연결되고 화면에 나타나기까지 과정에서 다음 순서로 method들이 호출된다. 
- `application(_:configurationForConnecting:options:)`
  - 새로운 Scene을 만들고 UIKit과 연결하기 위한 configuration을 지정한다.
  - 여기서 Configuration은 Scene의 delegation 객체를 지정하는 등 Scene을 연결하기 위한 정보가 들어 있는 UISceneConfiguration 객체를 말한다.
  - 일반적으로 info.plist에 추가된 기본값을 사용해서 생성한다.
- `scene(_:willConnectTo:options:)`
  - Scene이 연결될 것임을 delegate에 알려준다.
  - 기존에 application(_:didFinishLaunchingWithOptions:)에서 했던 UIWindow 생성 작업을 이 method에서 할 수 있다.
  - Storyboard를 사용한다면 storyboard에서 entry point를 찾아 내부적으로 UIWindow를 생성한다.
- `sceneDidBecomeActive(_:)` :  Inactive ➡️ Active

<br/>

#### 3. 앱 실행 후 홈 화면으로 나가면 : Active - Inactive - Background(-Suspended)

- 앱 실행 후 홈 화면으로 나가면 Active에서 Inactive를 거쳐 Background로 상태가 전환된다.
- 이 때, SceneDelegate는 Scene에 다음 순서로 method를 호출하도록 요청한다.
- `sceneWillResignActive(_:)` :  Active ➡️ Inactive
- `sceneDidEnterBackground(_:)` :  Inactive ➡️ Background

<br/>

#### 4. Background 상태에 있는 앱을 다시 실행하면 : Background - Inactive - Active 

- Background 상태에 있는 앱을 다시 실행하면 InActive를 거쳐 Active 상태로 전환된다.
- 이 때, SceneDelegate는 시스템에 다음 method를 호출하도록 요청한다.
- `sceneWillEnterForeground(_:)` :  Background ➡️ Inactive
- `sceneDidBecomeActive(_:)` :  Inactive ➡️ Active

<br/>

#### 5. Scene 연결 해제

- 기존 App-Based Life Cycle에서는 멀티태스킹 창에서 swipe up 제스처를 사용하여 앱을 종료시켰다.
- 하지만 Scene을 사용할 때는 multi window를 지원하기 때문에, 앱이 둘 이상의 scene window를 갖는다면 swipe up 제스처는 앱을 종료시키지 않고 Scene을 해제시키게 된다.
- 만약 모든 Scene의 연결이 해제되었다면 앱이 종료된다.
- Scene 연결이 해제될 때는 다음 순서로 method가 호출된다.
- `sceneDidDisconnected(_:)`
  - delegate에 UIKit에 연결된 Scene의 연결을 해제할 것을 요청한다.
- `application(_:didDiscardSceneSessions:)`
  - 사용자가 멀티태스킹 화면(app switcher)에서 한개 이상의 Scene을 종료시켰을 때 호출된다.
- `applicationWillTerminate(_:)`
  - 앱이 사용자에 의해 종료될 때 호출된다.
  - 시스템에 의해 예기치 못한 상황에서 종료될 때는 호출되지 않는다.

<br/>

## ✏️ 백그라운드에서 작업을 완료하기 위한 방법은 어떤 것이 있나요?

---

### 1. Background Tasks (백그라운드 작업)

- iOS는 특정 종류의 백그라운드 작업을 허용한다. 

### 2. Background Modes (백그라운드 모드)

- iOS 앱이 백그라운드에서 실행 가능한 기능을 설정하는 옵션이다.
- ➡️ Background Modes로 부터 승인된 기능을 Background Tasks API로 구현하는 것이다.

### 3. Background Fetch (백그라운드 패치)

### 4. Slient Push Notifications (조용한 푸시 알림)

### 5. URL Session Background Transfer (URL 세션 백그라운드 전송)

- 앱은 URL 세션을 사용하여 백그라운드에서 데이터를 업로드하거나 다운로드할 수 있다.

<br/>

## ✏️ Auto Layout을 사용하는 이유와 장점은 무엇인가요?

---

### Auto Layout

- 앱 화면을 다양한 기기와 해상도에서 <span style="color:#9fb584">**일관성**</span> 있게 보여주기 위한 레이아웃 시스템이다.
- 뷰의 제약 사항을 바탕으로 뷰 체계 내의 모든 뷰의 크기와 위치를 <span style="color:#9fb584">**동적으로 계산**</span> 한다.
  - 앱을 사용할 때 발생하는 외부 변경과 내부 변경에 동적으로 반응하는 사용자 인터페이스를 가능하게 한다.
- <span style="color:#9fb584">**제약조건(Constraints)**</span>을 사용해 화면 요소의 위치와 크기를 지정한다.
- 각 요소는 다른 요소나 화면의 특정 위치를 기준으로 배치되며, 화면이 회전하거나 크기가 바뀌어도 제약 조건에 따라 <span style="color:#9fb584">**유동적으로 크기와 위치가 자동으로 조정**</span>된다.

<br/>


#### 사용하는 이유

- 인터페이스의 <span style="color:#9fb584">**절대적인 좌표가 아닌 동적으로 상대적인 좌표가 필요한 경우 사용**</span>한다.
  - 앱이 실행되는 iOS 기기의 스키린 화면 크기가 다양한 경우
  - iOS 기기의 스크린이 회전할 수 있는 경우
  - 상태 표시줄에 전화 중임을 나타내는 액티브 콜과 오디오 바가 보여지거나 사라지는 경우
  - 앱의 콘텐츠가 동적으로 보여지는 경우
  - 앱이 동적 타입을 지원하는 경우

<br/>

#### 장점

1. 동적 레이아웃 지원 : 화면 크기나 방향이 바뀌어도 레이아웃이 적절하게 조정된다.
2. 유지보수 편리성 : 코드보다는 시각적으로 제약을 설정해 레이아웃 수정, 유지가 쉬워진다.
3. 다양한 기기 대응 : 아이폰, 아이패드와 같은 여러 기기에서 인터페이스를 일관되게 표현할 수 있다.

<br/>

#### 주요 요소

1. 제약 조건 (Constraints)
  - 요소의 <span style="color:#9fb584">**위치, 크기, 여백, 비율 등 레이아웃에 영향**</span>을 주는 규칙이다.
    - 버튼이 화면 가운데 위치하도록 제약 조건을 설정할 수 있다.
    - 화면의 양옆에 일정한 여백을 두고 배치하는 제약 조건을 설정할 수 있다.
  - 제약 조건을 활용하면 요소들이 서로 겹치거나 일관되지 않는 위치에 배치되는 것을 방지할 수 있다.
2. 앵커 Anchor
  - 요소 간 <span style="color:#9fb584">**관계를 설정**</span>할 수 있는 포인트이다.
  - 뷰의 topAnchor와 다른 뷰의 bottomAnchor를 연결하면 두 뷰 사이에 원하는 간격을 설정할 수 있다.
  - topAnchor, bottomAnchor, leadingAnchor, trailingAnchor 등을 활용해 요소 간 위치를 유연하게 설정할 수 있다.
3. 고유 콘텐츠 사이즈 (Intrinsic Content Size)
  - <span style="color:#9fb584">**뷰의 콘텐츠에 따라 정해지는 기본 크기**</span>이다.
    - ex) UILabel, UIButton, UISwitch, UISegementedControl 등
    - 고유 콘텐츠가 포함된 UI들은 사이즈에 맞춰 조건이 자동으로 만들어진다.
    - 안에 들어가 있는 고유 콘텐츠(폰트 크기, 텍스트의 양, 아이콘, 이미지 등)의 크기에 맞게 자동으로 조건을 설정한다.
    - 개발자가 따로 설정해주지 않아도 된다.
      - ex) 메시지 앱 : 텍스트 양에 따라 말풍선 모양 뷰의 크기가 달라짐
  - UILabel은 텍스트 내용에 따라 기본 크기가 결정되며, Auto Layout은 이 값을 이용해 요소의 크기를 자동으로 설정할 수 있다.
  - 레이아웃의 간결함과 유연함을 제공한다.

<br/>

> 고유 콘텐츠가 없는 뷰 <br/>
> `UIView`, `UIStackView` : 아예 없음<br/>
> `UITextview` : 보통은 있고, 스크롤 가능하게 설정하면 없음<br/>
> `UIImageview`: 이미지가 로드되면 있음<br/>
> `UISlider` : '세로(Y축)' 사이즈만 없음<br/>
> 위를 제외한 뷰들은 모두 고유 콘텐츠가 있다.

<br/>

## ✏️ 제약 조건(Constraints)의 우선순위(Priority)는 어떻게 동작하나요?

---

- Auto Layout의 <span style="color:#9fb584">**우선순위**</span>는 <span style="color:#9fb584">**각 제약 조건이 레이아웃에 얼마나 강하게 적용될지를 결정**</span>하는 요소이다.
- 우선순위는 1에서 1000까지의 값을 가지며, 1000은 필수로 적용되는 제약 조건을 의미하고 그 외의 값은 선택적 제약 조건이다.
  - 우선순위가 높은 제약 조건이 우선적으로 적용되며, 제약 조건이 서로 충돌하는 경우 우선순위가 낮은 제약 조건은 무시된다.
  - 디폴트 우선순위는 1000이며, 필요에 따라 이보다 낮은 값을 지정해 제약 조건을 유연하게 설정할 수 있다.

<br/>

## ✏️ Ambiguous Layout과 Unsatisfiable Constraints는 무엇이며, 어떻게 해결하나요?

---

### Ambiguous Layout

- <span style="color:#9fb584">**뷰의 위치나 크기가 모호하여 정확히 결정되지 않은 상태**</span>이다.
- 필요한 제약 조건이 부족하거나, 서로 모순되지 않지만 충분하지 않은 제약 조건이 있을 때 발생한다.
- 해결 방법
  - <span style="color:#9fb584">**필수 제약 조건을 추가**</span>하여 뷰의 크기와 위치가 명확하게 결정되도록 한다.
  - Xcode의 디버깅 기능에서 애매한 레이아웃을 감지할 수 있으며 `Resolve Auto Layout Issues`를 통해 문제를 확인하고 해결할 수 있다.

<br/>

### Unsatisfiable Constraints

- <span style="color:#9fb584">**제약 조건들이 서로 충돌하여 해결이 불가능한 상태**</span>이다.
- 상반된 제약 조건이 동시에 적용되었거나, 논리적으로 충돌하는 제약 조건이 설정되어 있을 때 발생한다.
- 해결 방법
  - 충돌하는 제약 조건을 확인하고, <span style="color:#9fb584">**우선순위를 조정하거나 불필요한 제약 조건을 삭제**</span>한다.
  - Xcode 콘솔에서 오류 메시지를 통해 문제의 원인이 되는 제약 조건을 파악할 수 있다.

<br/>

## ✏️ Swift에서 클로저(Closure)란 무엇이며, 어떻게 사용하나요?

---

### 클로저 Closure

- 코드에서 <span style="color:#9fb584">**일급 객체로 취급되는 함수의 일종**</span>이다.
  - 클로저는 `Named Closure`, `Unnamed Closure`가 있다.
  - `Named Closure` : 이름이 있는 함수로 클로저라 부르지 않고 함수라 한다.
  - `Unnamed Closure` : 익명 함수로 보통 클로저는 익명 함수를 말한다.
- <span style="color:#9fb584">**이름이 없고 주변의 변수와 상수를 캡쳐할 수 있다는 점에서 함수와 구분**</span>된다.
  - 전역 함수는 주변의 어떤 값도 캡쳐하지 않는다.
  - 중첩 함수는 자신을 포함하고 있는 함수의 값을 캡쳐한다.
- 클로저는 <span style="color:#9fb584">**보통 함수나 메서드의 인수로 전달되어, 나중에 실행될 코드 블록을 정의**</span>하는 데 사용된다.

<br>

> 함수와 차이점<br/>
> 1. 이름 유무 : 함수는 이름이 있는 코드 블록이고, 클로저는 이름이 없는 코드 블록이다. (주로 변수에 할당하거나 인자로 전달됨)<br/>
> 2. 구문 축약 : 클로저는 간결성을 위해 파라미터 및 리턴 타입 추론, 키워드 생략이 가능하다.<br>
> 3. 선언 위치<br>
>   - 함수는 특정 컨텍스트에서 `독립적`으로 선언된다. (클래스, 구조체, 파일 레벨 등)<br>
>   - 클로저는 코드 블록 내부에서 `즉석으로 정의`될 수 있다.(배열의 map 메소드나 completionHandler 등)<br>
> 4. 캡처 : 클로저는 외부 컨텍스트의 변수나 상수를 캡쳐할 수 있다.<br>
>   - 이를 통해 외부 변수의 값을 참조하거나 수정할 수 있다.<br>
>   ``` swift
func makeIncrementer(incrementAmount: Int) -> () -> Int {
    var total = 0
    return {
        total += incrementAmount
        return total
    }
}
let incrementer = makeIncrementer(incrementAmount: 2)
print(incrementer()) // 2
print(incrementer()) // 4
>   ```
> 5. 키워드 : 함수는 func 키워드를 사용해 정의되지만, 클로저는 {}를 사용해 정의된다.<br>
> 6. 사용 방식<br>
>   - 함수는 명시적 호출과 더불어 객체의 메서드로 주로 사용된다.<br>
>   - 클로저는 콜백(callback), 고차 함수의 매개변수로 주로 사용된다<br>


<br>

#### 클로저 표현식

- `func` 키워드를 사용하지 않는다.
- 클로저 헤드와 클로저 바디로 이뤄지고, 이 둘을 구분 지어주는 것이 `in` 키워드 이다.
  - 클로저 헤드 : `(parameters) -> returnType`
  - 클로저 바디 : 실행 구문
- 클로저는 함수로 1급 객체이기 때문에 상수에 클로저를 대입할 수 있다.
  - 파라미터와 리턴 타입을 추론할 수 있으면 생략할 수 있다.
  - Return Type이 있어도 생략 가능하다.
  - 함수에서 되지 않는 Parameter 생략이 가능하다.

``` swift
// 클로저 표현식
{ (parameters) -> returnType in
    // 실행 구문
}

// Parameter와 Return Type이 둘 다 없는 클로저
let closure = { () -> () in
    print("Closure")
}
```

- 클로저에서는 Argument Label을 사용하지 않는다.
  - `name`이 단독으로 쓰였으니 Argument Label이자, Parameter Name이라 생각할 수 있지만, `name`은 오직 Parameter Name이다.

``` swift
// Parameter와 Return Type이 있는 클로저
let closure = { (name: String) -> String in
    return "Hello, \(name)"
}

closure("gyeomji")
closure(name: "gyeomji")  // error
```

- 클로저를 변수나 상수에 대입할 수 있다.
  - 대입된 변수, 상수로 실행할 수 있다.

``` swift

let closure = { () -> () in
    print("Closure")
}


let closure2 = closure
```

- 함수의 파라미터 타입으로 클로저를 전달할 수 있다.

``` swift
func doSomething(closure: () -> ()) {
    closure()
}


doSomething(closure: { () -> () in
    print("Hello!")
})
```

- 함수의 반환 타입으로 클로저를 사용할 수 있다.

``` swift
func doSomething() -> () -> () {
    
    return { () -> () in
        print("Hello gyeomji!")
    }
}


let closure = doSomething()
closure()
```

- 클로저가 대입된 변수, 상수로 호출하거나 클로저를 직접 실행할 수 있다.
  - 클로저를 `()`로 감싸고, 마지막 호출 구문인 `()`를 추가하여 직접 실행한다.

``` swift
// 상수 closure 호출
let closure = { () -> String in
    return "Hello gyeomji!"
}

closure()

// 직접 실행
({ () -> () in
    print("Hello Sodeul!")
})()
```

<br/>

## ✏️ 트레일링 클로저(Trailing Closure) 문법은 어떤 경우에 유용한가요?

---

### 트레일링 클로저(Trailing Closure)

- 클로저를 간결하게 작성하는 경량 문법 중 하나이다.
  - 함수 호출이 읽기 쉽고 간결하게 작성될 수 있다.
  - 괄호 누락 실수를 줄일 수 있다.
- 함수의 <span style="color:#9fb584">**마지막 파라미터가 클로저**</span>일 때, 이를 파라미터 값 형식이 아닌 함수 뒤에 붙여 작성하는 문법이으로, 이때 <span style="color:#9fb584">**Argument Label은 생략**</span>된다.
- 파라미터가 클로저 하나일 경우 호출 구문인 `()`도 생략 가능하다.

``` swift
/// 파라미터가 클로저 하나인 함수
func doSomething(closure: () -> ()) {
    closure()
}
// 호출
// Inline Closure
doSomething(closure: { () -> () in
    print("Hello!")
})

// Trailing Closure
doSomething () { () -> () in
    print("Hello!")
}

// 파라미터가 클로저 하나일 경우 () 생략 가능
doSomething { () -> () in
    print("Hello!")
}
```

- 클로저가 <span style="color:#9fb584">**파라미터 값 형식으로 함수 호출 구문 ()안에 작성되어 있는 것**</span>을 `Inline Closure`라고 한다.
  - 이 클로저를 파라미터 값 형식으로 보내는 것이 아닌, 함수의 가장 마지막에 클로저를 꼬리처럼 덧붙여 사용할 수 있다. ➡️ `Trailing Closure`
- 이 클로저는 첫 파라미터이자 <span style="color:#9fb584">**마지막 파라미터**<span>이므로 트레일링 클로저가 가능
- `closure`라는 <span style="color:#9fb584">**ArgumentLabel은 트레일링 클로저에선 생략**</span>됨

``` swift
/// 파라미터가 여러 개인 함수
func fetchData(success: () -> (), fail: () -> ()) {
    //do something...
}

// 호출
// Inline Closure
fetchData(success: { () -> () in
    print("Success!")
}, fail: { () -> () in
    print("Fail!")
})

// Trailing Closure

fetchData(success:  { () -> () in
    print("Success!")
}) { () -> () in
    print("Fail!")
}
```

- `fail` 클로저는 <span style="color:#9fb584">**마지막 파라미터**<span>이므로 트레일링 클로저가 가능
- `fail`이라는 <span style="color:#9fb584">**ArgumentLabel은 트레일링 클로저에선 생략**</span>됨
- 파라미터가 여러 개일 경우 호출 구문인 `()`을 생략하면 안된다.
  - `success`란 파라미터는 파라미터 값 타입으로 넘겨줘야 하기 때문이다.

<br>

#### 클로저의 경량 문법

- 문법을 최적화하여 클로저를 단순하게 쓸 수 있게 하는 것이다.

1. 파라미터 형식과 리턴 형식을 생략할 수 있다.
2. Parameter Name은 `Shortand Argument Names`으로 대체하고, 이 경우 Parameter Name과 in 키워드를 삭제한다.
  - 축약 인자 이름 (Shortand Argument Names) : $와 index를 이용해 Parameter에 순서대로 접근하는 것이다.
    - ex) $0, $1
3. 단일 리턴문만 남을 경우, return도 생략한다.
4. 클로저 파라미터가 마지막 파라미터면, 트레일링 클로저로 작성한다.
5. ()에 값이 아무 것도 없다면 생략한다.

``` swift
func doSomething(closure: (Int, Int, Int) -> Int) {
    closure(1, 2, 3)
}

// 호출  Inline Closure 방식
doSomething(closure: { (a: Int, b: Int, c: Int) -> Int in
    return a + b + c
})

// 1. 파라미터 형식과 리턴 형식을 생략할 수 있다
doSomething(closure: { (a, b, c) in
    return a + b + c
})

// 2. Parameter Name은 Shortand Argument Names으로 대체하고, 이 경우 Parameter Name과 in 키워드를 삭제한다
doSomething(closure: {  
    return $0 + $1 + $2
})

// 3. 단일 리턴문만 남을 경우, return도 생략한다
doSomething(closure: {  
     $0 + $1 + $2
})

// 4. 클로저 파라미터가 마지막 파라미터면, 트레일링 클로저로 작성한다
doSomething() {  
     $0 + $1 + $2
}

// 5. ()에 값이 아무 것도 없다면 생략한다
doSomething {  
     $0 + $1 + $2
}

```

<br>

#### @autoclosure

- 파라미터로 전달된 일반 구문 & 함수를 클로저로 래핑(Wrapping) 하는 것이다.
- `@autoclosure` 키워드를 파라미터 함수 타입 정의 바로 앞에다가 붙인다.
  - `closure` 파라미터는 실제 클로저를 전달받지 않지만, 클로저처럼 사용 가능하다.
  - 실제 클로저를 전달하는 것이 아니기 때문에 파라미터로 값을 넘기는 것 처럼 ()를 통해 구문을 넘겨줄 수가 있다.
- 리턴 타입은 상관없지만, 반드시 파라미터가 없어야 한다.
- 일반 구문은 작성되자마자 실행되지만, autoclosure로 작서하면 함수 내에서 클로저를 실행할 때까지 구문이 실행되지 않는다.
  - 함수가 실행될 시점에 구문을 클로저로 만들어 주기 때문이다.
- 따라서 autoclosure는 원래 <span style="color:#9fb584">**바로 실행되어야 하는 구문이 지연되어 실행한다는 특징**</span>이 있다.

``` swift
func doSomething(closure: @autoclosure () -> Bool) {
  closure()
}

// 1 > 2 는 클로저가 아닌 일반 구문이지만 클로저로 래핑했기 때문에 클로저처럼 사용할 수 있다.
doSomething(closure: 1 > 2)

```

<br>

## ✏️ @escaping 클로저와 non-escaping 클로저의 차이점은 무엇인가요?

---

### @escaping

- non-escaping Closure
  - 함수 내부에서 직접 실행하기 위해서만 사용한다.
  - 따라서 파라미터로 받은 클로저를 변수나 상수에 대입할 수 없고, 중첩 함수에서 클로저를 사용할 경우, 중첩함수를 리턴할 수 없다.
  - 함수의 실행 흐름을 탈출하지 않아, 함수가 종료되기 전에 무조건 실행 되어야 한다.
  > [ 위의 조건들이 붙은 이유 ]<br><br>
  클로저가 함수 외부로 탈출하지 못하게 하기 위해 위의 조건들이 붙었다.<br>
  > non-escaping 클로저는 해당 클로저가 함수가 종료되기 직전에 무조건 실행이 된다는 조건이다.<br>
  > 만약 변수나 상수에 대입할 경우, 해당 클로저가 변수나 상수로 함수에서 리턴될 수도 있고, 중첩 함수 내부에서 클로저를 사용한 경우, 중첩 함수가 클로저를 캡쳐하기 때문에 중첩 함수를 리턴 시 클로저가 중첩 함수에 의해 함수 외부에서 실행될 수 있기 때문이다.<br>
- <span style="color:#9fb584">**함수 실행을 벗어나 함수가 끝난 후에도 클로저를 실행하거나, 중첩함수에서 실행 후 중첩 함수를 리턴하고 싶거나, 변수 상수에 대입하고 싶은 경우**</span> `@escaping` 키워드를 사용한다.
- 클로저 파라미터 타입 앞에 `@escaping` 키워드를 붙여 사용한다.
- escaping의 경우 함수가 종료되더라도 실제 클로저가 사용되지 않을 때까지 메모리를 추적해야 한다.
  - non-escaping의 경우 함수가 종료됨과 동시에 클로저도 사용이 끝난다.
  - 클로저가 해당 함수 내부에서만 쓰이기 때문에 컴파일러가 메모리 관리를 지저분하게 하지 않아도 돼서, 성능이 향상된다.

``` swift
// 상수에 클로저 대입
func doSomething(closure: @escaping () -> ()) {
    let f: () -> () = closure
}

// 함수가 종료된 후 클로저 실행
func doSomething(closure: @escaping () -> ()) {
    print("func start")
    
    DispatchQueue.main.asyncAfter(deadline: .now() + 10) {
        closure()
    }
    print("func end")
}

doSomething {
    print("closure")
}

//func start
//func end
//10초 뒤
//closure

```

<br/>

## ✏️ 클로저의 캡처(Capture) 기능은 무엇인가요?

---

### 클로저의 캡쳐

``` swift
// closure 실행 전 num 값 외부에서 변경 시 클로저 내부의 num 값도 변경
func doSomething() {
    var num: Int = 0
    print("num check #1 = \(num)")  // 0
    
    let closure = {
        print("num check #3 = \(num)") // 20
    }
    
    num = 20
    print("num check #2 = \(num)") // 20
    closure()
}


// closure 내부에서 num 값 변경 시 클로저 외부의 num 값도 변경
func doSomething() {
    var num: Int = 0
    print("num check #1 = \(num)") // 0
    
    let closure = {
        num = 20
        print("num check #3 = \(num)") // 20
    }
    
    closure()
    print("num check #2 = \(num)") // 20
}
```
- closure는 내부에서 외부 변수인 `num`이라는 `Value`타입의 변수를 사용하기 때문에 <span style="color:#9fb584">**num 값을 내부적으로 저장하는데 이것을 num 값이 캡쳐되었다**</span>라고 표현한다.
  - message란 변수는 closure 내부에서 사용하지 않기 때문에 따로 저장하지(캡쳐되지) 않는다.
- 클로저는 값을 캡처할때 <span style="color:#9fb584">**Value/Reference 타입에 관계 없이 Reference Capture**</span> 한다.
  - 클로저의 변수가 사용되는 시점의 변수 값을 평가한다.
  - 외부 값이 Value & Reference Type이든 모두 참조하는 것이다.

<br/>

#### Capture List

- `Capture List`를 이용해 Value 타입 값을 복사해 캡쳐할 수 있다.
- Capture List는 Copy해서 캡쳐할 변수를 명시해주는 것이다.
-  클로저의 시작인 { 의 바로 옆에  []를 이용해 나열한다.
-  이때 in 키워드도 함께 작성한다.
-  Closure를 선언할 당시의 [변수] 값을 `Const Value Type`으로 캡쳐한다.
-  즉, <span style="color:#9fb584">**상수로 캡쳐**</span>된다.
-  클로저 내부에서 [변수]의 값을 변경할 수 없다.
-  Reference Type 의 경우 Capture List를 이용해도 Reference Capture가 된다.

``` swift
// Copy 캡쳐할 변수들을 [] 안에 나열한다.
let closure = { [num, num2] in

}

// 클로저 실행 전 외부에서 num 값을 변경해도 클로저의 num 값에 영향주지 않음
func doSomething() {
    var num: Int = 0
    print("num check #1 = \(num)") // 0
    
    let closure = { [num] in
        print("num check #3 = \(num)") // 0
    }
    
    num = 20
    print("num check #2 = \(num)") // 20
    closure()
}
```

<br/>

#### 클로저와 ARC

- 클로저는 참조 타입으로 Heap에 저장된다.
- 클로저는 Reference 값을 캡쳐할 때 기본적으로 `strong`으로 캡쳐한다.
  - 이로 인해 메모리 누수가 발생할 수 있다.

``` swift
class Person {
    var name = ""
    // 해당 프로퍼티가 self로 자신의 인스턴스에 접근하고 있다.
    // 인스턴스의 초기화가 다 진행되어 self를 사용할 수 있게 된 후 해당 프로퍼티를 초기화 시킬 수 있기 때문에 lazy로 선언해야 한다.
    lazy var getName: () -> String = {
        return self.name
    }
    
    init(name: String = "") {
        self.name = name
    }
    
    deinit {
        print("Person Deinit")
    }
}

// gyeomji 인스턴스를 만든 후 클로저로 작성된 getName 지연 저장 프로퍼티를 호출
var gyeomji: Person? = .init(name: "gyeomji")
print(gyeomji!.getName())

// gyeomji 인스턴스에 nil을 할당하고, gyeomji를 다른 변수에 대입하지 않음
// 인스턴스의 RC가 0이 되어 denit 함수가 호출되어야 하지만 호출되지 않음
gyeomji = nil
```

- Person 인스턴스는 getName을 호출하는 순간 getName 클로저가 힙에 할당되며 해당 클로저를 <span style="color:#9fb584">**참조**</span>하게 된다.
  - 지연 저장 프로퍼티니 인스턴스 생성 직후가 아닌, 호출되는 순간에 메모리에 올라가게 된다.
- getName은 self를 통해 Person 인스턴스의 프로퍼티에 접근하고 있다.
- 클로저는 참조 값을 캡쳐할 때 기본적으로 `strong`으로 캡쳐한다.
- 따라서 이때 Person 인스턴스의 Reference Count가 증가한다.
  - Person 인스턴스는 클로저를 참조하고 클로저는 Person 인스턴스(의 변수)를 참조한다.
  - 서로가 서로를 참조하고 있어 둘 다 메모리에서 해제되지 않는 <span style="color:#9fb584">**강한 순환 참조가 발생**</span>한다.
- `weak`, `unowned`와 `Capture Lists`를 사용해 강한 순환 참조를 해결한다.
- <span style="color:#9fb584">**self에 대한 참조를 Closure Capture Lists를 이용해 weak, unowned로 캡쳐**</span>한다.
  - deinit이 정상 실행 된다.
- `weak`의 경우 nil을 할당받을 가능성이 있기에  Optional-Type으로 self에 대한 Optional Binding을 해준다.
- `unowned`의 경우 Non-Optional Type으로 self에 대한 Optional Binding 없이 사용할 수 있다.
  -  Lazy로 선언된 클로저가 self를 unowned로 캡쳐하고, 시점 차이로 인해 해당 인스턴스에 nil이 할당 된 후에도 lazy property 작업이 실행되어야 하는 상황이 생길 경우 문제가 생길 수 있다.

``` swift
class Person {
    var name = ""
    // weak
    lazy var getName: () -> String? = { [weak self] in
        return self?.name
    }
    lazy var getName: () -> String = { [unowned self] in
        return self.name
    }
    
    init(name: String = "") {
        self.name = name
    }
    
    deinit {
        print("Person Deinit")
    }
}

var gyeomji: Person? = .init(name: "gyeomji")
print(gyeomji!.getName())

gyeomji = nil

// gyeomji
// Person Deinit
```

<br/>

## ✏️ iOS에서 Delegate 패턴은 무엇이며, 어떤 상황에서 사용되나요?

---

### Delegate 패턴

- 델리게이트 패턴은 <span style="color:#9fb584">**어떤 객체의 행동을 다른 객체에 위임하기 위해 사용**</span>되는 패턴이다.
  - 특정 객체가 해야 할 일을 다른 객체가 대신 처리하도록 할 수 있다.
- 이벤트나 작업 처리 로직을 외부 객체에 위임하여 <span style="color:#9fb584">**코드의 재사용성을 높이고 객체 간 결합도를 낮출 수 있다.**</span>
- 이벤트를 <span style="color:#9fb584">**명확히 정의된 프로토콜을 통해 전달**</span>한다.
- 주로 <span style="color:#9fb584">**이벤트를 1:1로 전달**</span>할 때 많이 사용된다.
- 사용 사례
  - 주로 Controller와 다른 객체 사이의 관계를 다룬다.
  - UITableView, UICollectionView: 데이터 소스와 사용자 인터랙션 처리를 UITableViewDelegate, UITableViewDataSource로 위임.
  - 커스텀 뷰의 이벤트 처리: 사용자 정의 UI 요소가 특정 동작을 다른 객체에 위임할 때.
  - 비동기 작업 결과 전달: 네트워크 요청의 결과를 호출한 객체로 전달.
- 보통 Protocol을 정의하여 사용한다.
  - 델리게이트로 지정된 객체가 해야하는 메소드들의 원형을 적는다.
  - Delegate 역할을 하려는 객체는 이 Protocol을 따르며 원형만 있던 메소드들의 구현을 한다.
  - 세팅 후 이전 객체(SomeView)는 어떤 이벤트가 일어났을 시 delegate로 지정한 객체(SomeController)에 알려줄 수 있다.
- 장점
  - 프로토콜에 필요한 메소드들이 명확하게 명시됨.
  - 컴파일 시 경고나 에러가 떠서 프로토콜의 구현되지 않은 메소드를 알려줌.
  - 프로토콜 메소드로 알려주는 것뿐만이 아니라 정보를 받을 수 있음.
  - 커뮤니케이션 과정을 유지하고 모니터링하는 제 3의 객체(ex: NotificationCenter 같은 외부 객체)가 필요없음.
  - 프로토콜이 컨트롤러의 범위 안에서 정의됨.
- 단점
  - delegate 설정에 nil이 들어가지 않게 주의해야함. 
    - 크래시를 일으킬 수 있음.
  - 많은 객체들에게 이벤트를 알려주는 것이 어렵고 비효율적임.
  - 객체 간 결합도가 높아질 수 있다.
- 강한 순환 참조를 주의해야 한다.
  - Delegate 패턴을 구현할 때, 위임자(Delegator)가 delegate 프로퍼티를 통해 Delegate 객체를 참조한다.
  - 반대로 Delegate 객체도 위임자를 강한 참조(예: 프로퍼티)를 통해 보유하고 있는 경우, 강한 참조 순환이 발생한다.
  - ex) A 뷰 컨트롤러가 B 뷰 컨트롤러를 Delegate로 설정, B 뷰 컨트롤러가 A 뷰 컨트롤러를 강하게 참조
  - Delegate 프로퍼티를 weak로 선언하여 순환 참조를 방지한다.
  - 중재 객체를 사용해 간접적으로 연결한다.
  - unowned 를 사용한다.
    - 특정 상황에서 Delegate 객체가 항상 위임자보다 더 오래 살아있을 것이 확실한 경우, unowned를 사용할 수 있다.

``` swift
// 1) Delegate 프로토콜 선언
protocol SomeDelegate {
    func someFunction(someProperty: Int)
}

// 작업 위임 객체 Sender
class SomeView: UIView {

    // 2) 강한 순환 참조를 막기 위해 weak으로 delegate 프로퍼티를 가지고 있음 
    // (Delegate 속성 선언)
    weak var delegate: SomeDelegate?
    
    func someTapped(num: Int) {
        // 3) 이벤트가 일어날 시 delegate가 동작하게끔 함
        // (Delegate 호출)
        delegate?.someFunction(someProperty: num)
    }
}

// Delegate 구현 객체 Receiver
// 4) Delegate 프로토콜을 따르도록 함
class SomeController: SomeDelegate {
    var view: SomeView?
    
    init() {
        view = SomeView()
        // 6) delegate를 자신으로 설정
        view?.delegate = self
        someFunction(someProperty: 0)
    }
    
    // 5) Delegate 프로토콜에 적힌 메소드 구현
    func someFunction(someProperty: Int) {
        print(someProperty)
    }
}

let someController = SomeController()
// prints 0
```

<br/>

### Notification

- Notification Center라는 싱글턴 객체를 통해서 이벤트들의 발생 여부를 옵저버를 등록한 객체들에게 Notification을 post하는 방식으로 사용한다.
- Notification name이라는 key 값을 통해 보내고 받을 수 있다.
- <span style="color:#9fb584">**1:N 관계**</span>에서 사용한다.
- 앱 전체에 이벤트를 방송(Broadcast)하여 여러 객체가 이를 수신한다.
- 장점
  - 결합이 느슨하다.
  - 다수의 객체들에게 동시에 이벤트의 발생을 알려줄 수 있음.
  - Notification과 관련된 정보를 Any? 타입의 object, [AnyHashable: Any]? 타입의 userInfo로 전달할 수 있음.
- 단점
  - key 값으로 Notification의 이름과 userInfo를 서로 맞추기 때문에 컴파일 시 구독이 잘 되고 있는지, 올바르게 userInfo의 value를 받아오는지 체크가 불가능함.
  - 추적이 쉽지 않을 수 있음.
  - Notificaiton post 이후 정보를 받을 수 없음.
  - 수신 대상이 명시적이지 않음.

``` swift
// 1) Notification을 보내는 ViewController
class PostViewController: UIViewController {
    @IBOutlet var sendNotificationButton: UIButton!
    
    @IBAction func sendNotificationTapped(_ sender: UIButton) {
        guard let backgroundColor = view.backgroundColor else { return }
      
        // Notification에 object와 dictionary 형태의 userInfo를 같이 실어서 보낸다.
        NotificationCenter.default.post(name: Notification.Name("notification"), object: sendNotificationButton, userInfo: ["backgroundColor": backgroundColor])
    }
}

// 2) Notification을 받는 ViewController
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 옵저버를 추가해 구독이 가능하게끔 함
        NotificationCenter.default.addObserver(self, selector: #selector(notificationReceived(notification:)), name: Notification.Name("notification"), object: nil)
    }
    
    // iOS 9 이상이 아닐 경우에는 removeObserver를 해줘야 함
    deinit {
        NotificationCetner.default.removeObserver(self)
    }
    
    @objc func notificationReceived(notification: Notification) {
        // Notification에 담겨진 object와 userInfo를 얻어 처리 가능
        guard let notificationObject = notification.object as? UIButton else { return }
        print(notificationObject.titleLabel?.text ?? "Text is Empty")
        
        guard let notificationUserInfo = notification.userInfo as? [String: UIColor],
            let postViewBackgroundColor = notificationUserInfo["backgroundColor"] else { return }
        print(postViewBackgroundColor)
    }
}
```

<br/>

### Key Value Observing (KVO)

- A 객체에서 B 객체의 <span style="color:#9fb584">**프로퍼티가 변화됨을 감지**</span>할 수 있는 패턴이다.
- <span style="color:#9fb584">**객체와 객체 사이의 관계**</span>를 다루는데 적합하다.
- 메소드나 다른 액션에서 나타나는 것이 아니라 프로퍼티의 상태에 반응하는 형태이다.
- observe하려는 프로퍼티에 @objc attribute와 dynamic modifier를 추가해야한다.
- 장점
  - 두 객체 간 동기화가 가능하다.
  - 객체의 구현을 변경하지 
  - 321?앟고 내부 객체의 상태 변화에 대응할 수 있다.
  - new/old value를 쉽게 얻을 수 있음.
  - key path로 옵저빙하기 때문에 nested objects도 옵저빙이 가능함.
- 단점
  - NSObject를 상속받는 객체에서만 사용이 가능함.
    - Objective-C 런타임에 의존하게 된다.
    - 상속을 해야하므로 class에서만 사용 가능하다.
  - dealloc될 때 옵저버를 지워줘야 함.
  - 많은 value를 감지할 때는 많은 조건문이 필요.

``` swift
class Person: NSObject {
    let name: String
    @objc dynamic var age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}

var person = Person(name: "gyeomji", age: 20)

person.observe(\.age , options: [.old, .new]) { (object, change) in
    print("\(object.name)'s age changed from \(change.oldValue!) to \(change.newValue!)")
}
person.age = 30 // gyeomji's age changed from 20 to 30
person.age = 40 // gyeomji's age changed from 30 to 40
```

<br/>

## ✏️ Swift의 기본 데이터 타입과 컬렉션(Collection) 타입에는 어떤 것들이 있나요?

---

### 기본 데이터 타입

- Int : 정수 타입 (64비트 정수)
- Double, Float : 실수 타입 (Double = 64, Float = 32비트)
- Bool : 논리 타입
- String : 문자 시퀀스 타입
- Character : 단일 문자 타입

<br/>

### 컬렉션 타입

- Array : 순서가 있는 요소의 집합으로 동일한 타입의 데이터만 포함할 수 있다.
  - 인덱스를 통해 요소에 접근할 수 있다.
  - 값 추가, 삭제가 가능하다.
- Set : 순서가 없는 고유한 요소의 잡합이다.
  - 빠른 검색이 가능하고, 중복 값을 허용하지 않는다.
- Dictionary : 키와 값 쌍으로 구성된 컬렉션으로 키를 통해 값에 접근할 수 있다.
  - 데이터를 키로 정렬하고 빠르게 접근할 수 있어 특정 항목을 식별하고 찾는데 효과적이다.

<br/>

## ✏️ 값 타입(Value Type)과 참조 타입(Reference Type)의 차이점은 무엇인가요?

---

- 값 타입은 데이터의 전체 값이 복사된다.
  - 즉 값을 다른 변수, 상수에 할당하거나 함수에 전달할 때 해당 값의 복사본이 생성되어 전달된다.
  - 데이터는 독립적인 복사본을 가지기 때문에 한 인스턴스의 값을 수정해도 다른 인스턴스에 영향을 주지 않는다.
  - 구조체, 열거형, 기본 데이터 타입들이 해당된다.
- 참조 타입은 데이터의 주소 값이 복사된다.
  - 다른 변수, 상수에 할당하거나 함수에 전달할 때 새로운 참조가 생성될 뿐 원본 데이터는 그대로 공유된다.
  - 여러 변수나 상수가 동일한 인스턴스를 가리키며 한 쪽에서 데이터를 수정하면 모든 참조에 영향을 미친다.
  - 클래스로 생성한 모든 타입이 해당된다.
- <span style="color:#9fb584">**데이터 변경이 불필요하거나 독립성을 유지해야 하는 경우 값 타입**</span>을, <span style="color:#9fb584">**공유된 리소스가 필요하거나 객체 간의 관계를 표현해야 하는 경우 참조 타입**</span>을 선택한다.

<br/>

## ✏️ 구조체(Struct)와 클래스(Class)의 사용 시기는 어떻게 구분하나요?

---

- 구조체는 값 타입으로 인스턴스를 생성하여 다른 인스턴스에 할당할 경우 전체 값이 복사된다.
  - 독립적인 데이터 복사가 필요한 경우
  - 데이터 중심의 객체를 정의할 경우
    - 데이터를 저장하고 전달하는데 초점을 맞춘 간단한 모델 객체에 적합하다.
  - 상속이 필요 없는 경우
  - Immutable 데이터로 관리하려는 경우
    - 구조체는 let으로 선언하면 불변성을 가진다.
    - 이로 인해 인스턴스를 변경할 수 없는 상태로 관리할 수 있어, 데이터가 변경되지 않아야 하는 상황에서 안전하게 사용할 수 있다.
  - 프로퍼티가 값 타입인 경우
- 클래스는 레퍼런스 타입으로 인스턴스를 생성하여 다른 인스턴스에 할당하면 주소 값이 복사된다.
  - 데이터의 공유나 상호 참조가 필요한 경우
    - 여러 인스턴스가 동일한 데이터에 접근하고 변경해야 하는 경우 유용하다.
  - 상속 계층이 필요한 경우
  - ARC가 필요한 경우
    - 주소값은 스택에, 데이터는 힙에 할당되고 이로 인해 reference counting이 발생한다. (ARC가 관리)
    - 복잡한 객체 그래프나 특정 객체의 수명 주기를 관리해야 하는 경우
  - 객체의 정체성을 유지하고 상태를 공유해야 하는 경우
    - 동일한 인스턴스를 여러 위치에서 참조해야 하는 경우
- <span style="color:#9fb584">**구조체는 주로 불변성, 독립성, 경량 데이터를 처리**</span>할 때 유용하며, <span style="color:#9fb584">**클래스는 복잡한 상태 관리, 공유, 상속 구조가 필요**</span>할 때 유용하다.

<br/>

## ✏️ 열거형(Enum)의 원시값(Raw Value)과 연관값(Associated Value)은 무엇인가요?

---

### 원시값

- 열거형의 각 케이스에 <span style="color:#9fb584">**고정된 기본 값을 지정**</span>할 수 있는 기능이다.
- 열거형을 정의할 때 모든 케이스에 동일한 타입의 원시값을 설정해야 한다.
- 원시값은 기본적으로 Int, String, Double 등의 리터럴 타입을 지원한다.
- 원시값이 있는 열거형은 rawValue 프로퍼티를 통해 각 케이스의 값을 참조할 수 있다.

``` swift
enum Direction: String {
    case north = "N"
    case south = "S"
    case east = "E"
    case west = "W"
}
```

<br/>

### 연관값

- 열거형의 각 케이스가 <span style="color:#9fb584">**개별적인 값**</span>을 가질 수 있는 기능이다.
- 연관값은 열거형의 각 케이스에 따라 다른 타입과 개수의 데이터를 가질 수 있다.
- 열거형의 인스턴스를 생성할 때 연관값을 함께 설정할 수 있으며, 이를 통해 추가적인 데이터를 저장하고 활용할 수 있다.

``` swift
enum Media {
    case photo(filename: String)
    case video(filename: String, duration: Int)
    case audio(filename: String, bitrate: Int)
}
```

<br/>

## ✏️ Xcode에서 디버깅 시 자주 사용하는 기능은 무엇인가요?

---

> 디버깅 : 프로그램에서 개발 단계 중 발생하는 오류를 찾아내 수정하거나 디버깅을 통해 테스트 데이터가 아닌 실제 데이터를 사용해 보기 위한 테스트를 방법으로 사용한다.<br/>
>   - 빠른 문제 해결
>   - 코드 품질 향상
>   - 어떤 지점에서 문제가 발생하는지 찾고, 해당 상황에 어떤 변수에 어떤 값이 들어왔는지 검색하기 위해 Breakpoint, Watchpoint를 활용한다.
> LLDB (Low-Level Debugger) : xcoded에 기본적으로 내장되어 있는 Command Line Debug환경이다. LLDB를 통해 실행되고 있는 프로세스 값을 확인할 수 있고, 프로세스 값을 변경하여 제어할 수 있다.

<br/>

### 1. `po` 명령어

- breakpoint를 기점으로 변수에 어떤 값이 저장되어 있는지 알 수 있다.
- LLDB에서 `po {변수}`를 입력하면 변수에 대한 description이 출력된다.
  - property 접근도 가능

![po](/assets/img/debugPo.png)

- 런타임 시 변수 값을 직접 수정할 수 있다.
- 런타임 시 직접 변수를 생성하고 사용할 수 있다.
  - `po var ${변수명} = {value}`

![po](/assets/img/debugPo2.png)

<br/>

### 2. breakpoint

- 디버깅을 목적으로 실행중인 디버깅 대상 프로세스를 의도적으로 멈추게 하는 장소를 가리킨다.
  - 프로세스가 멈추면 멈춘 시점의 변수나 스택 파라미터, 혹은 특정 메모리 지점의 값 등을 조사할 수 있다.
- 코드 왼쪽 라인을 클릭하여 breakpoint를 생성할 수 있다.
  - 이렇게 생성한 breakpoint를 `line breakpoint`라 한다.
- 생성한 breakpoint를 더블 클릭하거나, 오른쪽 클릭 후 Edit Breakpoint를 선택하면 `Edit Breakpoint` 창이 뜬다.
  - Name : breakpoint에 네이밍을 지정할 수 있다.
  - Condition : 특정 조건일 때만 break가 걸린다. (swift 문법으로 작성)
  - Ignore : 특정 횟수 이후부터 break가 걸린다.
  - Action : break가 걸리기 전, 특정 동작을 수행한다. (LLDB 명령어, script, Sound 등)
  - Automatically continue after evaluating actions : Edit Breakpoint에 설정한 동작은 수행하지만 break가 걸리지 않는다.
    - Action > Debugger Command에 p, po 명령어를 입력하고 해당 옵션을 체크하면 해당 구간에 print문을 삽입한 것과 같은 효과가 난다.
- `Column Breakpoint` : line breakpoint와 다르게 코드 라인 중 특정 부분에 breakpoint를 생성한다.
  - breakpoint를 걸 곳에 command + 클릭 > Create Column Breakpoint로 생성할 수 있다.
- `Watchpoint` : 변수 값이 수정되는 곳에 일일이 breakpoint를 만들지 않아도 break가 걸리도록 해준다.
  - 콘솔 왼쪽 화면에서 트래킹할 변수를 오른쪽 클릭하고 `Watch {변수}`를 클릭한다.
  - 변수 값이 수정되면 수정하는 곳에 break가 걸린다.
  - read, write, read-write, modified 타입이 있다.

<br/>

### 3. Debug Veiw Hierarchy

- 뷰의 계층 구조를 시각적으로 보여주고, stack으로도 보여주는 툴이다.

<br/>

### 5. Network

- `Network Condition`
  - 서버 요청/응답이 필요한 앱의 경우 여러 네트워크 상태에 대해 테스트가 필요하다.
    - 네트워크가 좋지 않을 경우, 네트워크가 끊겼을 경우 앱의 동작 확인 등
  - `Xcode > Window > Devices and Simulator > DEVICE CONDITIONS`에서 네트워크 상태를 조절할 수 있다.
  - 네트워크 100% 유실 뿐만 아니라 2G, 3G 환경 등 다양하게 선택할 수 있다.
- `Network Instrument`
  - 서버 요청/응답에 대한 정보를 알고 싶을 때 사용한다.
    - iOS 15 이상 디바이스에서만 확인이 가능하다.
    - 시뮬레이터는 불가능하다.
  - Instruments 앱을 실행하고 Network를 선택한다.
  - 녹화 버튼을 누른다.
  - HTTP Traffic에서는 Task Durations, URL Sessions Tasks, HTTP Transactions를 확인할 수 있다.


<br/>

## ✏️ 중단점(Breakpoint)의 종류와 활용 방법을 설명해주세요.

---

### 1. Source File Breakpoint

- 단일 파일에 설정된 Breakpoint이다.
- Line breakpoint
- Column breakpoint
  - 한 줄에 여러 개의 클로저가 중첩되어 있는 경우 사용한다.
  - 컴파일러는 디버그 환경에서 컴파일 할 때 Line table map을 생성한다.
  - 이때 생성한 map은 source line과 column을 컴파일된 주소에 매핑한다.
  - 이 line에 있는 각 클로저에 대해 컴파일러는 디버거가 일시 중지하는 데 사용할 line table 항목을 생성한다.
  - 한 줄의 마지막 클로저를 확인하고 싶을 때 마지막 클로저에 도달하려면 컴파일러가 디버그를 위해 여러 line table을 생성하고, 생성되는 line table로 인해 step-in/out을 여러 번 실행해야 한다.
  - column breakpoint를 사용하면 line table을 여러 개 생성할 필요가 없어 간단하게 디버깅 가능하다.

### 2. Swift Error Breakpoint

- Swift Error가 발생했을 때 중단하는 포인트이다.
- 시스템 프레임워크의 기본 에러 뿐 아니라 개발자가 커스텀 정의한 에러도 포함된다.
- 에러를 던지고 캐치하는 곳에 간단하게 line 중단점을 만들 수 있다. 
- 하지만 에러를 던지고 받는 사슬이 길 때 처음 에러가 발생한 지점을 알고 싶다든지, 그 지점에서 뭐가 잘못된 건지 찍어보고 싶다든지 하는 상황에서는 Swift Error 중단점이 제격이다. 
- 원하는 에러의 종류를 지정해줄 수도 있다.

### 3. Exception Breakpoint

- Objective-C의 Exception이 발생 또는 감지되면 멈춘다.
  - ex) Array의 index out of range
- swift에서는 NSException을 핸들링 할 수 없기에 크래시 발생 직전에 멈추려면 Objective-C 코드가 필요하다.
- 중단점이 없을 경우 Exception 발생 시 Error 메시지
  - 디버그 네비게이터에서 스레드 콜스택을 거슬러 발생 지점을 찾을 수 있지만, 그 범위에 들어가 디버깅 할 수 없다.
  - 중단점이 설정되어 있을 경우 크래시가 나기 직전 해당 지점에서 먼저 멈춘다.
  - 원인이 뭔지 디버깅 해볼 수 있다.

### 4. Symbolic Breakpoint

- 같은 이름을 가진 함수들이 호출될 때마다 중단해준다.
- 같은 이름을 가진 함수가 한 둘이 아닐 때 해당 함수에 대해 모두 적용하고 싶을 경우
- 그 중에서도 특정 모듈이나 클래스 내에 정의된 것만 따로 모아 중단점을 부여하고 싶을 경우

### 5. Runtime Issue Breakpoint

- 백그라운드 스레드에서 UI를 변경하는 것과 같이 런타임시 발생하는 문제이다.
- 앱 충돌만큼 위험하지 않고 실행에 방해가 될 수 있기 때문에 xcode도 프로세스를 일시중지하지 않는다.
- 대신 xcode는 문제를 역추적하여 이를 Issue navigator에 기록한다.
  - issue는 과거에 발생했기 때문에 현재 프로세스 상태를 따로 검사하진 않는다.
- 해당 버그를 잡고 싶을 경우 Runtime Issue Breakpoint를 사용한다.
- 디버거에서 일시중지를 하고 그곳에서 프로세스를 탐색할 수 있다.
  - type popup을 사용해 특정 유형을 선택할 수 있다.

### 6. Constraint Error Breakpoint

- AutoLayout 설정 시 제약끼리 충돌하면 멈춰준다.
- 원래도 런타임에 문제가 생기지 않지만, 로그가 뜬다.
  - 로그가 길고 지저분할 뿐더러 정확히 어떤 UI 요소끼리 충돌한 건지 직관적으로 알기 어렵다.

### 7. Test Failure Breakpoint

- 테스트가 실패하면 멈추는 중단점이다.
- 통합 테스트 등 불가피하게 복잡한 테스트의 경우 유용하다.
- 실패 로그가 뜨기 전에 멈춘다.
  - 테스트가 실패한 순간 테스트 함수 내에서 멈추므로 해당 케이스에 대한 디버깅이 가능하다.


<br/>

## ✏️ LLDB 콘솔에서 유용한 명령어는 어떤 것이 있나요?

---

- `po` : 객체 값 출력
- `print` : 변수 값 출력
- `bt` : 호출 스택 출력
- `expr` : 변수 값 변경 또는 코드 실행
- `thread` : 스레드 정보 출력 및 선택


<br/>

## ✏️ iOS 앱에서 데이터를 저장하는 방법에는 어떤 것들이 있나요?

---

- UserDefaults, Keychain, Core Data, SQLite, CloudKit, File Manager 등이 있다.

### UserDefaults

- 간단한 설정 값이나 사용자 선호도 데이터를 저장한다.
- String, Int, Bool, Array, Dictionary 등 기본 데이터 타입을 저장한다.
- 로그인 상태, 앱 설정(다크 모드 여부), 사용자가 선택한 언어 등 간단한 데이터를 저장한다.
- 앱이 삭제되지 않는 한 데이터를 영구적으로 저장한다.
- 보안이나 데이터 크기에 제한이 있기 때문에 중요한 데이터는 적합하지 않다.

### FileManager

- 앱의 파일 시스템에 데이터를 저장할 수 있게 해주는 클래스이다.
- 파일 형식으로 저장할 수 있는 모든 데이터 타입을 저장한다.
  - 이미지, 문서, 텍스트 파일 등
- 이미지, 동영상, 오디오 파일 등 큰 용량의 미디어 파일을 저장하거나 JSON 파일로 데이터를 저장할 때 사용한다.
- 파일을 직접적으로 다루기 때문에 저장 위치와 파일 관리를 세밀하게 제어할 수 있다. 
- 보통 Document 디렉터리에 데이터를 저장한다.

### Keychain

- 보안이 중요한 데이터를 저장하는 장소로, 암호화된 방식으로 데이터를 저장한다.
- 비밀번호, 토큰, 사용자 인증 정보 등 민감한 정보를 저장한다.
- 보안에 초점이 맞춰져 있으며, 사용자가 앱을 삭제해도 데이터가 유지될 수 있다.


### Core Data

- iOS에서 객체 관계 데이터베이스를 다룰 수 있는 프레임워크이다.
- SQLite를 기반으로 데이터 구조를 쉽게 관리할 수 있다.
- 구조화된 데이터. 예를 들어, 사용자 정보, 앱 내 복잡한 엔티티 관계 등을 저장하기에 적합하다.
- 게시글, 사용자 정보, 복잡한 관계를 가지는 객체 데이터 등에서 사용된다.
- Core Data는 데이터베이스와 유사한 방식으로 데이터를 관리하며, 데이터를 쿼리할 수 있는 기능이 있어 대규모 데이터를 효율적으로 관리할 수 있다.
- 설정이 조금 복잡할 수 있다.

### SQLite

- SQL 데이터베이스 엔진으로 Core Data보다 직접적으로 SQL을 활용해 데이터를 다룰 수 있다.
- 관계형 데이터베이스에 적합한 데이터를 저장한다.
- 데이터를 직접 SQL 쿼리로 관리하고 싶거나, Core Data의 복잡한 설정을 피하고자 할 때 사용한다.
- SQL 구문을 사용해 데이터를 제어할 수 있어 데이터베이스 경험이 있는 경우 적합하지만, 직접 쿼리를 작성해야 하기 때문에 설정과 관리가 다소 복잡할 수 있다.

### CloudKit (iCloud)

- Apple의 iCloud를 통해 데이터를 클라우드에 저장하는 방식이다.
- 사용자 별로 동기화해야 하는 데이터를 저장한다.
  - 노트, 사진, 기록 등
- 사용자의 iCloud 계정을 통해 데이터가 자동으로 동기화되기 때문에, 동일한 Apple ID를 사용하는 기기 간 데이터 일관성을 유지할 수 있다.
- 네트워크 상태에 의존하며, Apple의 CloudKit 설정이 필요하다.


<br/>

## ✏️ UserDefaults의 사용 시 주의할 점은 무엇인가요?

---

- 큰 데이터나 민감한 정보를 저장 금지
  - UserDefaults는 데이터를 빠르게 저장할 수 있지만, 큰 데이터나 이미지, 동영상과 같은 대용량 데이터를 저장하기에는 적합하지 않다. 
  - 또한, 암호화되지 않기 때문에 비밀번호, 인증 토큰과 같은 민감한 정보를 저장하는 것은 보안에 매우 취약하다.
  - 파일로 저장하거나 FileManager, Core Data 등을 사용하는 것이 좋다.
- 데이터 저장의 비동기 처리 지연
  - UserDefaults는 데이터를 즉시 저장하지 않고 일정 시간마다 비동기적으로 저장한다.
  - 따라서 앱을 종료하거나 백그라운드로 이동하기 직전에 저장한 데이터는, 동기화되기 전에 종료되면 저장되지 않을 수 있다.
  - 앱이 종료될 때 확실히 저장하려면 synchronize() 메서드를 호출해 즉시 데이터를 저장할 수 있다.
  - synchronize()는 데이터를 즉시 저장하지만, 성능에 영향을 줄 수 있으므로 꼭 필요한 경우에만 사용해야한다.
- 데이터 용량 제한
  - UserDefaults에는 저장 용량 제한이 있으며, iOS에서는 보통 약 512KB 정도가 권장된다.
  - 데이터가 많아지면 UserDefaults를 사용할 때 성능이 저하될 수 있다.
  - Core Data나 파일 시스템을 사용하는 것이 효율적이다.
- 데이터 타입 제한
  - Int, Double, String, Array, Dictionary, Bool 등의 기본 타입만 저장할 수 있다.
  - 커스텀 객체를 직접 저장할 수 없으므로, 이를 저장하려면 객체를 Data로 변환하거나 Codable 프로토콜을 구현해 JSON 형식으로 저장해야한다.

<br/>

## ✏️ Core Data와 SQLite의 차이점은 무엇이며, 각각 언제 사용하면 좋나요?

---

- Core Data와 SQLite는 둘 다 iOS에서 데이터를 영구적으로 저장할 수 있지만, 사용 목적과 기능적인 차이가 있다. 
- 이 두 가지는 데이터베이스처럼 보이지만, 실제로는 사용 용도에 따라 각각 장단점이 있으며, 저장 방식과 사용 예에 따라 구분하여 선택하는 것이 좋다.
- Core Data: 관계가 복잡한 데이터를 관리할 때, 객체 지향적인 방식으로 데이터를 관리하고 자동화된 최적화가 필요할 때 사용하면 좋다.
  - 소셜 미디어 앱이나 개인 기록 앱에서 사용자와 데이터 간의 복잡한 관계를 관리하는 데 적합하다.
- SQLite: 단순한 관계형 데이터베이스가 필요한 경우, 직접 SQL 쿼리로 데이터를 제어하고 싶을 때 사용하면 좋다.
  - 할 일 목록, 간단한 데이터 저장이 필요한 앱에서 적합하다.

> 일정 관리 앱을 만든다고 가정하면, 유저정보는 CoreData로 할 일 목록은 SQL로 만드는게 효율적인가?<br/><br/>
> 일정 관리 앱에서 유저와 일정 데이터가 서로 관련되어 있다면, 처음부터 Core Data로 모두 관리하는 것이 더 효율적이다. 데이터의 일관성과 관계를 유지하려면 한 가지 저장 방식을 일관되게 사용하는 것이 좋다.
> <br/><br/>
> [ Core Data로 통합하는 것이 더 효율적인 이유 ]<br/>
> - 객체 간의 관계를 쉽게 관리: Core Data는 유저와 일정처럼 엔티티 간의 관계를 설정하고 관리하기에 매우 유리하다. 예를 들어, 유저가 여러 일정을 작성할 수 있는 1대N 관계를 Core Data에서 쉽게 설정할 수 있다.<br/>
> - 데이터 일관성 유지: 한 가지 저장 방식만 사용하면 데이터를 통합적으로 관리할 수 있어 일관성을 유지하기 쉽다. 유저와 일정 정보를 서로 다른 저장소에서 관리하면, 데이터를 동기화하거나 참조하는 데 복잡성이 증가하고, 코드 관리도 어려워질 수 있다.
> - Core Data의 성능 최적화 기능: Core Data는 내부적으로 SQLite를 사용하면서도, 데이터 로딩 최적화, 지연 로딩(Lazy Loading) 등 성능을 최적화하는 기능을 제공한다. 이 기능을 통해 대규모 데이터를 더 효율적으로 관리할 수 있다.

<br/>

> 일정 관리 앱에서 이미지나 영상을 사용해야 하는 경우에는 CoreData를 사용하지 못하니,  FileManager로 대신해야하나?<br/><br/>
> 유저 정보와 일정 데이터를 모두 Core Data로 관리하면서 이미지나 영상 파일만 FileManager로 저장하는 방식이 효율적이다.<br/>
> 이미지 파일을 FileManager에 저장하고, Core Data에 경로를 저장한다.

<br/>

## ✏️ Swift에서 프로토콜(Protocol)이란 무엇이며, 어떻게 활용하나요?

---

- 특정 역할을 수행하기 위한 메소드, 프로퍼티, 기타 요구사항을 선언해두는 추상 인터페이스이다.
  - 이러한 프로토콜을 정의함으로써 클래스, 구조체 또는 열거형에서 일관성 있는 기능을 제공할 수 있다.
- 추상적인 인터페이스로 선언만 허용되며 상속을 받아 구체화시켜 사용한다.
- 클래스, 열거형, 구조체에서 상속(채택)하고 구현할 수 있다.
  - 클래스는 일반적으로 하나의 상위 클래스만 상속할 수 있지만, 프로토콜을 여러 개를 동시에 채택할 수 있다.
  - 이를 통해 클래스나 구조체가 여러 프로토콜에서 요구하는 기능을 모두 구현할 수 있다.
- 프로토콜의 요구 사항을 모두 충족하는 모든 유형(클래스, 열거형, 구조체)은 해당 프로토콜을 준수한다고 한다.
- protocol을 상속 받아도 optional로 선언된 메소드는 클래스에서 선택적으로 구현할 수 있다.
- static 선언이 가능하다.
- 제네릭으로 정의할 수 있다.
  - 동일한 프로토콜을 사용하면서도 다양한 유형의 데이터를 처리할 수 있다.
- Extension을 사용하여 기본 구현을 제공할 수 있다.

<br/>

## ✏️ 프로토콜의 요구사항은 무엇인가요?

---

- 프로토콜은 특정 역할을 수행하기 위한 메서드, 프로퍼티, 이니셜라이저 등의 요구사항을 정의한다.
- 구조체, 클래스, 열거형은 프로토콜을 채택하여 프로토콜의 요구사항을 실제로 구현할 수 있다.

### 프로퍼티 요구사항 (Property Requirements)

- 항상 `var`으로 선언해야 한다.
  - 상속 받는 곳에서 저장, 연산 프로퍼티로 구현 가능한데, 연산 프로퍼티는 반드시 `var`으로 선언해야 하기 때문이다.
- gettable과 settable 프로퍼티는 타입 선언 뒤에 `{ get set }`으로 작성하고, gettable 프로퍼티는 `{ get }`으로 작성한다.
  - get 속성 : 저장 프로퍼티는 `var`, `let` 모두 가능하고 연산 프로퍼티는 `get-only`, `getter setter` 사용 가능
  - get set 속성 : 저장 프로퍼티는 `var`만 가능하고 연산 프로퍼티는 `getter setter` 모두 제공해야 한다.

### 메소드 요구사항 (Method Requirements)

- 함수의 헤더 부분만 작성한다.
- 값 타입(구조체, 열거형)의 경우 `mutating`이 필요하면 프로토콜 자체에 추가한다.
  - 값 타입의 경우는 반드시 mutating을 붙여야 한다.
  - 클래스의 경우 mutating 키워드를 떼고 선언한다.

### 초기화 구문 요구사항 (Initializer Requirements)

- 모든 프로토콜을 채택한 타입에서 초기화 구문을 요구하고자 할 때 특정 초기화를 요구할 수 있다.
- 이 때 프로토콜을 준수하는 클래스에서는 required 키워드를 사용해야한다.

<br/>

## ✏️ 프로토콜 확장(Protocol Extension)을 사용하는 이유는 무엇인가요?

---

- 프로토콜 확장은 프로토콜에 기본 구현을 추가하는 기능으로, 이를 통해 공통 동작을 재사용할 수 있다.
  - 기본 구현을 제공한다.
  - 코드 재사용성이 증가한다.
  - 기능을 확장시킬 수 있다.
- extension을 통해 프로토콜 메소드의 기본 구현을 제공할 수 있다.
- protocol에 선언된 메소드는 기본 값을 가질 수 없다.
  - extension을 통해 기본값을 가지는 메소드를 구현해줄 수 있다.
- `where`를 사용하여 기본 메소드 제공에 제한을 둘 수 있다.
- 연산 프로퍼티도 extension을 통해 기본 값을 제공할 수 있다.
- 프로토콜이 `associatedtype`을 가지고, 파라미터 타입이 해당 제네릭 타입일 경우 기본값을 가질 수 없다.
  - 제네릭을 사용하는 이유는 타입에 대한 제한을 주지 않기 위해서인데, 타입 제한을 주고 싶을 경우 3가지 방법을 사용한다.
  - associatedtype 타입 자체에 제약을 준다.
  - extension에서 제약을 준다.
  - 제네릭 메소드를 사용한다.

<br/>

## ✏️ 프로토콜 지향 프로그래밍(Protocol-Oriented Programming)의 장점은 무엇인가요?

---

- 프로토콜 지향 프로그래밍(POP)은 Swift의 프로토콜과 프로토콜 확장을 활용한 설계 패턴이다.
- 객체 지향 프로그래밍(OOP)에서 클래스 중심의 설계를 대체하거나 보완한다.
- 장점
  - 유연성과 재사용성: 공통 기능을 프로토콜 확장에서 정의하여 다양한 타입에 쉽게 적용한다.
  - 다중 상속 문제 해결: 클래스의 단일 상속 제약을 극복하여 여러 프로토콜을 채택할 수 있다.
  - 테스트 용이성: Mock 객체를 쉽게 만들어 단위 테스트에 유리하다.
  - 추상화 강화: 구체적인 구현이 아닌 동작에 집중하여 의존성을 줄인다.


<br/>

## ✏️ Swift의 접근 제어자(Access Control Levels)에 대해 설명해주세요.

---

- 접근 제어자는 코드 내에서 접근 가능한 범위를 정의하여 모듈성과 코드의 캡슐화를 향상시키는데 사용된다.
- 5가지 접근 수준을 제공한다.

### open

- 모듈 외부에서 접근 및 재정의가 가능하다.
- 클래스 및 클래스 멤버에만 사용 가능하다.
- 해당 클래스를 다른 모듈에서 확장(상속)하고, 메소드를 재정의하도록 허용한다.
- 외부 모듈에서 상속과 오버라이드를 지원해야 하는 경우 사용한다.

### public

- 모듈 외부에서 접근은 가능하지만 재정의와 상속은 불가능하다.
- 클래스, 구조체, 열거형, 프로토콜 및 그 멤버에 사용한다.
- API로 제공하되, 상속 및 재정의는 허용하지 않으려는 경우 사용한다.

### internal (기본 접근 수준)

- 모듈 내부에서만 접근 가능하다.
- 클래스, 구조체, 열거형, 프로토콜 및 그 멤버에 사용한다.
- 모듈 내부에서만 사용되며 외부에 노출되지 않아야 하는 코드에 사용한다.
- 앱 모듈에서만 사용할 목적으로 작성될 때 사용한다.

### fileprivate

- 같은 파일 내에서만 접근 가능하다.
- 클래스, 구조체, 열거형, 프로토콜 및 그 멤버에 사용한다.
- 특정 파일 내에서 코드가 상호작용해야 하지만, 다른 파일에는 노출되지 않아야 하는 경우 사용한다.
- 클래스의 내부 구현이 같은 파일 내의 다른 클래스를 사용할 때 사용한다.

### private

- 같은 선언 내에서만 접근 가능하다.
  - 다른 클래스나 구조체에서도 접근이 불가능하다.
- 클래스, 구조체, 열거형, 프로토콜 및 그 멤버에 사용한다.
- 객체의 가장 엄격한 은닉을 유지하려는 경우 사용한다.
- 클래스 내부에서만 사용되는 보조 메소드나 프로퍼티에 사용한다.

<br/>

## ✏️ open과 public의 차이점은 무엇인가요?

---

### 1. 상속 및 재정의 가능 여부

- open은 외부 모듈에서 <span style="color:#9fb584">**상속과 재정의가 가능**</span>하다.
- public은 외부 모듈에서 <span style="color:#9fb584">**접근만 가능하고 상속과 재정의는 불가능**</span>하다.

### 사용 용도

- open: 주로 <span style="color:#9fb584">**프레임워크나 라이브러리의 API에서 상속과 재정의**</span>를 허용할 때 사용한다.
  - 외부 모듈이 클래스나 메서드를 확장해 커스터마이징할 수 있도록 설계할 때 적합하다.
- public: <span style="color:#9fb584">**상속이 필요 없고, 외부에서 접근만 가능하게 하고 싶을 때**</span> 사용한다.
  - 외부 모듈이 API를 사용하되, 변경을 허용하지 않아야 할 때 사용한다.

### 적용 대상

- open은 <span style="color:#9fb584">**클래스와 클래스 멤버에만 사용**</span>할 수 있다.
- public은 <span style="color:#9fb584">**모든 타입(클래스, 구조체, 열거형 등)과 속성, 메서드, 서브스크립트, 이니셜라이저에 사용**</span>할 수 있다.

<br/>

## ✏️ internal, fileprivate, private의 사용 시기는 어떻게 결정하나요?

---

- internal, fileprivate, private은 코드의 은닉화와 협력 범위를 정의하는 데 사용된다.
- internal : <span style="color:#9fb584">**같은 모듈 내부에서만 사용**</span>되는 경우 (기본 접근 수준)
  - 모듈 내 여러 파일에서 접근이 필요할 때 사용
  - ex) 앱 전체에서 사용할 유틸리티 함수나 데이터 모델
- fileprivate : <span style="color:#9fb584">**같은 파일 내에서 여러 클래스나 구조체 간 협력**</span>이 필요한 경우
  - 같은 파일 내의 다른 타입에서 접근해야 하지만, 파일 외부에서는 접근할 필요가 없는 경우
  - <span style="color:#9fb584">**코드를 파일 단위로 분리하여 캡슐화**</span>할 수 있다.
  - 특정 파일 내의 타입 간에 공유할 필요가 있을 때 사용
  - ex) 클래스와 그 확장이 같은 파일에 있는 경우
- private : <span style="color:#9fb584">**특정 클래스나 구조체 내부에서만 사용되도록 제한**</span>할 경우
  - 같은 스코프(클래스, 구조체, 확장 등) 내에서만 사용해야 하는 속성, 메소드가 있을 때 사용
  - 주로 외부에서 접근하지 못하도록 철저하게 숨기고 싶은 경우, 특정 타입의 내부 구현 세부 사항을 감추고자 할 때 사용

 
<br/>

## ✏️ 접근 제어자를 사용하는 이유는 무엇인가요?

---

- <span style="color:#9fb584">**코드의 안전성과 캡슐화를 강화하고 코드의 가독성과 유지보수성을 높이기 위해서**</span>이다.
- Swift에서 접근 제어자는 코드의 접근 범위를 제한하여 불필요한 외부 접근을 방지하고, 코드가 의도된 대로 사용되도록 돕는 중요한 역할을 한다.
- 캡슐화와 정보 은닉
  - 캡슐화란 객체 지향 프로그래밍의 중요 개념 중 하나로 객체의 속성과 메서드를 외부에서 접근할 수 없도록 보호하여 <span style="color:#9fb584">**내부 구현을 감추는 것**</span>이다.
  - 접근 제어자를 통해 외부에 꼭 필요한 부분만 공개하고, 내부적인 세부 사항은 감춘다.
  - 코드 수정과 외부 접근을 방지한다.
- 코드의 안전성 및 무결성 유지
  - <span style="color:#9fb584">**의도치 않은 접근이나 수정으로 인한 오류를 예방**</span>한다.
- 코드 가독성과 유지보수성 향상
  - 코드의 사용 의도를 명확히 하여, 유지보수가 용이하다.
- 모듈 간 결합도 낮추기
  - 모듈 간 결합도를 줄여, 코드 수정이 다른 모듈에 미치는 영향을 최소화한다.
- API 설계의 명확성
  - 필요한 기능만 외부에 공개하여, 외부 모듈이 의도된 API만 사용할 수 있도록 제한한다.


<br/>

## ✏️ 의존성 관리 도구(CocoaPods, Carthage, Swift Package Manager)의 종류와 차이점은 무엇인가요?

---

- iOS 개발에서 사용하는 의존성 관리 도구로는 `CocoaPods`, `Carthage`, `Swift Package Manager (SPM)`가 있으며, 각 도구는 라이브러리 설치, 버전 관리, 의존성 업데이트를 쉽게 관리할 수 있게 도와준다.
- 설치 방식이나 프로젝트 통합 방식, 지원 기능 등에 차이가 있으며, 프로젝트 요구사항과 팀의 협업 방식에 맞춰 선택할 수 있다.
- CocoaPods: 라이브러리 지원이 가장 많고 의존성 관리가 자동화되어 있어, 많은 라이브러리를 사용하는 프로젝트나 협업 시 CocoaPods가 유리하다.
- Carthage: 프로젝트 설정에 영향을 주지 않고 독립적으로 프레임워크를 관리하고 싶을 때 유용하다. 특히 Xcode 설정을 유지해야 하는 프로젝트나 특정 라이브러리 관리가 필요한 경우 유리하다.
- Swift Package Manager: 최신 Swift 및 Xcode 통합이 필요하고, Apple의 공식 지원을 활용하고 싶을 때 적합하다. Objective-C가 필요하지 않은 Swift 전용 프로젝트에서는 가장 간편하고 최적화된 선택이다.

<br/>

### CocoaPods

- iOS 개발에서 가장 오랫동안 사용된 의존성 관리 도구로, 라이브러리를 쉽게 추가하고 관리할 수 있도록 해준다.
- Ruby로 작성되었으며, iOS와 macOS 프로젝트에서 널리 사용되고 있다.
- 설치 방식 : 라이브러리를 Xcode 프로젝트 파일(.xcworkspace)을 통해 통합하며, 의존성들이 모두 한 번에 설치된다.
- 작동 방식 : Podfile에 의존성을 명시하고 `pod install` 명령어를 통해 의존성을 설치하거나 업데이트할 수 있다.
- 장점
  - 많은 라이브러리가 CocoaPods를 공식 지원하며, 라이브러리 검색과 설치가 용이하다.
  - 라이브러리 의존성 해결이 자동으로 이루어지므로, 여러 라이브러리 간의 충돌을 방지할 수 있다.
  - 사용 빈도가 높은 만큼 관련 자료가 풍부해 초기 학습이 쉽다.
- 단점
  - Xcode 프로젝트 파일을 .xcworkspace로 바꾸고 프로젝트 설정을 CocoaPods 방식에 맞춰야 한다.
  - 의존성 업데이트 속도가 느리고, Xcode 설정이 자동으로 변경되기 때문에 관리가 어려울 수 있다.

### Carthage

- CocoaPods와는 다른 방식으로 Xcode 프로젝트 파일을 직접 수정하지 않고, 독립적인 프레임워크 파일을 빌드하여 연결하는 의존성 관리 도구다. 
- 프로젝트에 덜 침투적이며, 필요할 때만 프레임워크 파일을 수동으로 통합한다.
- 설치 방식 : Carthage는 의존성을 프레임워크 파일로 빌드하고, 빌드된 프레임워크 파일을 Xcode 프로젝트에 직접 연결하여 사용한다.
- 작동 방식 : Cartfile 파일에 의존성을 작성하고, `carthage update` 명령어로 프레임워크를 다운로드하고 빌드한다.
  - 빌드된 파일을 수동으로 프로젝트에 포함한다.
- 장점
  - Xcode 프로젝트 파일을 직접 수정하지 않으므로, 설정이 독립적으로 유지된다.
  - Carthage는 각 프레임워크를 독립적으로 빌드하기 때문에, CocoaPods보다 빌드 시간이 짧을 수 있다.
  - Swift와 Objective-C 모두 지원하며, 각종 Xcode 설정에 영향을 미치지 않아 팀 협업 시 유리하다.
- 단점
  - 의존성 파일을 수동으로 프로젝트에 포함해야 하므로, 설치와 통합 과정이 다소 복잡할 수 있다.
  - 의존성 간 버전 충돌 해결이 자동화되지 않아 수동으로 관리해야 하는 경우가 많다.


### Swift Package Manager

- Apple이 공식적으로 지원하는 의존성 관리 도구로, Swift 언어와 Xcode에 내장되어 있어 가장 간편하게 사용할 수 있는 방식이다.
- Xcode 11 이상부터 통합되어 있으며, Swift와 iOS, macOS 프로젝트에 최적화되어 있다.
- 설치 방식 : Xcode의 File > Add Packages 메뉴에서 Swift Package Manager를 통해 프로젝트에 직접 의존성을 추가할 수 있다.
- 작동 방식 : SPM은 Git 저장소를 기반으로 하는 패키지를 관리하며, 의존성 버전을 자동으로 확인하여 설치한다.
  - Package.swift 파일에서 패키지와 의존성을 관리한다.
- 장점
  - Apple에서 공식 지원하므로 Xcode와 Swift에 최적화되어 있어, Xcode 내에서 설정과 관리가 간편하다.
  - 외부 툴 설치가 필요 없으며, Xcode 프로젝트 파일을 수정하지 않고 통합할 수 있다.
  - Xcode 11 이상을 사용하는 모든 프로젝트에 기본 제공되어 별도의 설치 과정 없이 바로 사용 가능하다.
- 단점
  - Swift와 Objective-C 혼합 프로젝트에서는 완벽한 지원이 어려울 수 있다.
  - 기존 CocoaPods나 Carthage를 사용하는 일부 라이브러리들은 SPM 지원이 부족할 수 있다.


<br/>

## ✏️ 의존성 관리를 통해 얻을 수 있는 이점은 무엇인가요?

---

- 프로젝트의 외부 라이브러리와 프레임워크를 효과적으로 관리하는 방법으로, 이를 통해 코드의 재사용성, 유지보수성, 개발 생산성을 크게 향상시킬 수 있다.
- 효율적인 버전 관리, 협업 환경의 일관성 유지, 라이브러리 업데이트 자동화와 같은 여러 장점을 통해 프로젝트의 전반적인 생산성과 안정성을 높일 수 있다.
- 코드 재사용성 향상
  - 외부 라이브러리를 손쉽게 추가하고 관리함으로써, 이미 검증된 라이브러리를 재사용할 수 있다.
  - 이를 통해 네트워크 요청, 데이터베이스 관리, 이미지 처리 등의 공통 기능을 효율적으로 구현할 수 있어, **기본적인 기능 구현에 드는 시간을 절약**할 수 있다.
- 버전 관리 용이
  - 의존성 관리 도구를 사용하면 **특정 버전을 고정하거나 원하는 버전으로 업데이트**할 수 있어, 라이브러리의 안정성과 호환성을 보장할 수 있다.
  - **프로젝트의 안정성과 유지보수성이 향상**된다.
- 의존성 충돌 방지
  - CocoaPods와 같은 의존성 관리 도구는 **의존성 간의 버전 충돌을 자동으로 해결**하여 프로젝트에서 발생할 수 있는 문제를 최소화한다.
  - 여러 라이브러리를 사용할 때 종속성 문제가 발생할 수 있는데, 의존성 관리 도구는 이를 감지하고 적합한 버전으로 조정하여 **호환성을 유지**한다.
- 프로젝트 구조와 유지보수성 향상
  - 의존성 관리 도구는 외부 라이브러리를 **자동으로 설치, 구성, 관리**하므로, 코드 구조가 일관되게 유지된다.
  - 프로젝트의 **유지보수성이 높아지고, 협업 시 충돌이 줄어드는 효과**를 얻을 수 있다.
- 자동화된 설치 및 업데이트
  - 의존성 관리 도구는 필요한 의존성을 자동으로 다운로드하고 설치해 주며, 업데이트 시에도 명령어 한 줄로 모든 의존성을 최신 버전으로 맞출 수 있다.
  - 초기 설정을 쉽게 할 수 있고, **라이브러리 업데이트에 소요되는 시간을 줄여 개발 생산성을 높일 수 있다.**
- 협업과 팀 생산성 향상
  - 팀 내에서 동일한 의존성을 사용하고 버전을 맞출 수 있으므로, 협업 과정에서 충돌이나 일관성 문제를 줄일 수 있다.
  - 각 개발자가 동일한 버전의 라이브러리를 사용하도록 강제할 수 있어 **팀 전체의 개발 환경이 일관되게 유지**된다.
- 테스트 및 디버깅 편의성
  - 의존성 관리 도구를 통해 업데이트 시 변경 사항을 추적하거나 특정 버전으로 되돌릴 수 있어, **디버깅이 수월하고 오류 발생 시 빠르게 대응**할 수 있다.

<br/>

## ✏️ Git에서 브랜치(Branch)를 사용하는 이유와 장점은 무엇인가요?

---

- 브랜치를 사용하는 이유는 <span style="color:#9fb584">**독립적인 작업 공간을 제공하여 안정적인 개발 환경을 유지하고, 병렬 개발과 협업을 효율적으로 지원**</span>하기 위함이다.
  - 브랜치는 코드 베이스를 여러 가지 버전으로 나눠 병렬로 작업할 수 있게 해준다.
  - 여러 명의 개발자가 각각의 브랜치에서 동시에 작업할 수 있어 개발 속도가 빨라지고, 개발자 간의 충돌을 최소화할 수 있다.
- 브랜치를 통해 작업을 분리하고, 코드 리뷰와 테스트를 거쳐 <span style="color:#9fb584">**코드 품질을 유지하면서 버전 관리와 릴리즈 관리를 유연하게 처리**</span>할 수 있다.
  - 브랜치를 이용해 기능별, 릴리즈별로 코드베이스를 분리할 수 있어 특정 시점의 코드로 되돌아가거나 긴급한 버그 수정을 독립적으로 적용하는 것이 용이하다.
  - 기능 개발이 완료되면 코드 리뷰를 통해 품질을 검토하고 테스트한 후 메인 브랜치에 병합할 수 있어, 프로젝트의 코드 품질을 높일 수 있다.
  - 테스트용 브랜치를 생성하여 새로운 기능이나 버그 수정을 테스트해볼 수 있으며, 테스트 결과에 따라 안전하게 수정 및 업데이트가 가능하다.
- 이러한 장점 덕분에 Git 브랜치는 팀 개발과 대규모 프로젝트에서 필수적인 도구로 사용된다.

<br/>

## ✏️ 브랜치를 병합(Merge)하는 방법에는 어떤 것들이 있나요?

---

- `Fast-Forward Merge`, `Three-Way Merge`, `Rebase` 병합 방식이 있다.
- 각 병합 방식은 <span style="color:#9fb584">**개발 흐름과 충돌 해결 방식에 차이가 있다.**</span>

### Fast-Forward Merge

- <span style="color:#9fb584">**병합 대상 브랜치가 다른 브랜치로부터 분기된 후 변경 사항이 없을 때**</span> 적용되는 병합 방식이다.
- 커밋 이력을 그대로 유지하면서 브랜치를 병합할 수 있다.
- 현재 브랜치를 병합하려는 브랜치의 최신 커밋으로 <span style="color:#9fb584">**포인터를 이동시켜 병합**</span>한다.
- 커밋 이력이 단순하게 유지돼 이력을 추적하기 쉽다.
- 단순히 포인터를 이동하므로 두 브랜치 간 변경 사항이 서로 독립적으로 존재하는 경우 사용할 수 없다.

### Three-Way Merge

- <span style="color:#9fb584">**두 브랜치가 서로 독립적으로 변경된 상태에서 병합**</span>할 때 사용하는 방식이다.
- 공통 조상 커밋을 기준으로 현재 브랜치와 병합 대상 브랜치의 차이점을 비교하고, 새로운 <span style="color:#9fb584">**병합 커밋(Merge Commit)**</span>을 생성한다.
- 두 브랜치에서 독립적으로 작업한 내용을 병합할 수 있어 협업 시 충돌 해결이 용이하다.
- 병합 커밋이 추가되어 커밋 히스토리가 복잡해질 수 있다.

### Rebase

- 한 브랜치의 커밋을 <span style="color:#9fb584">**다른 브랜치의 최신 커밋 뒤에 추가하는 방식으로 병합**</span>하는 방법이다.
- 커밋 히스토리를 재정렬하여 일직선으로 병합되며 커밋 기록이 깔끔하게 유지된다.
- 병합 대상 브랜치의 공통 조상 이후의 커밋들을 새로 추가하여 <span style="color:#9fb584">**커밋 이력을 재배열**</span>한다.
- <span style="color:#9fb584">**커밋 히스토리가 단순**</span>해져 이력을 깔끔하게 유지할 수 있다.
- 기존 커밋을 재정렬하는 방식이기 때문에, 특히 공유된 브랜치에서 Rebase를 사용하면 충돌이 발생하거나 다른 개발자의 커밋 기록에 영향을 줄 수 있다.


<br/>

## ✏️ 브랜치 전략(예: Git Flow, GitHub Flow)에 대해 설명해주세요.

---

- 브랜치 전략은 <span style="color:#9fb584">**효율적인 버전 관리와 협업**</span>을 위해 Git 브랜치를 사용하는 방식과 규칙을 정한 개발 워크플로이다.
- 프로젝트의 개발 흐름에 맞는 브랜치 전략을 사용하면 <span style="color:#9fb584">**코드 품질 관리, 버그 수정, 기능 개발**</span>이 체계적으로 이뤄진다.

### Git Flow

- <span style="color:#9fb584">**배포 주기가 긴 대규모 프로젝트에 적합**</span>한 브랜치 전략이다.
- main 브랜치와 develop 브랜치를 기본으로 여러 브랜치를 분기해 작업한다.
- <span style="color:#9fb584">**기능 개발, 버그 수정, 릴리즈 관리를 체계적으로 분리해 관리**</span>할 수 있어 <span style="color:#9fb584">**여러 기능과 안정적인 릴리즈가 중요한 프로젝트에 자주 사용**</span>된다.
- `main` : 항상 안정적인 상태의 코드만 포함한다.
  - 프로덕션에 배포되는 최종 코드가 저장되는 브랜치이다.
- `develop` : 기능 개발 기본 브랜치로 모든 새로운 기능이 병합되고 테스트되는 브랜치이다.
  - 기능 개발이 완료되면 main 브랜치로 병합하여 배포할 준비를 한다.

#### Git Flow의 보조 브랜치

1. `feature` : 새로운 기능을 개발할 때 사용한다.
  - develop 브랜치에서 분기되어 개발이 완료되면 develop 브랜치로 병합된다.
2. `release` : 배포 전에 테스트하고 버그를 수정하는 브랜치이다.
  - develop 브랜치에서 분기되며, 배포 준비가 완료되면 main과 develop 브랜치에 병합된다.
3. `hotfix` : 프로덕션에서 발생한 긴급한 버그 수정을 위한 브랜치이다.
  - main 브랜치에서 분기되며, 수정 후 main과 develop 브랜치에 병합된다.

<br/>

### GitHub Flow

- <span style="color:#9fb584">**경량화된 브랜치 전략으로 지속적인 배포(CI/CD)가 이뤄지는 소규모 프로젝트나 빠르게 배포가 필요한 프로젝트**</span>에 적합하다.
- Git Flow에 비해 간단한 구조를 가지고 있으며 주로 main 브랜치와 기능별 브랜치만 사용한다.
- `main` : 항상 배포 가능한 상태를 유지한다.
  - 모든 코드 변경 사항은 기능 브랜치를 통해 PR(Pull Request)로 병합되며, 병합 후 즉시 배포할 수 있다.
- `feature` : 기능 개발이 이뤄진다.
  - main 브랜치에서 분기하며 기능 개발이 완료되면 main 브랜치에 PR을 통해 병합된다.

<br/>

## ✏️ 충돌(Conflict)이 발생했을 때 해결 방법은 무엇인가요?

---

- 충돌은 <span style="color:#9fb584">**두 브랜치에서 동일한 파일의 동일한 부분이 서로 다르게 수정된 경우**</span> 발생한다.
- 주로 브랜치 병합, 리베이스 과정에서 발생하며 Git이 자동으로 병합하지 못할 때 수동으로 해결해야 한다.
- 충돌이 발생하면, 코드의 변경 사항을 검토하고 수동으로 수정한 후 다시 병합을 완료할 수 있다.

### 1. 충돌 확인

- 충돌이 발생하면 Git은 충돌된 파일을 표시하고 <<<<<<<, =======, >>>>>>>와 같은 구문을 통해 충돌 영역을 나타낸다.
- <<<<<<< HEAD 부터 ======= 까지가 현재 브랜치의 변경 사항을, ======= 부터 >>>>>>> feature-branch 까지가 병합 대상 브랜치의 변경 사항이다.

```
<<<<<<< HEAD (현재 변경 사항)
This is the change in the current branch.
=======
This is the change in the branch being merged.
>>>>>>> feature-branch (수신 변경 사항)
```

### 2. 충돌된 파일 수정

- 충돌된 파일을 열고 <<<<<<<, =======, >>>>>>> 구문을 기준으로 어떤 변경 사항을 유지할지 선택한다.
- 현재 브랜치의 변경 사항, 병합 대상 브랜치의 변경 사항, 또는 두 브랜치의 내용을 모두 합쳐서 새로운 내용을 작성할 수 있다.

1-2. 수동으로 충돌 해결하기
텍스트 편집기를 사용해 해당 파일을 열고, 충돌을 해결합니다. 충돌이 발생한 부분을 적절하게 수정한 후, <<<<<<<, =======, >>>>>>> 마크를 제거합니다.

### 3. 충돌 해결 표시

- 충돌이 발생한 부분을 적절하게 수정한 후, <<<<<<<, =======, >>>>>>> 마크를 제거한다.
- 충돌된 부분을 수정하고 파일을 저장한 후, 변경 사항을 스테이지에 추가하고 커밋한다.
  - 스테이지에 추가하기 : git add <files>

### 4. 병합 또는 리베이스 완료

- 모든 충돌이 해결되면 병합을 완료하거나 리베이스를 계속 진행할 수 있다.

### 5. 변경 사항 확인

- 병합이 완료된 후, 충돌이 제대로 해결되었는지 다시 한 번 코드를 확인한다.
- 정상적으로 병합되었으면 충돌 해결이 완료된다.

<details>
<summary>git의 작업 영역</summary>
<div markdown="1">

### git의 3가지 작업 영역

- git은 `Working Directory`, `Staging Area`, `Repository` 3가지 영역으로 이뤄진다.
- `Working Directory`
    - 작업 공간으로 아직 git에 기록될 준비가 되지 않은 파일들이 위치한다.
    - 현재 프로젝트 폴더에 존재하는 파일이다.
    - 실제 위치 : 프로젝트 폴더
- `Staging Area`
    - 대기 공간으로 git에 기록될 준비가 된 파일(`commit` 준비가 된 파일)들이 위치한다.
    - git add 명령어를 통해 수정된 파일을 스테이징 영역에 담을 수 있다.
    - 실제로는 하나의 파일 (.git/index)로서 존재한다.
    - 인덱스 파일(.git/index)에는 commit 준비가 된 파일 각각에 대해 파일명과 해당 파일의 내용을 담고 있는 Blob 파일의 주소(이름)가 기록된다.
    - 실제 위치 : 프로젝트 폴더 하위 .git/index 파일
- `Repository`
    - git에 기록될 파일들(`commit` 파일)이 위치한다.
    - 여러 버전들에 해당하는 파일들의 내용이 Blob 파일로 저장되어 있다.
    - 실제 위치 : 프로젝트 폴더 하위 .git/objects/ 폴더

> Staging Area의 유용한 점<br/>
> - 일부분만 커밋할 때 (세밀한 버전 관리가 가능하다)<br/>
>   - Working Directory에서 여러 파일의 코드를 자유롭게 고쳤지만 일정 파일만 커밋하고 싶을 때<br/>
>   - Staging Area가 없다면 커밋될 예정인 내용들을 디스크에 저장해 둘 공간이 없으므로 Working Directory에서 고친 모든 파일을 커밋해야한다.<br/>
> - 충돌을 해결할 때<br/>
>   - 5개의 파일을 머지하고 그 중 2개에서 충돌이 발생할 경우<br/>

### File 관점의 4가지 단계

- git으로 관리되는 파일은 크기 2가지의 상태를 가진다.
- `Untracked`, `Tracked` 상태이다.
- `Untracked` : Working Directory에 있는 파일이지만 Git으로 버전관리를 하지 않는 상태이다.
  - 새롭게 파일들이 생성되면 Untracked 상태가 된다.
  - git add를 해주지 않은 상태이다.
- `Tracked` : 파일이 Git에 의해 변동사항이 추적되고 있는 상태로 3가지 상태로 나뉜다.
  - `Staged` : Staging Area에 반영된 상태이다.
    - git add 명령어로 수행한 파일들이 Staging Area에 추가된다.
    - 새로 생성한 파일/커밋에 포함됐었던 파일의 내용을 수정하고 git add를 한 상태
    - `git restore --staged ${filename}` 명령어를 수행하면 Staging Area에서 제거할 수 있다.
    - 이후 결과는 파일의 이전 상태에 따라 다른데, 새로 추가한 파일이라면 Untracked 상태가 된다.
    - 만약 한번이라도 Commit을 했던 파일이라면 Unmodified 상태가 된다.
  - `Unmodified` : 현재 파일의 내용이 최신 커밋의 모습과 비교했을 때 전혀 바뀐게 없는 상태이다.
    - 해당 파일을 수정하면 Modified 상태가 된다.
    - Staged 상태였던 파일들을 Commit하면 Unmodified 상태가 된다.
  - `Modified` : 최신 커밋의 모습과 비교했을 때 바뀐 내용이 있는 상태이다.
    - 해당 파일은 아직 Staging Area에 올라가지 않은 상태이다.

</div>
</details>

<br/>

## ✏️ Swift의 에러 처리 방법에 대해 설명해주세요.

---

- Swift에서 에러 처리(Error Handling)는 프로그램 실행 중 발생할 수 있는 예외 상황에 대비하여 오류를 처리하고 안정적으로 코드가 실행될 수 있도록 도와준다.
- 명확하고 안전한 에러 처리를 제공하며, `Error 프로토콜`과 `do-catch 구문`, `throws 키워드`, `try 구문`, `defer 구문` 등을 사용해 에러를 처리한다.

### Error 프로토콜

- Swift의 에러 타입은 Error 프로토콜을 준수하는 타입으로 정의된다.
- enum을 사용해 에러의 종류를 구체적으로 정의하고, 상황에 맞게 사용할 수 있다.

``` swift
enum ReservationError : Error {
    case format
    case timeZone
    case timeRange
    case attendance
}
```

### throws 키워드

- throws 키워드는 함수가 에러를 발생시킬 수 있음을 표시하며, 에러를 던질 수 있는 함수를 정의한다.
- 함수 내부에서 에러가 발생할 가능성이 있는 코드를 포함하고, 호출 시 에러 처리를 요구한다.

``` swift
private func separateReservation(_ reservation: String) throws {
    let reservationArr = reservation.split{$0 == "-"}.map{String($0)}
        
    if reservationArr.count < 3 { throw ReservationError.format }

    let timeZone = reservationArr[0]
    guard let attendance = Int(reservationArr[1]) else { throw ReservationError.format}
    guard let conferenceTime = Int(reservationArr[2]) else { throw ReservationError.format}

    try isValid(timeZone, conferenceTime, attendance)
}
```

### try 구문

- try 키워드는 에러가 발생할 수 있는 함수 호출 앞에 붙이며, 호출 시 에러가 발생할 경우 이를 상위 코드로 전달한다.
- 에러 처리 방법에 따라 **try, try?, try!**를 사용할 수 있다.
  - `try` : 에러를 던질 가능성이 있는 코드 앞에 사용하며, do-catch 구문으로 에러를 처리해야 한다.
  - `try?` : 에러가 발생할 경우 nil을 반환하고, 옵셔널로 결과를 처리한다.
  - `try!` : 에러가 발생하지 않을 것을 확신할 때 사용하며, 에러가 발생하면 런타임 오류가 발생한다.

``` swift
do {
    try separateReservation(reservation)
} catch {
    print(error.localizedDescription)
}
```

### do-catch 구문

- do-catch 구문은 에러가 발생할 가능성이 있는 코드를 실행하고, 에러가 발생할 경우 해당 에러를 처리하는 구문이다.
- catch 블록에서 특정 에러를 처리할 수 있으며, 에러의 종류에 따라 분기 처리가 가능하다.


``` swift
func reservationAndPrint(_ reservation: String) {
    do {
        try separateReservation(reservation)
    } catch {
        print(error.localizedDescription)
    }
    printSchedule()
}
```

### defer 구문

- defer 구문은 코드 블록이 종료되기 직전에 실행할 코드를 작성할 때 사용한다.
- 파일 닫기, 리소스 해제와 같은 작업을 수행할 때 유용하다.
- do-catch 구문 내부에서도 defer를 사용하여 에러 발생 여부와 관계없이 마지막에 반드시 실행될 코드를 지정할 수 있다.


``` swift
func readFile() throws {
    let file = openFile()
    defer {
        closeFile(file)
    }
    // 파일 처리 코드
}
```

### 에러 처리 예시

``` swift
enum ReservationError : Error {
    case format
    case timeZone
    case timeRange
    case attendance
}

extension ReservationError: LocalizedError {
    public var errorDescription: String? {
        switch self {
        case .format:
            return NSLocalizedString("\n[예약 정보 형식이 올바르지 않습니다.]", comment: "format error")
        case .timeZone:
            return NSLocalizedString("\n[시간대는 AM / PM 형식으로 입력해주세요.]", comment: "timeZone error")
        case .timeRange:
            return NSLocalizedString("\n[회의 시간은 1시간부터 4시간까지 가능합니다.]", comment: "timeRange error")
        case .attendance:
            return NSLocalizedString("\n[참석 인원은 2명부터 20명까지 가능합니다.]", comment: "attendance error")
        }
    }
}

func reservationAndPrint(_ reservation: String) {
    do {
        try separateReservation(reservation)
    } catch {
        print(error.localizedDescription)
    }
    printSchedule()
}

/// reservation 분리
private func separateReservation(_ reservation: String) throws {
    let reservationArr = reservation.split{$0 == "-"}.map{String($0)}
        
    if reservationArr.count < 3 { throw ReservationError.format }

    let timeZone = reservationArr[0]
    guard let attendance = Int(reservationArr[1]) else { throw ReservationError.format}
    guard let conferenceTime = Int(reservationArr[2]) else { throw ReservationError.format}

    try isValid(timeZone, conferenceTime, attendance)
}


```

<br/>

## ✏️ 옵셔널을 사용한 에러 처리와 do-catch를 사용하는 에러 처리의 차이는 무엇인가요?

---

- 에러의 발생 여부를 어떻게 다루고, 에러에 대해 얼마나 상세히 처리할 수 있는지에서 차이가 난다.
- 옵셔널은 에러 발생 시 nil로 결과를 표현하는 반면, do-catch는 에러를 구체적으로 처리할 수 있는 구조를 제공한다.
- 이 차이는 주로 에러를 간단히 무시할지 또는 에러를 구체적으로 다뤄야 하는지에 따라 선택하게 된다.

### 1. 옵셔널을 사용한 에러 처리 (try?)

- try? 구문을 사용하면, 에러가 발생할 수 있는 함수 호출 결과를 옵셔널 타입으로 반환한다.
- 이 방식은 에러가 발생했을 때 해당 값을 nil로 표현하여, 에러에 대한 구체적인 처리를 생략하고 단순히 값이 없음을 표시하는 데 유용하다.
- 장점: 코드가 간결해지고, 에러 발생 여부를 무시하고 간단하게 nil로 처리할 수 있다.
- 단점: 에러가 발생한 원인을 알 수 없고, 에러에 대한 구체적인 처리가 어렵다.

``` swift
func fetchData() throws -> String {
    throw NSError(domain: "DataError", code: 1, userInfo: nil)
}

let result = try? fetchData()
print(result)  // 출력: nil (에러 발생 시)
```


### 2. do-catch를 사용한 에러 처리

- 에러를 명시적으로 처리하고, 발생한 에러를 분기하여 상세히 다룰 수 있는 구조를 제공한다.
- do 블록 내에서 에러가 발생하면 catch 블록에서 해당 에러를 잡아내며, 에러의 종류에 따라 다양한 대응 방식을 설정할 수 있다.
- 장점: 에러의 원인과 종류에 따라 구체적인 처리가 가능하며, 필요한 대응을 설정할 수 있다.
- 단점: 코드가 다소 길어지지만, 에러 처리가 필요한 상황에서는 명확하고 안정적인 구조를 제공한다.

``` swift
enum NetworkError: Error {
    case noData
    case invalidURL
}

func fetchData() throws -> String {
    throw NetworkError.noData
}

do {
    let result = try fetchData()
    print("Data received: \(result)")
} catch NetworkError.noData {
    print("Error: No data received.")
} catch NetworkError.invalidURL {
    print("Error: Invalid URL.")
} catch {
    print("Unknown error: \(error)")
}
```

<br/>

## ✏️ 에러를 전파하는 방법은 무엇인가요?

---

- 에러 전파(Error Propagation)는 하위 함수에서 발생한 에러를 상위 함수로 전달하여, 상위 함수에서 에러를 처리하도록 하는 방법이다.
- Swift에서는 `throws` 키워드를 통해 에러를 발생시키고 전파할 수 있으며, 호출한 함수 쪽에서 이를 받아 적절히 처리한다.

### 에러 전파 과정

1. throws 키워드로 에러가 발생할 수 있음을 표시
  - 함수 선언에서 throws 키워드를 사용해 해당 함수가 에러를 던질 수 있음을 표시한다.
  - 이 함수는 에러가 발생할 경우 이를 상위로 전파할 수 있다.
2. 호출 시 try 키워드를 통해 에러가 발생할 수 있음을 표시
  - throws 함수는 호출 시 try 키워드를 통해 호출해야 한다.
  - try는 에러가 발생할 수 있음을 나타내며, 호출한 함수로 에러를 전파하거나 처리할 준비를 하게 한다.
3. 상위 함수에서 에러 처리
  - 상위 함수는 do-catch 구문을 사용해 에러를 처리하거나, 또 다시 throws로 에러를 전파할 수 있다.
  - 이처럼 에러를 연쇄적으로 전파하여 최종적으로 한 곳에서 처리하는 방식으로 구현할 수 있다.

``` swift
// 3. 상위 함수에서 에러 처리 (최종적으로 에러 처리)
func reservationAndPrint(_ reservation: String) {
    do {
        try separateReservation(reservation)
    } catch {
        print(error.localizedDescription)
    }
    printSchedule()
}

// 3. 상위 함수에서 에러 처리 (또 다시 throws로 에러 전파)
private func separateReservation(_ reservation: String) throws {
    let reservationArr = reservation.split{$0 == "-"}.map{String($0)}
        
    if reservationArr.count < 3 { throw ReservationError.format }

    let timeZone = reservationArr[0]
    guard let attendance = Int(reservationArr[1]) else { throw ReservationError.format}
    guard let conferenceTime = Int(reservationArr[2]) else { throw ReservationError.format}

    // 2. 호출 시 try 키워드를 통해 에러가 발생할 수 있음을 표시
    try isValid(timeZone, conferenceTime, attendance)
}

// 1. throws 키워드로 에러가 발생할 수 있음을 표시
private func isValid(_ timeZone: String, _ conferenceTime: Int, _ attendance: Int) throws {
    if !isValidTimeZone(timeZone) { throw ReservationError.timeZone }
    if !isValidTimeRange(conferenceTime) { throw ReservationError.timeRange }
    if !isValidAttendance(attendance) { throw ReservationError.attendance }
    
    reserveConferenceRoom(timeZone, conferenceTime, attendance)
}
```

<br/>

## ✏️ UIKit에서 TableView와 CollectionView의 차이점은 무엇인가요?

---

- UITableView와 UICollectionView는 데이터를 표시하기 위한 두 가지 주요 컴포넌트이다.

### 1. 레이아웃 구조

- `UITableView`는 <span style="color:#9fb584">**기본적으로 단일 열로 구성되며, 세로 스크롤만 가능**</span>하다.
  - 즉, 데이터를 단순히 세로로 나열할 때 적합하다.
- `UICollectionView`는 <span style="color:#9fb584">**그리드 형식을 지원하여 다중 열과 행을 구성**</span>할 수 있다.
  - <span style="color:#9fb584">**가로 및 세로 스크롤을 모두 지원**</span>하기 때문에 더 복잡하고 유연한 레이아웃을 표현하는 데 적합하다.

### 2. 데이터 구조 및 표현 방식

- `UITableView`는 <span style="color:#9fb584">**섹션과 행으로 구성**</span>된다.
  - 각 섹션 안에 여러 개의 행이 배치되는 구조이며, 특정 행을 선택하는 didSelectRowAt 메서드를 통해 상호작용을 처리할 수 있다.
- `UICollectionView`는 <span style="color:#9fb584">**섹션과 아이템으로 구성**</span>되며, 행과 열로 이루어진 다양한 레이아웃이 가능하다.
  - 유연한 레이아웃을 구현할 수 있기 때문에 이미지 그리드, 카테고리 리스트 등 다양한 데이터 시각화에 적합하다.


### 3. 레이아웃 커스터마이징

- `UITableView`는 <span style="color:#9fb584">**레이아웃이 고정**</span>되어 있고, 크게 커스터마이징할 필요 없이 표 형태의 데이터를 쉽게 표시할 수 있다.
- `UICollectionView`는 UICollectionViewLayout 및 UICollectionViewFlowLayout을 사용해 <span style="color:#9fb584">**레이아웃을 유연하게 변경**</span>할 수 있어, 복잡한 그리드 및 맞춤형 레이아웃을 구현할 수 있다.

### 4. 성능

- `UITableView`는 단순한 스크롤링 데이터 구조를 가질 때 성능이 좋다.
  - 기본적인 목록 형태로 많은 양의 데이터를 효율적으로 처리할 수 있다.
- `UICollectionView`는 더 많은 데이터나 복잡한 레이아웃을 처리하기 위한 다양한 옵션을 제공하지만, 최적화가 필수다.
  - 특히 그리드 형식의 레이아웃은 많은 셀을 표시할 때 성능에 영향을 줄 수 있으므로 적절한 메모리 관리가 필요하다.

### 5. 사용 예시

- `UITableView`는 메시지 목록, 설정 화면, 이메일 목록 등 단일 열의 단순한 목록 데이터를 표시할 때 자주 사용된다.
- `UICollectionView`는 이미지 갤러리, 상품 목록, 카테고리 선택 화면 등 다양한 형식의 데이터를 표시할 때 유용하다.


<br/>

## ✏️ 셀(Cell)의 재사용(Reusability)은 어떻게 구현되나요?

---

- 셀을 재사용하는 원리
  - UITableView와 UICollectionView는 모두 재사용 큐(reuse queue)를 활용해 스크롤할 때 화면에서 벗어난 셀을 재사용 큐에 저장하고, 새로운 데이터로 재사용한다.
    - 화면에 나타나는 셀만 메모리에 두고 셀 틀을 재사용하여 내용만 바꾼다.
  - 이 방식을 통해 메모리 사용량을 줄이고, 특히 많은 데이터를 스크롤할 때 새로운 셀을 생성하지 않아 성능을 높일 수 있다.
- 셀에 구성되는 데이터 소스의 내용은 다르지만 셀 자체는 재사용되기 때문에 셀의 UI 또는 Attribute들이 중첩되거나 반복되는 문제가 발생한다.
  - 이미지, 텍스트가 중첩되는 경우 : prepareForReuse()를 통해 해결한다.
    - 잔상이 남는 UI를 nil로 처리한다.

``` swift
override func prepareForReuse() {
      super.prepareForReuse()
      self.imgView.image = nil
}
```

### Cell Life Cycle

#### 초기 Cell

1. 초기화 구문 호출
  - `override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?)` : 셀의 초기화 작업을 한다.
  - `awakeFromNib()` : UIView를 상속받는 Custom View Class를 만들고 Interface Builder에 지정한 경우
    - UIView를 상속받는 Custom View Class를 만들고 Interface Builder에 지정하면 Custom Class는 UINib에 Archive 되어있다가 필요할 때 바로 UnArchive한다.
    - 이렇게 하면 앱이 nib 파일의 내용을 인스턴스화할 때 nib file을 로드하는 시간을 줄여 성능이 향상된다.
    - awakeFromNib()은 이런식으로 nib file이 로드된 이후 호출되는 메서드다.
      - init 메서드를 사용하여 초기화하여 모든 객체가 인스턴스화 된 후, 모든 객체에 대한  outlet과 action 연결을 다시 설정한다.
      - 그 다음 awakeFromNib 메서드가 호출된다.
    - UIView나 UITableViewCell 등에서는 awakeFromNib()을 override하여 그 안에서 IBOutlet 객체에 대한 초기화를 진행한다.
    > 인터페이스 빌더(Interface Builder)는 iOS 개발에 사용되는 시각적인 툴로, 개발자가 유저 인터페이스를 직관적으로 디자인할 수 있게 해주는 툴이다.
2. cellForRowAt 으로 셀 만듬
  - `tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath)` : UITableView의 특정 위치에 넣을 Cell을 DataSource에 요청한다.
3.  willDisplay 실행
    - `tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath)` : 테이블 뷰가 셀을 사용하여 행을 그리기 직전에 호출된다.

#### 스크롤 이후

1. 스크롤 이후에는 초기화 메소드가 호출되지 않고 `prepareForReuse()` 호출
  - `prepareForReuse()` : 테이블 뷰 Delegate에 의해 재사용이 가능한 셀을 준비한다. 
    - 셀이 재사용 된다면 `dequeueReusableCell` 함수가 리턴되기 전 호출된다.
2. `tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath)`
3. `tableView(_ tableView: UITableView, didEndDisplaying cell: UITableViewCell, forRowAt indexPath: IndexPath)` : 테이블 뷰로부터 셀이 화면에 사라지면 호출된다.
4. `tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath)` : 

### 구현

- `dequeueReusableCell(withIdentifier:)` : 재사용 큐에 대기하고 있는 셀을 꺼낼 때 호출한다.
  - 재사용 큐에 대기하고 있는 셀 인스턴스가 있으면 제공하고 없으면 nil을 반환한다.
  - identifier의 역할은 원하는 셀을 꺼내올 수 있도록 하는 재사용 식별자이다.
  - 원하는 객체를 가르키는 값을 인자 값으로 넣어줘야 한다.
 
``` swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    guard let cell = tableView.dequeueReusableCell(withIdentifier: "AbandonedNoticeTableViewCell", for: indexPath) as? AbandonedNoticeTableViewCell
    else {fatalError("no matched TableViewCell identifier")}
    
    let notice = self.abandonedM.abandonedNoticeList[indexPath.row]
    
    // 셀 설정
    
    return cell
}
```

<br/>

## ✏️ 동적인 셀 높이(Dynamic Cell Height)를 설정하는 방법은 무엇인가요?

---

### 1. UITableView에서 동적 셀 높이 설정

- 셀 내용에 따라 셀 높이를 자동으로 조정할 수 있다.
  - 셀 내부에 표시할 콘텐츠에 오토 레이아웃을 설정해야 한다.
  - 모든 뷰의 상하좌우 제약 조건이 명확하게 설정되어야 자동으로 셀 높이가 조정된다.
  - 예를 들어, UILabel이나 UIImageView가 있는 경우 상하 제약 조건을 설정하면, 내용에 맞춰 셀 높이가 조정된다.
  - 셀 높이가 자동으로 조정되므로 tableView(_:heightForRowAt:) 메서드를 구현할 필요가 없다.
  - 오토 레이아웃이 셀의 높이를 자동으로 계산한다.


1. UITableView의 rowHeight를 UITableView.automaticDimension으로 설정한다.
2. estimatedRowHeight를 설정하여 높이 추정값을 설정한다.


``` swift
tableView.rowHeight = UITableView.automaticDimension
tableView.estimatedRowHeight = 100 // 대략적인 높이 추정값
```

### 2. UICollectionView에서 동적 셀 높이 설정

- 셀의 높이를 설정하기 위해 UICollectionViewFlowLayout 또는 커스텀 레이아웃을 사용해야 한다.
- 각 셀의 내용에 따라 셀 높이를 동적으로 조정할 수 있도록 코드를 작성한다.
- UICollectionViewDelegateFlowLayout 프로토콜의 collectionView(_:layout:sizeForItemAt:) 메서드를 사용하여 셀 크기를 동적으로 설정할 수 있다. 
  - 각 셀에 필요한 콘텐츠의 높이를 계산하고, 해당 크기를 반환한다.
  - 셀 내부에 있는 모든 요소의 제약 조건을 설정해줘야 한다.
  - 예를 들어, UILabel이 셀 높이에 맞게 늘어나도록 상하좌우 제약을 설정해야 한다. 
  - 이렇게 하면 셀의 높이가 자동으로 계산된 크기에 맞춰진다.


``` swift
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
    // 예: UILabel의 텍스트 길이에 따라 셀 높이를 조정
    let width = collectionView.frame.width
    let dummyLabel = UILabel()
    dummyLabel.text = data[indexPath.row]
    dummyLabel.numberOfLines = 0
    dummyLabel.lineBreakMode = .byWordWrapping
    let targetSize = CGSize(width: width, height: UIView.layoutFittingCompressedSize.height)
    let estimatedSize = dummyLabel.systemLayoutSizeFitting(targetSize)
    return CGSize(width: width, height: estimatedSize.height)
}
```

### 셀 높이를 설정할 때 고려사항

- 성능 최적화: 높이 계산이 빈번히 발생하는 경우, 성능에 영향을 줄 수 있다.
  - estimatedRowHeight를 설정하여 UITableView가 스크롤할 때 추정 높이를 먼저 사용하게 하여 성능을 높일 수 있다.
- 캐싱 사용: 셀 높이를 미리 계산해 캐싱해 두면 스크롤 성능이 향상된다.


<br/>

## ✏️ CollectionView의 레이아웃을 커스터마이징하는 방법은 무엇인가요?

---

### 1. UICollectionViewFlowLayout 수정

- UICollectionViewFlowLayout을 사용하여 레이아웃을 커스터마이징할 수 있다.
- UICollectionViewFlowLayout은 기본적으로 그리드나 리스트 레이아웃을 제공하므로 이를 기반으로 크기나 간격을 조정할 수 있다.
- itemSize: 각 셀의 크기를 설정한다.
- minimumInteritemSpacing: 셀 간의 가로 간격을 설정한다.
minimumLineSpacing: 셀 간의 세로 간격을 설정한다.
- scrollDirection: 스크롤 방향을 설정한다. (가로: .horizontal, 세로: .vertical)
- sectionInset: 각 섹션의 여백을 설정한다.


``` swift
let layout = UICollectionViewFlowLayout()
layout.itemSize = CGSize(width: 100, height: 100)
layout.minimumInteritemSpacing = 10
layout.minimumLineSpacing = 20
layout.scrollDirection = .vertical
layout.sectionInset = UIEdgeInsets(top: 10, left: 10, bottom: 10, right: 10)
collectionView.collectionViewLayout = layout
```

### 2. Delegate 사용

- UICollectionViewDelegateFlowLayout 프로토콜을 채택해 각 셀의 크기나 간격을 동적으로 설정할 수 있다.

``` swift
extension ViewController: UICollectionViewDelegateFlowLayout {
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        return CGSize(width: 100, height: 100)
    }

    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumLineSpacingForSectionAt section: Int) -> CGFloat {
        return 20
    }

    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumInteritemSpacingForSectionAt section: Int) -> CGFloat {
        return 10
    }
}
```

### 3. UICollectionViewCompositionalLayout 사용 (iOS 13 이상)

- 더 복잡한 레이아웃을 만들 수 있다.
- 다양한 섹션 레이아웃을 손쉽게 정의할 수 있다.
- NSCollectionLayoutItem: 단일 아이템(셀)의 크기를 정의한다.
- NSCollectionLayoutGroup: 여러 아이템을 그룹으로 묶어 배치할 수 있다.
- NSCollectionLayoutSection: 그룹을 사용해 섹션을 정의하며, 섹션마다 다른 레이아웃을 지정할 수 있다.
- NSCollectionLayoutSize: 아이템과 그룹의 크기를 설정합니다. 상대적(비율) 또는 절대적(고정값) 크기를 지정할 수 있다.

### 4. UICollectionViewLayout 서브클래싱

- 더 복잡하고 유연한 레이아웃이 필요할 때는 UICollectionViewLayout을 서브클래싱하여 직접 커스터마이징할 수 있다.
- 많은 코드가 필요하지만, 가장 높은 수준의 커스터마이징이 가능하다.
- prepare(): 레이아웃 속성을 설정하는 초기화 메서드입니다. 레이아웃에 필요한 데이터나 계산을 준비한다.
- layoutAttributesForElements(in:): 주어진 영역에 표시될 아이템들의 레이아웃 속성을 반환한다.
- layoutAttributesForItem(at:): 특정 아이템의 레이아웃 속성을 반환한다.
- shouldInvalidateLayout(forBoundsChange:): 레이아웃을 갱신할 조건을 지정한다.


<br/>

## ✏️ ARC(Automatic Reference Counting)의 동작 원리는 무엇인가요?

---

- Swift와 Objective-C에서 객체의 메모리를 관리하는 시스템이다.
- ARC는 객체가 더 이상 필요하지 않을 때 자동으로 메모리를 해제하여 메모리 누수를 방지한다.
- 수동으로 메모리를 해제해야 하는 번거로움을 줄여준다.
- ARC의 동작 원리는 객체의 `참조 카운트 Reference Count`를 기반으로 한다.
- ARC는 객체에 대한 <span style="color:#9fb584">**강한 참조**</span>를 추적하여 객체가 얼마나 참조되고 있는지 관리한다.
- 각 객체는 참조될 때마다 참조 카운트가 증가하고, 참조가 해제되면 참조 카운트가 감소한다.
- 참조 카운트가 0이 되면 객체가 더 이상 필요하지 않다고 판단하여 메모리에서 해제한다.
- 대부분의 경우 객체는 **강한 참조**를 통해 다른 객체를 참조하고, 이때 참조 카운트가 증가한다.

### 동작 과정 예시

- 객체 생성 시 참조 카운트는 1로 증가한다.
- 다른 객체가 강한 참조를 통해 이를 참조하면 참조 카운트가 증가한다.
- 강한 참조가 해제되면 참조 카운트가 감소한다.
- 참조 카운트가 0이 되면, deinit 메소드가 호출되고 객체가 메모리에서 해제된다.


``` swift
class Example {
    var data: String
    init(data: String) {
        self.data = data
        print("\(data) initialized")
    }
    deinit {
        print("\(data) is being deinitialized")
    }
}

var example: Example? = Example(data: "Sample Data") // 참조 카운트 1
example = nil // 참조 카운트 0, 메모리 해제 및 deinit 호출
```

<br/>

## ✏️ deinit 메서드는 언제 호출되며, 어떤 역할을 하나요?

---

- deinit 메서드는 객체가 메모리에서 해제될 때 호출되는 소멸자이다.
- Swift에서는 객체가 해제되기 직전 deinit 메소드가 자동으로 호출된다.
- deinit 메서드는 오버라이드 없이 자동으로 호출되며, 파라미터나 반환값을 가질 수 없다.
- 리소스 해제 : 파일 핸들, 네트워크 연결, 데이터베이스 연결과 같은 리소스를 해제하는 데 사용된다.
- Observer 제거 : NotificationCenter나 KVO에서 추가된 옵저버를 제거하여 메모리 누수를 방지한다.
- 디버깅 : 객체의 해제 여부를 확인하는 로그를 추가하여 retain cycle이 발생하는지 확인하는 데 유용하다.

<br/>

## ✏️ 상속(Inheritance)과 프로토콜(Protocol)의 차이점은 무엇인가요?

---

- 상속과 프로토콜은 Swift에서 객체지향 프로그래밍 및 프로토콜 지향 프로그래밍을 구현하기 위한 두 가지 주요 개념이다.
- 상속은 <span style="color:#9fb584">**코드 재사용이 필요한 경우와 기본적인 동작을 자식 클래스에서 확장할 때 유용**</span>하다.
- 프로토콜은 <span style="color:#9fb584">**클래스뿐만 아니라 구조체와 열거형에서도 사용할 수 있어, 특정 기능을 여러 타입에 걸쳐 공통적으로 구현해야 할 때 적합**</span>하다.
- Swift에서는 상속과 프로토콜을 함께 사용할 수도 있다. 
- 클래스 상속을 통해 공통 동작을 물려받으면서, 프로토콜을 채택해 추가적인 기능 요구 사항을 정의할 수 있다.

### 상속

- 상속은 클래스가 다른 클래스의 속성과 메소드를 물려받는 기능이다.
- Swift에서는 <span style="color:#9fb584">**단일 상속만 지원**</span>되며 하나의 클래스는 하나의 부모 클래스만 가질 수 있다.
- <span style="color:#9fb584">**상속을 통해 코드 재사용이 가능하며, 부모 클래스의 기능을 확장하거나 수정하기 위해 사용**</span>한다.
- 부모 클래스에 있는 기본 속성이나 메소드를 자식 클래스에서 사용하거나 <span style="color:#9fb584">**오버라이딩하여 부모 클래스의 메서드, 프로퍼티, 서브스크립트를 재정의할 수 있다.**</span>
- 상속은 클래스에만 적용되며 구조체, 열거형에 사용할 수 없다.

<details>
<summary>오버라이딩💡</summary>
<div markdown="1">

- 서브 클래스는 슈퍼 클래스에서 상속할 인스턴스 메서드, 타입 메서드, 인스턴스 프로퍼티, 타입 프로퍼티, 서브스크립트를 구현할 수 있는데, 이것을 `오버라이딩(overriding)`이라 한다.
- `override` 키워드 없이 오버라이딩 할 경우 컴파일 에러가 난다.
- `override` 키워드가 있을 경우, 컴파일러는 슈퍼 클래스에 오버라이딩 선언한 것과 일치한 정의가 있는지 확인 한다.
  - 슈퍼 클래스에 정의가 없을 경우 에러가 발생한다.
  - Method does not override any method from its superclass
 

### 메소드 오버라이딩

- 상속받은 인스턴스&타입 메서드를 오버라이딩(재정의) 하여, 하위 클래스 내에서 해당 메서드를 원하는 대로 구현을 할 수 있다.
- `override` 키워드를 붙여 슈퍼클래스의 메소드를 재정의 할 수 있다.
- 오버라이딩을 할 경우, 슈퍼 클래스의 메소드를 재정의한 것이기 때문에, 슈퍼 클래스의 메소드는 실행되지 않는다.
  - 슈퍼 클래스의 메소드를 같이 실행하고 싶을 경우 `super`를 사용한다.
  - super를 부르는 순서는 상관 없다.

``` swift
class Human {
    func description() {
        print("나는 사람입니다")
    }
}

class Teacher: Human {
}

let gyeomji: Teacher = .init()

// Teacher클래스가 Human 클래스를 상속받기 때문에, Human 멤버들에 모두 접근 가능
gyeomji.description()
// 나는 사람입니다

// description 오버라이딩(재정의)
class Teacher: Human {
    override func description() {
        print("나는 선생입니다")
    }
}

gyeomji.description()
// 나는 선생입니다

// super 사용
class Teacher: Human {
    override func description() {
        super.description()
        print("나는 선생입니다")
    }
}

gyeomji.description()
// 나는 사람입니다
// 나는 선생입니다
```

### 프로퍼티 오버라이딩

- 상속받은 프로퍼티를 오버라이딩하여 해당 속성에 대한 getter, setter를 제공하거나 상속받은 프로퍼티 값의 변경을 추적할 수 있도록 프로퍼티 옵저버를 추가할 수 있다.
- 저장 프로퍼티 : 저장 프로퍼티에 저장 속성을 추가하는 오버라이딩은 불가능하다.
    - getter, setter를 모두 구현해주면 오버라이딩이 가능하다.

``` swift
class Human {
    var name = "gyeomji"
}

// 저장 프로퍼티에 저장 속성을 추가하는 오버라이딩은 불가능하다.
class Teacher: Human {
    override var name: String = "gyeomjiYoon"       // Cannot override with a stored property 'name'
}


// getter, setter를 모두 구현해주면 오버라이딩이 가능하다.
class Teacher: Human {
    var alias = "gigi"
 
    override var name: String {   
        get {
            return self.alias
        }
        set {
            self.alias = newValue
        }
    }
}

let teacher = Teacher()
print(teacher.name)  // gigi
```

- 연산 프로퍼티
  - 부모 클래스에 연산 프로퍼티가 getter로만 구현된 경우 : getter만 구현 / setter 추가 구현 오버라이딩 가능
  - getter, setter 로 구현된 경우 : getter 만 구현 ❌ / getter, setter 구현 오버라이딩 가능

``` swift
// 부모 클래스에 연산 프로퍼티가 getter로만 구현된 경우
class Human {
    var name = "gyeomji"
 
    var alias: String {
        return self.name + " 바보"
    }
}

// getter만 구현 가능
class Teacher: Human {
    override var alias: String {
        return self.name + " 멍청이"
    }
}

// setter 추가 구현 가능
class Teacher: Human {
    override var alias: String {
        get {
            return self.name + " 멍청이"
        }
        set {
            self.name = newValue
        }
    }
}

// 부모 클래스에 연산 프로퍼티가 getter, setter 로 구현된 경우
class Human {
    var name = "gyeomji"
 
    var alias: String {
        get {
            return self.name + " 바보"
        }
        set {
            self.name = newValue
        }
    }
}

// getter/setter 구현 가능
class Teacher: Human {
    override var alias: String {
        get {
            return self.name + " 멍청이"
        }
        set {
            self.name = newValue
        }
    }
}
```


### 프로퍼티 옵저버 추가

- 저장 프로퍼티의 경우, var로 선언된 프로퍼티만 오버라이딩으로 옵저버를 추가할 수 있다.
  - 프로퍼티 옵저버의 경우 값이 변경될 때를 알려주는 것인데 상수면 값이 바뀔 일이 없다.
  - 슈퍼 클래스의 프로퍼티기 타입 추론에 의해 타입이 명시 되어 있지 않다고 해도 프로퍼티를 오버라이딩 할 경우엔 타입을 반드시 명시해야 한다.
- 연산 프로퍼티의 경우, getter / setter가 모두 구현된 경우만 오버라이딩으로 옵저버를 추가할 수 있다.
  - getter만 구현된 경우 setter(값 변경)이 호출될 일이 없다.

``` swift
// 저장 프로퍼티의 경우, var로 선언된 프로퍼티만
class Human {
    var name = "gyeomji"
}
 
class Teacher: Human {
  // 타입이 반드시 명시되어야 함
    override var name: String {
        willSet {
            print("name willSet \(newValue)")
        }
        didSet {
            print("name didSet \(oldValue)")
        }
    }
}

let teacher = Teacher()
teacher.name = "unknown"
// name willSet unknown
// name didSet gyeomji


// 연산 프로퍼티의 경우, getter / setter가 모두 구현된 경우만
class Human {
    var name = "gyeomji"
 
    var alias: String {
        get {
            return name + " 바보"
        }
        set {
            self.name = newValue
        }
    }
}
 
class Teacher: Human {
    override var alias: String {
        willSet {
            print("Computed Property willSet")
        }
        didSet {
            print("Computed Property didSet")
        }
    }
}

let teacher = Teacher()
teacher.alias = "unknown"
// Computed Property willSet
// Computed Property didSet
```


### final 오버라이딩 금지

- 오버라이딩이 가능한 프로퍼티, 메서드, 서브스크립트 등에 final을 붙이면, 해당 정의는 더이상 오버라이딩이 불가하다.
- 오버라이딩이 금지된 것이지, 서브 클래스에서 접근 자체가 금지된 것은 아니다.

</div>
</details>

### 프로토콜

- <span style="color:#9fb584">**특정 기능이나 속성의 요구 사항을 정의하는 틀**</span>으로, 실제 구현을 포함하지 않고, 요구사항을 선언 하는 추상 인터페이스이다.
- 클래스, 구조체, 열거형은 프로토콜을 채택하고 구현할 수 있다.
- 여러 타입이 공통적으로 구현해야 하는 기능을 정의하는데 사용된다.
- <span style="color:#9fb584">**다중 프로토콜 채택이 가능**</span>하므로, 클래스나 구조체, 열거형이 여러 프로토콜을 채택하여 다양한 기능을 구현할 수 있다.
- <span style="color:#9fb584">**프로토콜 익스텐션을 사용해 모든 프로토콜을 채택한 타입에 기본 구현을 제공할 수 있다.**</span>

### 차이점 비교


|특성|상속|프로토콜|
|:------|:------:|:------:|
|주요 사용 대상|클래스|클래스, 구조체, 열거형|
|상속 관계|단일 상속|다중 프로토콜 채택|
|목적|코드 재사용 및 클래스 계층 구조 정의|특정 기능 요구 사항 정의, 다형성 구현|
|오버라이딩|오버라이딩 가능|프로토콜 자체에 구현이 없고 채택한 타입이 구현|
|확장성|클래스 상속 체계 내에서말 확장 가능|프로토콜 익스텐션을 통해 모든 타입에 확장 가능|
|추상성|구체적인 구현 포함 가능|요구사항만 정의, 구현은 타입에서 수행|


<br/>

## ✏️ 클래스 상속을 사용할 때의 장단점은 무엇인가요?

---

### 장점

- 코드 재사용
  - 부모 클래스의 속성과 메서드를 자식 클래스에서 그대로 상속받을 수 있어 코드 중복을 줄이고, 코드 재사용성을 높인다.
  - 공통 기능을 부모 클래스에 구현함으로써 여러 자식 클래스에서 공유할 수 있다.
- 확장성
  - 상속을 통해 기존 클래스에 기능을 추가하거나, 기존 메서드를 오버라이드하여 특정 동작을 변경할 수 있어, 유지 보수와 확장이 용이하다.
  - 자식 클래스에서 부모 클래스의 기본 동작을 활용하면서 필요한 기능을 추가할 수 있다.
- 계층 구조 형성
  - 상속을 사용하면 객체 계층 구조를 정의할 수 있다.
  - 이 계층 구조는 개념적으로 연관된 클래스 간의 관계를 명확히 하여, 프로그램의 구조를 이해하기 쉽게 만든다.
  - 상위 클래스와 하위 클래스 간의 계층을 통해 특정 유형의 객체를 그룹화하고 관리하기 쉽다.
- 다형성(Polymorphism) 활용
  - 부모 클래스 타입의 참조를 통해 자식 클래스의 인스턴스를 다룰 수 있어, 다형성을 활용한 유연한 코드를 작성할 수 있다.
  - 이를 통해 자식 클래스의 고유 기능을 통해 동일한 부모 클래스 타입을 확장할 수 있다.

### 단점
 
- 강한 결합(Coupling)
  - 상속은 자식 클래스가 부모 클래스에 의존하게 만들어, 부모 클래스의 변경이 자식 클래스에 영향을 미칠 수 있다.
- 복잡한 계층 구조
  - 상속 계층이 깊어질수록 클래스 구조가 복잡해져서 가독성과 관리가 어려워진다. - 또한, 다수의 상속 계층이 있는 경우, 클래스 간 관계를 파악하기가 까다로울 수 있다.
- 다중 상속 제한
- 유연성 부족
  - 자식 클래스에서 불필요한 부모 클래스의 기능까지 물려받는 경우도 발생할 수 있다.
- 캡슐화 문제:
  - 상속을 사용하면 부모 클래스의 내부 구현이 자식 클래스에 노출될 수 있어 캡슐화가 깨질 수 있다.

- 클래스 간 명확한 'is-a' 관계가 있을 때 적합하다.
  - Bird는 Animal의 한 종류이므로 Bird 클래스가 Animal 클래스를 상속하는 것은 적절하다.
- 그러나 여러 기능을 조합하여 객체를 구성해야 하는 경우, 상속 대신 프로토콜이나 컴포지션(구성)을 사용하는 것이 더 유리할 수 있다.
  - Flyable 프로토콜을 정의하고 여러 클래스가 채택하여 fly() 메서드를 구현하게 하면 상속을 통해 깊은 계층을 형성하지 않고도 객체에 특정 기능을 부여할 수 있다.


<br/>

## ✏️ 다중 상속(Multiple Inheritance)이 불가능한 이유는 무엇인가요?

---

- 다이아몬드 문제
  - 다중 상속을 허용하면 동일한 상위 클래스가 두 개 이상의 경로로 상속되는 상황이 발생할 수 있다.
  - 이 경우 D 클래스가 A 클래스의 어떤 인스턴스를 상속받을지 명확하지 않아, A 클래스의 속성과 메서드가 중복되거나 모호해진다.

```
    A
   / \
  B   C
   \ /
    D
```

- 모호성 문제
  - 다중 상속을 통해 부모 클래스들이 동일한 이름의 메소드나 속성을 정의할 때, 자식 클래스에서 어떤 부모 클래스의 메소드, 속성을 사용할지 모호할 수 있다.
- 복잡한 상속 구조
  - 다수의 부모 클래스를 상속받는 클래스는 여러 부모 클래스에서 가져온 메서드와 속성을 모두 포함하므로, 클래스 간 관계가 복잡해지고 코드의 이해가 어렵다.
  - 상속 구조가 복잡해지면 클래스 간의 결합도가 높아져 유지보수와 확장성이 떨어질 수 있다.
- 객체 지향 원칙 위반 가능성
  - 객체 지향 프로그래밍의 원칙 중 하나는 "상속보다는 구성(Composition over Inheritance)"을 사용하는 것이다.
  - 상속은 강한 결합을 만들기 때문에, 다중 상속을 허용할 경우 결합도가 더 높아지고, 객체 지향 설계의 유연성을 해칠 수 있다.
  - Swift는 상속보다 프로토콜을 통한 다형성을 지원함으로써 이러한 결합도를 낮추고, 더 유연한 설계를 유도한다.

<br/>

## ✏️ 프로토콜 준수(Conformance)를 통해 다형성을 구현하는 방법은 무엇인가요?

---

### 1. 프로토콜 정의

- 여러 타입이 공통적으로 준수해야 할 요구사항을 정의하는 프로토콜을 만든다.
- 해당 프로토콜을 채택하는 모든 타입은 프로토콜의 요구사항을 반드시 구현해야 한다.

``` swift
protocol Animal {
    func makeSound()
}
```

### 2. 다양한 타입이 프로토콜을 준수하도록 구현

- 프로토콜을 준수하는 여러 타입을 정의한다.
- 각각의 타입은 프로토콜의 요구사항을 다르게 구현할 수 있다.

``` swift
struct Dog: Animal {
    func makeSound() {
        print("Woof!")
    }
}

struct Cat: Animal {
    func makeSound() {
        print("Meow!")
    }
}

struct Bird: Animal {
    func makeSound() {
        print("Chirp!")
    }
}
```

### 3. 프로토콜 타입을 사용하여 다형성 구현

- 프로토콜을 채택한 여러 타입의 인스턴스를 프로토콜 타입으로 선언하여 다형성을 구현할 수 있다.
  > 다형성 : 하나의 타입에 여러 객체를 대입할 수 있는 성질
- Animal 타입 변수는 Dog, Cat, Bird 인스턴스를 참조할 수 있고, 각각의 makeSound() 메서드를 호출할 때 다형적으로 동작한다.

``` swift
let animals: [Animal] = [Dog(), Cat(), Bird()]

for animal in animals {
    animal.makeSound() // 각 타입에 맞는 메서드 호출
}
```

### 4. 프로토콜 확장을 활용해 기본 구현 제공

- 프로토콜 확장을 통해 프로토콜을 준수하는 모든 타입에 대해 기본 구현을 제공할 수 있다.
- 다형성을 구현하면서도 코드 중복을 줄인다.

``` swift
extension Animal {
  // Dog, Cat, Bird 타입에 추가적으로 구현하지 않아도 eat()을 사용할 수 있다.
    func eat() {
        print("Eating food")
    }
}

for animal in animals {
    animal.eat()       // "Eating food" 출력
    animal.makeSound() // 각 동물에 맞는 소리 출력
}
```

### 5. 프로토콜 타입을 함수의 파라미터로 사용

- 프로토콜을 함수의 파라미터 타입으로 지정함으로써 다양한 객체를 동일한 함수에서 처리할 수 있다.

``` swift
func describe(animal: Animal) {
    animal.makeSound()
}

describe(animal: Dog()) // "Woof!"
describe(animal: Cat()) // "Meow!"
describe(animal: Bird()) // "Chirp!"
```

### 6. 제네릭과 프로토콜 함께 사용

- 제네릭을 사용하여 다형성을 좀 더 유연하게 구현할 수 있다.
- 제네릭을 사용하면서 특정 프로토콜을 준수하도록 제한하여 다형성을 구현할 수 있다.

``` swift
func performAction<T: Animal>(on animal: T) {
    animal.makeSound()
    animal.eat()
}

performAction(on: Dog()) // "Woof!", "Eating food."
performAction(on: Cat()) // "Meow!", "Eating food."
```

<br/>

## ✏️ 사용자 인터페이스(UI) 테스트와 단위(Unit) 테스트의 차이점은 무엇인가요?

---

### 단위 테스트

- 애플리케이션의 <span style="color:#9fb584">**특정 기능을 독립적으로 테스트하는 것을 목표**</span>로 한다.
- 보통 메소드 수준에서 작성된다.
- 외부 의존성(네트워크 요청, 데이터베이스 접근 등)이 있을 경우, 이를 실제로 호출하지 않고 모의 객체(Mock Object)를 사용해 테스트를 격리시킨다.
- 작은 코드 조각을 테스트하기 때문에 빠르게 실행되며, 즉각적인 피드백을 제공해 코드 수정 시 영향을 최소화 한다.
  - 작은 코드 조각의 문제를 빨리 발견할 수 있어, 전체 시스템에 문제가 퍼지기 전에 수정할 수 있다.
- 각 함수가 예상대로 작동하는지를 확인하기 때문에, 특정 기능에 대한 의도한 구현이 올바른지 검증할 수 있다.
- 코드 변경 시 영향 범위를 쉽게 파악할 수 있고, 리팩토링 후에도 기능이 올바르게 작동하는지 확인할 수 있다.
- 사용 예시
  - 계산기 앱에서 덧셈, 뺄셈 등의 함수를 개별적으로 테스트하여 함수가 정확히 동작하는지 확인한다.
  - 네트워크 서비스의 응답 처리 로직을 테스트하기 위해 모의 응답(Mock Response)을 설정하여 데이터를 파싱 하는 로직이 잘 작동하는지 확인한다.

> 단위 테스트의 예로 함수 결과를 print() 문으로 확인하는 것도 포함되는가?<br/>
> print() 문으로 함수의 결과를 확인하는 것은 단위 테스트와는 다르다. 단위 테스트는 특정 기능의 동작을 자동화된 테스트 코드로 작성하여, 코드가 수정되더라도 동일한 테스트를 반복적으로 실행할 수 있도록 하는 것이 목표다.<br/><br/>
> print() : 매번 수동으로 결과를 확인해야 한다.<br/>
>   - 코드 변경 후 다시 결과를 확인할 때 반복적으로 수동 확인이 필요하며, 코드에 영향을 주지 않고 임시로 확인할 때 주로 사용한다.<br/>
> 단위 테스트 : XCTest 등의 테스트 프레임워크를 사용하여 테스트 케이스를 코드로 작성한다.<br/>
>   - 코드가 변경되어도 테스트가 자동으로 실행되기 때문에 코드가 예상대로 동작하는지 쉽게 확인할 수 있다.<br/>
>   - 각 함수가 기대한 대로 작동하는지 예상 결과와 실제 결과를 비교하여, 성공 여부를 자동으로 판별한다.<br/>

### 사용자 인터페이스 테스트 

- 사용자 관점에서 애플리케이션을 테스트하며, 사용자가 애플리케이션과 상호작용하는 모든 흐름을 자동화하여 테스트한다.
- 버튼 클릭, 텍스트 입력, 화면 전환 등 실제 UI 요소와 상호작용한다.
  - 사용자가 앱을 사용할 때의 경로와 흐름을 테스트할 수 있다.
- 기능이 아닌 사용자의 경험을 확인하는 데 중점을 둔다.
  - 화면이 올바르게 표시되는지, 버튼을 클릭했을 때 올바른 화면으로 이동하는지 등을 확인한다.
  - 사용자 경험에 기반한 검증을 통해 예상치 못한 오류나 사용자 불편 요소를 발견할 수 있다.
- 전체 화면을 테스트하기 때문에 상대적으로 오래 걸리고, 다양한 디바이스 환경에서 테스트해야 할 수도 있다.
- iOS에서는 XCTest 프레임워크와 XCUITest를 사용하여 UI 테스트를 작성할 수 있다.
- 새로운 기능을 추가하거나 기존 기능을 수정할 때, UI 테스트는 기존 UI가 안정적으로 작동하는지 확인하여 회귀 버그를 방지할 수 있다.
- 배포 전에 실제 디바이스 환경과 유사한 조건에서 앱의 전반적인 동작을 확인하는 데 유용하다.


### 차이점 비교

|구분|단위 테스트|사용자 인터페이스  테스트|
|:------|:------:|:------:|
|대상|특정 기능, 함수 또는 메서드|사용자 인터페이스와 전체 앱 흐름|
|목적|개별 기능 검증 (코드의 정확성 확인)|사용자 경험 검증 (UI 흐름 및 사용성 확인)|
|외부 의존성|외부 의존성을 모의 객체로 대체하여 격리|실제 UI 환경에서 실행|
|테스트 속도|매우 빠름 (로컬에서 실행)|상대적으로 느림 (UI 요소를 실제로 렌더링)|
|유지보수 비용|낮음|높음 (UI 변경 시 테스트 수정 필요)|
|실행 빈도|자주 실행 (빌드할 때마다 또는 코드 변경 시)|보통 배포 전 또는 주요 기능 변경 시|


<br/>

## ✏️ XCTest 프레임워크를 사용하여 테스트를 작성하는 방법은 무엇인가요?

---


<br/>

## ✏️ 테스트 주도 개발(TDD)의 장점은 무엇인가요?

---


<br/>

## ✏️ 의존성 주입(Dependency Injection)을 활용하여 테스트 가능한 코드를 작성하는 방법은 무엇인가요?

---



<br/>
<br/>

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source