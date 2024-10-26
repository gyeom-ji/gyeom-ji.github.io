---
title: "객체지향 프로그래밍 OOP (Object-Oriented Programming)"
author: gyeomji
date: 2024-06-03 10:00:00 +0900
categories: [Swift]
tags: [객체지향 프로그래밍, OOP]
pin: false
math: true
mermaid: true
---

## Swift

---

swift는 멀티 패러다임 언어로 아래와 같은 프로그래밍 패러다임을 차용하여 만들어졌다. 애플의 프레임워크 대부분은 객체지향 프로그래밍 패러다임을 기반으로 설계된 클래스로 구성되어 있기 때문에 객체지향 프로그래밍 패러다임을 수용해야만 한다. 또한, swift는 함수형 패러다임을 강조하는데 애플 프레임워크를 벗어나 다른 영역에서 swift를 사용했을 때 순수하게 함수형 프로그래밍 패러다임으로 프로그램을 작성할 수 있기 때문이다.

- 명령형 프로그래밍 패러다임
- 객체지향 프로그래밍 패러다임
- 함수형 프로그래밍 패러다임
- 프로토콜 프로그래밍 패러다임

### swift 특징

- ARC (Automatic Reference Counting) 지원
- Objective-C의 동적 객체 모델과 매개 변수 형식 도입
- 컴파일 언어

<br/>

## 객체지향 프로그래밍 OOP (Object Oriented Programming)

---


현실 세계의 개체를 속성과 메소드가 결합한 형태의 객체로 표현한다. 즉, 프로그램을 데이터와 처리 방법으로 나누는 것이 아닌 프로그램을 다수의 `객체`로 만들고 객체들끼리의 상호작용을 통해 하나의 프로그램을 만드는 방식이다.

> 클래스 Class : 특정 객체 내에 있는 변수와 메소드를 정의하는 일종의 틀으로 객체 지향 프로그래밍에서 데이터를 추상화하는 단위이다. 하나 이상의 유사한 객체들을 묶어 하나의 공통된 특성을 표현한다. 속성은 변수 형태, 행위는 메소드 형태로 선언한다.<br/> <br/> 
> 객체 Object : 물리적, 추상적으로 자신과 다른 것을 식별 가능한 대상으로 클래스에서 정의한 것을 토대로 메모리에 할당된다. 객체마다 각각의 상태와 식별성을 가진다.<br/> <br/> 
> 메소드 Method : 클래스로부터 생성된 객체를 사용하는 방법으로 객체가 메시지를 받아 실행해야 할 객체의 구체적인 연산이다.<br/> <br/> 
> 메시지 Message : 객체 간 상호 작용을 하기 위한 수단으로 객체에게 어떤 행위를 하도록 지시하는 방법이다. 객체 간 상호 작용은 메시지를 통해 이뤄지고 메시지는 객체에서 객체로 전달된다.<br/> <br/> 
> 인스턴스 Instance : 객체 지향 기법에서 클래스를 통해 만든 실제의 실형 객체로 클래스에 속한 각각의 객체이다. 실제로 메모리상에 할당된다.<br/> <br/> 
> 속성 Property : 한 클래스 내 속한 객체들이 가지고 있는 데이터 값들을 단위별로 정의한 것으로 성질, 분류, 식별, 수량, 현재 상태 등에 대한 표현 값이다.<br/> 

<br/> 

### 결합도와 응집도

#### 결합도 Coupling

- 서로 다른 모듈 간 상호 의존하는 정도 또는 연관된 관계를 의미한다.
- 모듈 간 관련성을 나타내는 척도이다.
- 결합도가 높을 경우 모듈간 의존하는 정도가 크기 때문에 코드 수정 시 다른 모듈에 영향(Side Effect)을 끼칠 수 있다.
- 오류 발생 시에도 다른 모듈에 영향을 끼칠 수 있다.
- 결합도가 낮을수록 좋다.

<br/> 

#### 응집도 Cohesion

- 모듈 내부 요소들 간 기능적 연관성을 나타내는 척도이다.
- 모듈이 얼마나 독립적인지를 나타낸다.
- 응집도가 높을 경우 수정, 오류 발생 시 하나의 모듈 안에서 처리할 수 있다.
  - 유지보수가 용이하다.
