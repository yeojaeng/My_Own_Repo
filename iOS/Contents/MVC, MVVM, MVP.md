# MVC, MVVM, MVP

개발자는 프로젝트를 시작하기 이전에, 혹은 이미 진행중인 프로젝트에 투입되어 프로젝트를 진행하게 된다면 **어떠한 아키텍처 패턴** 으로 설계되어 있는지 파악해야 한다.

오늘은 iOS 개발시 주로 사용되는 MVC, MVVM, MVP에 대해 간단히 살펴보도록 한다.

<br>

### 아키텍처의 역할
---

본문 시작에 앞서 오늘날에는 왜이리 많은 디자인 패턴이 존재하며 이 선택이 왜 중요한지에 대해 짚고 넘어가도록 한다.

만일 우리가 정말 무수히 많은 양의 코드가 존재하는 대형 프로젝트를 진행하는 도중에 디버깅을 해야하는 상황이라고 생각해보자, 이러한 경우 우리는 클래스의 모든 속성들을 머릿속에 담아두고 기억하기 어렵기 때문에 어떠한 버그도 찾지 못하고 고치지도 못하는 불상사가 일어날 수 있다.

**좋은 패턴들은 아래와 같은 공통된 특징을 갖는다.**

- 개체들 간 균형있는 **책임 분리**
- 테스트의 가능 여부 , **Testability**
- 유지보수 & 사용의 용이성

위 특징들의 각각에 토픽에 대하여 간단히 짚고 넘어간다.

#### 왜 분리해야 하는가?
복잡한 문제를 해결하기 위해서는 해결하기 위한 키 포인트를 잘 잡아내야 한다.

가장 빠르고 효율적으로 복잡한 것을 극복해내기 위해서는 **단일 책임 원칙**으로 수 많은 개체의 책임을 분리한다.

#### 왜 테스트가 가능해야 하는가?
테스트는 런타임 내에서의 이슈를 찾는것을 도와주며, 반대로 만일 실제 유저에게 이슈가 발생한다면 이걸 고친 앱을 다시 유저가 사용하기까지 생각보다 긴 기간이 걸린다.

#### 왜 유지보수와 사용이 편해야 하는가?
일반적으로 적은 양의 코드에서 버그가 적게 발생한다.
게으른 개발자는 같은 기능을 하되 적은 양의 코드로 작성하기를 갈망한다.

각각의 디자인 패턴에을 앱의 특성에 연결하여 보다 적합한 설계를 가지고 작업을 하면 **개발 및 관리 효율**이 증가하기 때문에 다양한 디자인 패턴이 존재한다.


### MVC
---

![image](https://user-images.githubusercontent.com/33051018/82071231-c5154d80-9710-11ea-9cc4-c4b45297fbcd.png)

가장 빈번히 사용했던 MVC패턴에 대하여 먼저 알아보도록 한다.

우리가 아는 일반적인 MVC 패턴은 `Model`, `View`, `Controller` 의 약자로 네이밍 되었다.

`Model`은 애플리케이션에서 사용할 데이터들을 관리하고, `View`는 사용자가 보게 될 `User Interface`를 관리하고, `Controller`는 `Model` 과 `View` 의 중간에서 데이터를 가공하여 전달한다.

`View`에서 사용자의 입력을 `Model`에 반영하고, `Model`의 변화를 `View`에 갱신한다.

즉, `Model`과 `View`는 직접적으로 연결되어져 있지 않고 그 사이의 `Controller`를 통해 서로에게 접근한다.

하지만 `Apple` 에서의 `MVC`는 우리가 아는 일반적인 `MVC` 와는 조금은 다르다.

![image](https://user-images.githubusercontent.com/33051018/82071549-381ec400-9711-11ea-93b8-71beff6f0616.png)

애플에서는 `View` 와 `Controller`가 보다 깊은 관계를 가지고 `ViewController`가 거의 모든 일을 한다.

<br>

### MVVM
---
![image](https://user-images.githubusercontent.com/33051018/82072887-5ab1dc80-9713-11ea-988a-c79fceaf58a0.png)

MVVM은 Model, View, ViewModel로 구성된 패턴이다.

`Controller` 의 위치를 `ViewModel` 이 대체하였다.

`ViewModel` 은 View를 위한 Model이다. 즉 View를 나타내주기 위한 데이터들을 처리하는 역할을 한다.

그림을 보면 `View` 와`ViewModel` 사이에는 바인딩 연결고리가 있다.

각각의 부분을 독립적으로 모듈화하여 개발할 수 있으며 결론적으로 이 둘 사이의 의존성을 덜 수 있다. 

<br>

### MVP
---
![image](https://user-images.githubusercontent.com/33051018/82073726-99946200-9714-11ea-9312-522f5b3959b6.png)

MVP 모델은 기존의 Model 과 View 는 가져가되, Presenter라는 개념이 도입되었다.

Model 은 기존 역할을 이어간다. 실제 데이터를 가지고 있으며 이것을 Presenter가 소유하고 갱신한다. 

View는 모든 비즈니스 로직은 Presenter에게 맡긴다, UIView & UIViewController가 여기에 속한다.

Presenter : UIKit을 사용하지 않는 모든 비즈니스 로직을 수행한다. 또한 Model을 가공하여 View의 포맷을 바꾸기도 한다.

MVP 패턴은 기존 MVC의 장점을 가져가며 사용자에게 보여주는 부분과 비즈니스 로직을 분리하여 모듈화 정도를 심화시켰다. 따라서 이는 곧 테스트 부분에서 기존보다 용이하게 이뤄질 수 있다는 것을 의미한다.