- 응집도가 높을수록 좋다.

<br/> 

### OOP 객체지향 프로그래밍의 주요 특성

#### <span style="color:#9fb584">**1. 추상화**</span> 

- 객체의 공통적인 속성과 기능을 추출하여 정의하는 것
  - 현실 세계의 사물을 객체로 보고 필요한 공통 특성만 다뤄 현실의 복잡성을 제거하고 목적에 집중할 수 있게 한다.
  - ex) 사자, 호랑이, 강아지 ➡️ 표유류

#### <span style="color:#9fb584">**2. 캡슐화**</span> 

- 객체 수행 목적에 따라 데이터와 함수를 결합시켜 외부에 내부 기능 구현 내용을 감추고 필요한 인터페이스만 밖으로 드러낸다.
  - 캡슐화는 관련있는 변수와 함수를 하나의 클래스로 묶고 외부에서 접근하지 못하도록 `은닉`하는 것이 핵심이다. 
  - 객체에 직접적인 접근을 막아 외부에서 내부의 정보에 직접 접근하거나 변경할 수 없고 객체가 접근하는 필드와 메서드를 통해서만 접근이 가능하다.
  - 접근제어자를 통한 설계가 잘 이뤄져야 한다.
    - 자신 내부의 모듈을 감추고 다른 모듈 내부 작업도 직접적으로 개입하지 못하도록 설계해야한다.
    - public, private, getter, setter 가 존재하는 이유
  - 결합도가 낮아지고 재사용이 용이하다.
  - 인터페이스가 단순화된다.
  - 변경 발생 시 오류의 파급 효과가 적다.


> [ 외부 객체가 알지 못하게 캡슐화를 하는 이유 ]<br/>
> 은닉화 특징의 장점을 가지기 때문이다.<br/>


#### <span style="color:#9fb584">**2-1. 은닉화**</span> 

- 외부에는 내부 기능 구현 내용을 감추고 이용 방법만 알려주는 것으로, 내부의 중요한 사항이 외부에 노출되지 않도록 막는다.
- 캡슐화의 한 개념으로 더 구체적인 개념이다. 
- 변수를 private로 지정하고 setter, getter로 접근, 제어한다.
- 외부에서 특정 객체의 데이터 및 함수로 직접 접근하는 것을 막음으로써 변경 하지 못하도록 하여 유지보수나 확장시 오류의 범위를 최소화할 수 있다. 
  - 객체 내 정보 손상과 오용을 방지하고 조작법이 바뀌어도 사용방법 자체는 바뀌지 않아서 오류의 범위를 최소화한다.
- 데이터가 변경되어도 다른 객체에 영향을 주지 않는 독립성을 갖는다.
- 처리된 결과를 사용하므로 이식성이 좋고 객체를 모듈화 할 수 있어서 새로운 시스템의 구성에 하나의 모듈처럼 사용 가능하다.


#### <span style="color:#9fb584">**3. 상속**</span> 

- 재사용과 확장성을 위해 상위 개념의 특징을 하위 개념이 물려받아 사용하는 것
  - OCP 원칙에 따라 기존 코드를 변경하지 않으면서 기능을 확장할 수 있도록 설계하기 위해 사용한다.
  - 상속은 코드의 재사용 관점이 아닌 기능의 확장 개념으로 생각해야 한다.
  - 코드 길이를 줄이고 가독성을 높일 수 있다.

> [ swift 상속의 특징 ] <br/>
> - 클래스 / 프로토콜에서만 가능하다.<br/>
> - 단일 상속만 가능하다. <br/>
>     - 하나의 클래스만 상속 가능하다.<br/>
>     - 프로토콜은 여러 개 채택 가능하다.<br/>
> - 상속받은 클래스도 새로운 자식 클래스에게 상속이 가능하다.


#### <span style="color:#9fb584">**4. 다형성**</span> 

- 상속과 연관된 개념으로, 한 객체가 다른 여러 형태(객체)로 재구성되는 것
  - 한 부모 밑에서 상속받는 자식들이 모두 똑같지는 않다는 개념이다.
  - 오버로드와 오버라이딩처럼 자식 클래스가 다양한 형태로 존재할 수 있게 하는 특징이다.
  - Overriding : 자식클래스가 부모클래스에서 상속할 메소드, 프로퍼티, 서브스크립트를 원하는대로 재정의 하는 것
  - Overloading : 함수의 이름은 같으나, 매개변수, 리턴타입 등을 다르게 설정하여 함수를 중복으로 선언 하는 것
    - 타입이나 리턴 값이 다르지만 동일한 기능을 하는 함수를 하나로 만들 수 있다.


>💡  <span style="font-size: 15px"> instance : 클래스, 구조체에서 생성된 **객체** <br />
> 　 property : 클래스, 구조체의 객체에 들어있는 **정보, 값**<br />
> 　 method : 클래스, 구조체 객체에 들어있는 **함수** </span>

<br />

### OOP 객체지향 프로그래밍의 장점

- 재사용성 : 상속을 통해 코드의 재사용성을 높일 수 있다.
- 생산성 향상 : 잘 설계된 클래스를 만들어 독립적인 객체를 사용함으로써 개발의 생산성을 향상시킬 수 있다.
- 자연적인 모델링 : 일상 생활 모습의 구조가 객체에 자연스럽게 녹아들었기 때문에 생각하고 있는 것을 자연스럽게 구현할 수 있다. (기능 별로 나눠서 구현)
- 유지보수의 우수성 : 기존 기능 수정 시 함수를 새롭게 바꾸더라도 함수의 세부 정보가 은닉되어 있기 때문에(캡슐화) 주변에 미치는 영향을 최소화 한다.(유지보수의 우수성) 새로운 객체의 종류를 추가할 때는 상속을 통해서 기존의 기능을 활용하고, 존재하지 않는 새로운 속성만 추가하면 되므로 매우 경제적이다.

<br />

### OOP 객체지향 프로그래밍의 단점

- 많은 오버헤드가 발생한다.
  - 객체 간의 정보 교환이 모두 메시지 교환을 통해 일어나기 때문이다.
- 상태를 가지는 객체로 인한 버그 발생 가능성이 존재한다.
  - 내부 변수로 인해 객체가 예측할 수 없는 상태를 가지게 되기 때문이다.

<br />

## SOLID 원칙 (객체지향 프로그램 및 설계의 기본 원칙 다섯가지)

---

### <span style="color:#9fb584">**SRP - Single Responsibility Principle 단일 책임 원칙**</span>

클래스는 단 하나의 책임을 가져야 하며, 클래스를 변경하려는 이유는 단 하나여야 한다는 원칙

- 하나의 책임이 여러 클래스에 나눠 있으면 안된다.
- 변화에 대한 유연성 확보를 위해 이 원칙을 지켜야 한다.
- 낮은 결합도와 높은 응집도를 추구하기 위해 이 원칙을 지켜야 한다.

<br/>

#### [swift에서 SRP를 위반한 예시]

- MVC 패턴이 가진 Massive View 현상
    - Massive View : MVC의 흐름은 Model -> Controller -> View가 되어야 하는데, View가 Model과 Controller의 도움 없이 모든 것을 다 해내는 것

=> MVC 패턴 단점을 극복하기 위해, 많은 역할을 하는 VC를 분할하여 단일 책임만을 가지는 여러 클래스를 만드는 다른 패턴들이 생겨나고 있다.

``` swift
// 하나의 클래스가 여러 개의 책임(func)을 가짐
class Market {
    func manage() {
        stock()
        sell()
        deliver()
    }
    
    private func stock() {}
    
    private func sell() {}
    
    private func deliver() {}
} 

// 하나의 클래스가 하나의 책임을 가지도록 분리
class Market {
    let stockManager: StockManager
    let sellManager: SellManager
    let deliveryManager: DeliveryManager
    
    init(stockManager: StockManager, sellManager: SellManager, deliveryManager: DeliveryManager) {
        self.stockManager = stockManager
        self.sellManager = sellManager
        self.deliveryManager = deliveryManager
    }
    
    func manage() {
        stockManager.stock()
        sellManager.sell()
        deliveryManager.deliver()
    }
}

class StockManager {
    func stock() {}
}

class SellManager {
    func sell() {}
}

class DeliveryManager {
    func deliver() {}
}
```

<br/>

#### [SRP를 지키기 위한 방법]

- 클래스를 작게 만든다.
  - 클래스(200줄 이내)와 함수(10줄 이내)를 작게 만드는 것이 무조건적인 SRP를 충족시키기 위한 충분 조건은 아니지만 필요 조건이라 할 수 있다.
  - 함수가 짧으면 하나의 역할을 가진 함수가 될 수 밖에 없고, `추상화 레벨`에 대해 고민할 수 밖에 없다.
    > 추상화 레벨 : 얼만큼의 정보를 해당 함수에서 노출하여 보여줄 것 인지 정하는 것<br/>
    > ex) 라면 조리법<br/>
    > 1. 물 500ml를 끓인다. ➡️ 2. 면과 스프를 넣는다. ➡️ 3. 5분 동안 끓인다.<br/>
    > 위의 단계에서 1번 과정을 냄비를 꺼낸다, 물을 받는다 등 여러 동작으로 나눌 수 있지만 이를 조리법에 포함시키지는 않았다. 물 500ml를 끓인다는 단계(함수)로 묶어서 표현 했기 때문이다.<br/>
    > 예시처럼 작업(함수)을 어느 수준까지 나눠서 설명(추상화)할 건지 지속적으로 고민하게 되므로 함수를 짧게 만드는 것은 SRP의 필요조건이다.
  - 클래스를 작게 만들기 위해서는 프로퍼티나 함수 메서드를 마음대로 만들 수 없다. 
  - 해당 프로퍼티, 함수가 꼭 필요한지에 대한 지속적인 고민이 필요하다. 
    - 해당 클래스에 있을 이유가 없는데 존재하게 된다면 SRP 위반이다.
    - 함수나 메소드 내부에서 self의 프로퍼티나 메소드를 얼마나 쓰고 있는지 확인한다.
    - 하나도 사용하고 있지 않다면 해당 클래스 내부에 있을 이유가 없다.
  - 함수 내부에서 self의 호출 빈도가 적을 수록 클래스와 연관성이 떨어지므로 클래스를 리팩토링 시켜야할 상황이 오면 우선적으로 내쫓을 후보가 된다.


<br/>

### <span style="color:#9fb584">**OCP - Open-Closed Principle 개방-폐쇄 원칙**</span>

모듈은 확장에는 열려있어야 하고, 변경에는 닫혀있어야 한다. 기능을 변경, 확장할 수 있으면서 그 기능을 사용하는 코드는 수정하지 않아야 한다.

- Open : 기능 추가나 변경을 할 수 있어야 한다.
- Closed : 기능 추가를 할 때 그 모듈을 쓰고 있는 코드들은 수정하지 않아야 한다.


<br/>

#### [OCP를 위반한 예시 ]

- 어떤 타입에 대한 반복적인 분기문
    - 하나의 enum에 여러 군데에서 반복적으로 if/switch문을 사용하고 있다면 고민을 해봐야한다.
    - 기능 추가는 case로 추가하면 되지만, 추가하는 순간 enum을 스위칭하고 있는 모든 코드를 찾아 수정해줘야한다. (OCP 위반)

> enum 적절한 사용 방법 : <br/>
> enum 자료형은 수정할 가능성이 적은 코드(확장 가능성이 적을 때)에서 사용한다. 즉 case가 계속해서 추가될 가능성이 있는 코드는 enum 보다 프로토콜 정의 후, 새로운 타입으로 구현하는 것이 좋다. 

<br/>

#### [OCP를 지키기 위한 방법]

- protocol을 사용하여 if/ switch 문을 대체하기

##### protocol 사용하기

- 프로토콜을 상속받아 사용하는 방법으로, 직접적으로 OCP를 지키는 구조이다.
- 분기 처리 하지 않아도 되는 코드를 protocol로 바꾸는 리팩토링 기술을 `조건부를 다형성으로 바꾼다`라고 한다.
  - 조건의 분기와 일치하는 하위 클래스를 만들어 객체 클래스에 따라 다형성을 통해 적절한 구현이 달라지게 하는 방식이다.
  - 다형성은 하위 자식들이 모두 같은 자식은 아니다 라는 개념이다.
  - 자식 클래스에서 각자의 구체적인 계산 로직을 정의하는 방식으로 조건부를 다형성으로 바꾼다 라고 말하는 것이다.
  - 이렇게 다형성으로 바꾸게되면 가독성 있는 코드를 만들 수 있다. 
- 분기 처리 하지 않아도 되는 코드를 protocol로 바꿀 수 있는 스위프트 언어를 `프로토콜 지향 프로그래밍`이라 한다.

``` swift

// enum을 사용한 분기 처리 ❎
enum Animal {
    enum Size {
        case small
        case medium
        case large
    }

    case dog
    case cat
    case pig

    var noise: String {
        switch self {
            case .dog :
            return "Bark"
            case .cat :
            return "Meow"
            case .pig :
            return "Oink"
        }
    }

    var size: Size {
        switch self {
        case .dog:
            return .medium
        case .cat:
            return .small
        case .pig:
            return .large
        }
    }
}

// protocal으로 분기문 대체 ✅

enum AnimalSize {
    case small
    case midium
    case large
}

protocol Animal {
    var noise: String {get}
    var size: AnimalSize {get}
}

struct Dog: Animal {
    var noise = "Bark"
    var size = AnimalSize.medium
}

struct Cat: Animal {
    var noise = "Meow"
    var size = AnimalSize.small
}

struct Pig: Animal {
    var noise = "Oink"
    var size = AnimalSize.large
}
```

- 새로운 유형 추가 시 Animal 프로토콜을 준수하는 새로운 struct를 추가하면 된다.
  - 새롭게 추가되었다고 코드가 수정되지 않는다.
  - OCP원칙을 제대로 지키는 방식이다.

``` swift
// 새로운 유형 추가 ➡️ 다른 코드에 어떤 영향도 주지 않음 OCP 준수 ✅
struct Goat: Animal {
    var noise = "Baa"
    var size = AnimalSize.large
}
```

<br/>


### <span style="color:#9fb584">**LSP- Liskov Substitution Principle 리스코프 치환 원칙**</span>

자식 클래스는 부모 클래스의 역할을 완벽하게 할 수 있어야 한다는 원칙

- 상속을 사용했을 때 자식 클래스는 부모 클래스 대신 사용 되어도 같은 동작을 해야 한다.
  - B가 A의 자식 타입이면 부모 타입인 A객체는 자식 타입인 B로 치환해도 작동에 문제가 없어야 한다.
- 자식 클래스는 부모 클래스가 따르던 제약사항을 모두 따라야 한다.
- 자식 클래스는 부모 클래스의 동작을 바꾸지 않아야 한다.
- 즉, <span style="color:#9fb584">**자식이 부모의 동작을 제한해서는 안된다**</span>

<br/>

#### [LSP를 위반한 예시 ]

- 직사각형(Rectangle)을 상속받아서 만든 정사각형(Square) 클래스에서의 LSP 위반
  - 정사각형이 직사각형을 완전히 대체했다면 동일한 결과인 50이 반환되어야 하지만, 25가 반환된다.
  - 너비와 높이를 자유롭게 바꿀 수 있는 직사각형 부모 클래스의 동작을 제한해야만 원하는 동작을 할 수 있다. (LSP 위반)

``` swift
class Rectangle {
    var width: Int = 0
    var height: Int = 0
    
    func area() -> Int {
        return width * height
    }
}

class Square: Rectangle {
    override var width: Int {
        didSet {
            super.height = width
        }
    }
    
    override var height: Int {
        didSet {
            super.width = height
        }
    }
}

let rectangle = Rectangle()

rectangle.height = 10
rectangle.width = 5

print(rectangle.area())
// 결과값 : 50

let square = Square()
square.height = 10
square.width = 5

print(square.area())
// 기댓값 : 50 (정사각형이 직사각형을 완전히 대체했다면 동일한 결과인 50이 반환되어야 하기 때문)
// 결과값 : 25
```

- iOS 프레임워크에서의 LSP 위반
  - 10개의 UIView subclass들의 높이를 30으로 바꿨으니 300이 되길 예상하지만 실제 결과는 272다. 
  - 일부 뷰들은 intrinsicSize를 마음대로 바꿀수 없게 되어 있기 때문이다. 
  - 이렇게 UIView(부모 타입)의 동작(높이를 바꾸는 것)을 제한하는 일부 뷰 (UIProgressView, UISwitch 등)이 바로 LSP 위반이다.

``` swift

var label = UILabel(frame: .zero)
var button = UIButton(frame: .zero)
var segmentedControl = UISegmentedControl(frame: .zero)
var textField = UITextField(frame: .zero)
var slider = UISlider(frame: .zero)
var switchButton = UISwitch(frame: .zero)
var activityIndicator = UIActivityIndicatorView(frame: .zero)
var progressView = UIProgressView(frame: .zero)
var stepper = UIStepper(frame: .zero)
var imageView = UIImageView(frame: .zero)

let views: [UIView] = [...] //위 뷰들을 UIView 어레이에 저장

views.forEach { $0.frame.size.height = 30 } //뷰들의 height를 30으로 변경

let height = views.reduce(0) { $0 + $1.frame.height } 
print(height) 

// 기댓값 : 300
// 결과값 : 272
```

- 아래와 같이 부모의 함수를 오버라이드해서 퇴화시켜 버리는 함수를 만들 경우 LSP 위반이다.

``` swift
required init?(coder aDecoder: NSCoder) {
  fatalError("init(coder:) has not been implemented")
}
```

<br/>

#### [LSP를 지키기 위한 방법]

- 올바르지 못한 상속 관계를 제거하고 부모 객체의 동작을 완벽하게 대체할 수 있는 관계만 상속하도록 코드를 설계한다.
- <span style="color:#9fb584">**상속보다는 구성(composition)을 이용**</span>한다.
  - 상속보다 프로토콜로 동작을 추상화하고 확장(Extension)을 통해 프로토콜 타입에 대한 기본 구현을 제공하면 타입 계층에 무관하게 다형성을 실현할 수 있다.
- LSP를 절대 어기지 않고 프로그래밍하는 것은 어렵지만, 너무 많은 곳에서 LSP를 어길 경우 문제가 발생한다.
  - 상위 클래스를 기준으로 코딩할 수 없어지고, 그렇게되면 상속 자체가 의미가 없어지고 OCP조차 지킬수 없게 된다.
  - UITableView는 UITableViewCell을 기준으로 만들어져 있기 때문에 커스텀 셀이 LSP를 지키지 않는다면 테이블뷰도 제대로 동작하지 않는다. 
- LSP를 모든 곳에서 지키려고할 경우 과도한 추상화로 인해 코드의 복잡성이 올라갈 수 있다.
- 따라서 상속이 정말 필요한지, 하위 클래스가 기반 클래스의 규약을 지킬 수 있을지에 대한 확인이 필요하다.

``` swift
// Quadrangle 프로토콜으로 area를 추상화 시켜 각 객체에서 프로토콜을 채택하고 area를 구하는 로직을 구현
protocol Quadrangle {
    func area() -> Int
}

class Rectangle: Quadrangle {
    var width: Int
    var height: Int

    init(width: Int, height: Int) {
        self.width = width
        self.height = height
    }

    func area() -> Int {
        return width * height
    }
}

class Square: Quadrangle {
    var length: Int

    init(length: Int) {
        self.length = length
    }

    func area() -> Int {
        return length * length
    }
}

let rectangle = Rectangle(width: 10, height: 5)

print(rectangle.area())

let square = Square(length: 5)

print(square.area())

```

<br />

### <span style="color:#9fb584">**ISP- Interface Segregation Principle 인터페이스 분리 원칙**</span>

클라이언트가 자신이 사용하지 않는 메소드에 의존하지 않도록 인터페이스를 각각 사용에 맞게 작고 구체적으로 분리해야 한다는 원칙

- 객체는 자신이 사용하는 메소드에만 의존해야 한다.
- 인터페이스는 지나치게 광범위하거나 많은 기능을 구현해서는 안된다.
  - 인터페이스가 거대해지는 경우 SRP를 위반할 수 있다.
- 인터페이스를 사용하는 객체를 기준으로 잘게 분리되어야 한다.
  - 클라이언트가 필요한 부분만 선택하여 사용할 수 있도록 한다.
- ISP를 위반할 경우 (Fat Interface))
  - 하나의 프로토콜에 2가지 이상의 서로 다른 책임을 가지고 있어 결합도가 높아진다.
    - 사용하지 않는 메서드를 강제로 선언해야 하고 불필요한 코드가 늘어난다.
    - 해당 클래스를 상속받은 클래스 또한 해당 메서드를 오버라이딩해야 한다.
  - 타입에 대한 이해와 테스트가 어려워질 수 있다.
    - 테스트 측면에서는 Fat Interface가 요구하는 불필요한 메서드를 모두 구현해야 하므로 더 큰 Mock를 만들어야 한다.
  
<br />

#### [ISP를 위반한 예시 ]

- FaxMachine은 convert() -> PDF, UIImage를 Iphone은 fax() 메소드를 사용하지 않는다. (ISP 위반)

``` swift

protocol Machine {
    func convert(document: Document) -> PDF?
    func convert(document: Document) -> UIImage?
    func fax(document: Document)
}

class FaxMachine: Machine {
    func convert(document: Document) -> PDF? { // PDF 변환 불가능
        return nil 
    }

    func convert(document: Document) -> UIImage? { // 이미지 변환 불가능
        return nil 
    }

    func fax(document: Document) {
		    ...
    }
}

class Iphone: Machine {
    func convert(document: Document) -> PDF? {
        return PDF(document: document)
    }

    func convert(document: Document) -> UIImage? {
        return UIImage()
    }

    func fax(document: Document) { } // 팩스 불가능
}


```

<br/>

#### [ISP를 지키기 위한 방법]

- 프로토콜을 분리하여 필요한 것만 채택하여 사용할 수 있도록 한다.
    - 프로토콜을 제일 작은 단위로 분리할 경우 과도한 설계가 될 수 있으므로 적절한 중간점을 찾아야 한다.
    - 미래에 PDF로 변환되고 이미지로 변환되지 않은 기계나 PDF로 변환되지 않고 이미지로 변환되는 기계를 추가할 계획이라면 3개의 프로토콜로 분리한다.
    - 그렇지 않을 경우 적절하게 2개의 프로토콜으로 분리한다.
- ISP를 지키기 위해서는 인터페이스를 역할에 따라 분리하여 사용해야 한다. 
  - 이는 곧 모듈 간 결합도를 낮추고 유연성과 확장성을 높일 수 있다. 
  - 하지만 너무 작은 단위로 분리한다면 과도한 설계가 될 수 있으며 관리가 어려울 수 있다. 
  - 분리하는 단위의 적절한 중간점을 찾는 것이 중요하다.

``` swift
// 3가지로 분리
protocol PDFConverter {
    func convert(document: Document) -> PDF
}
protocol ImageConverter {
    func convert(document: Document) -> UIImage
}

protocol Fax {
    func fax(document: Document)
}

// 2가지로 분리
protocol DocumentConverter {
    func convert(document: Document) -> PDF
    func convert(document: Document) -> UIImage
}

protocol Fax {
    func fax(document: Document)
}
```

<br />

### <span style="color:#9fb584">**DIP - Dependency Inversion Principle 의존 역전 원칙**</span>

상위 레벨의 모듈이 하위 레벨 모듈에 의존하지 않고, 둘 다 추상화에 의존해야 한다. 추상화가 세부 사항에 의존하는 것이 아닌 세부 사항이 추상화에 의존해야 한다.

- 의존관계를 맺을 때 변화하기 쉬운 것 또는 자주 변화하는 것(클래스) 보다 변화하기 어려운 것, 거의 변화가 없는 것(인터페이스)에 의존한다.
- 상위 클래스일수록, 인터페이스일수록, 추상 클래스일수록 변하지 않을 가능성이 높기에 하위 클래스나 구체 클래스가 아닌 상위클래스, 인터페이스, 추상 클래스를 통해 의존하라는 것이다. 
- 의존성 역전 원칙을 지키기 위해서는 `의존성 주입`으로 해결 할 수 있다.

> 상위 모듈 : 더 큰 규모의 기능을 수행하는 클래스, 패키지를 의미한다. 즉, 사용자 인터페이스와 관련된 작업을 하는 Controller나 비즈니스 로직을 처리하는 Service 계층이 고차원 모듈에 속한다.<br/><br/>
> 하위 모듈 : 고차원 모듈에서 정의한 기능을 구체적으로 구현하는 클래스 등을 의미한다. 주로 데이터베이스 접근 객체(DAO) 클래스나 파일 처리를 위한 클래스 등이 저차원 모듈에 속한다.<br/><br/>
> [ 상위 모듈이 하위 모듈에 의존하지 않아야 하는 이유 ]<br/>
> 의존성이 높으면 결합도가 높아 소프트웨어의 유연성이 저하되며 코드의 재사용성을 크게 저하한다.<br/> 모듈 간 의존성이 높을 경우 하나의 모듈을 변경하면 전체 모듈에 대해서 변경을 해야하므로 변경에 취약하다.<br/>
또한 독립적인 단위테스트 작성이 어려워진다.


<br/>

#### 의존성 Dependency

- 코드에서 두 모듈(클래스)간의 연결 혹은 관계를 말한다.
  - 클래스 A가 다른 클래스 B를 이용할 때 A가 B에 의존한다고 한다. 
  - 이런 관계에서 A는 B없이 작동할 수 없다.
- 의존하는 객체가 수정되면 다른 객체 또한 영향을 받는다.
- A 클래스가 B 클래스의 인스턴스를 프로퍼티로 가지고 있을 경우 A 클래스가 B 클래스에 의존성이 생긴다.

<br/>

#### 의존성 주입 Dependent Injection

- 내부에서 인스턴스를 생성하는 것이 아닌 <span style="color:#9fb584">**외부에서 인스턴스를 생성하여 클래스에 할당**</span>한다.
  - 생성자, setter를 통해 주입할 수 있다.
- 재사용성이 높아진다.
- 테스트가 용이하다.
- 코드가 단순해진다.
- 결합도는 낮추면서 유연성과 확장성이 향상된다.

<br/>

#### 의존관계 역전의 원칙 IOC (Inversion Of Control)

- 메인 프로그램에서 프레임워크로 객체와 객체의 의존성에 대한 제어를 넘기는 것이다.
- 객체를 직접 생성, 제어하는 것이 아닌 외부에서 관리하는 객체를 가져와서 사용하는 것으로, 클레스 간의 `결합을 느슨하게` 하여 테스트와 유지관리를 더 쉽게 설계하는 원칙이다.

<br/>

#### [ DIP를 위반한 예시 ]

- 자주 변화하는 쉬운 클래스에 의존하고 있다.

``` swift
class MacBook {
    func turnOn() {}
}

class Developer {
    let laptop = MacBook()
    
    func develop() {
        laptop.turnOn() // 의존 관계 성립
    }
}

```
<br/>

#### [DIP를 지키기 위한 방법]

- `의존성 주입` 
  - Developer 생성자는 여전히 MacBook 타입만 받을 수 있다. 
  - laptop을 Gram으로 변경할 경우 Developer는 영향을 받는다. 
  - Developer가 구체적인 MacBook 클래스를 의존하고 있다.

``` swift
class MacBook {
    func turnOn() {}
}

class Gram : Laptop {
    func turnOn() {}
}

class Developer {
    let laptop : MacBook
    
    // 생성자를 통해 의존성을 주입 받음
    init(laptop: MacBook) {
        self.laptop = laptop
    }
    
    func develop() {
        laptop.turnOn()
    }
}

// 외부에서 의존성 주입
let gyeomji = Developer(laptop: MacBook())
```

- `의존성 역전 원칙 IOC`에 따라 의존성을 분리 한다.
  - Developer가 MacBook이 아닌 추상화된 Laptop에 의존하고 있다.
  - Developer는 사용하는 laptop을 변경해도 영향을 받지 않는다.
  - 구체적인 MacBook, Gram 클래스는 Laptop 인터페이스에 의존한다.
  - 의존 관계의 방향이 역전됐다.

``` swift
protocol Laptop {
    func turnOn()
}

class Developer {
    let laptop : Laptop
    
    init(laptop: Laptop) {
        self.laptop = laptop
    }
    
    func develop() {
        laptop.turnOn()
    }
}

class MacBook : Laptop {
    func turnOn() {}
}

class Gram : Laptop {
    func turnOn() {}
}

let gyeomji = Developer(laptop: MacBook())
```
<br/>

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
