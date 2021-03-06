# Completion Handler

본 문서에는 평소에 공부를 진행하며 한번 정리가 필요하다고 생각했던 `Completion Handler`에 대한 내용을 기재한다.

<br>

## Prerequisite
---

`Completion Handler` 개념은 알면 알수록 어려운 개념이다..

Closure (특히 Trailing Closure) 에 대한 이해가 부족하다면 [Closure Summary](https://github.com/yeojaeng/My_Own_Repo/blob/master/iOS/Contents/Closure.md) 를 비롯한 다양한 레퍼런스들을 앞서 살펴보는 것이 좋지만 `Trailing Closure` 개념에 대해 간단히 정리하고 본 내용으로 들어가도록 한다.

<br>

### Trailing Closure
Trailing Closure, 후행 클로저는 함수의 **마지막** 인자로 클로저 표현식을 함수에 전달하거나 클로저 표현식이 긴 경우에 사용한다. (매우 꿀 기능이다!)

아래 코드 예시를 살펴보도록 하자.

```swift
func travel(action: () -> Void) {
    print("I'm getting ready to go.")
    action()
    print("I arrived")
}
```

위 `travel` 메서드는 인자로 action 이라는 `Closure`를 채택하고 있다,

action은 함수 내부에서 두 번의 프린트 사이에 실행된다.

우리는 위 `travel` 메서드를 아래와 같은 방식으로 사용할 수 있다.

```swift
travel() {
    print("I'm driving in my car")
}
```

위 예시의 경우에는 인자가 필요없기 때문에 인자 표현을 위한 `()` 또한 생략이 가능하다.

```swift
travel {
    print("I'm driving in my car")
}
```

<br>
<br>

## Completion Handler?
---

자 이제는 `Completion Handler`가 무엇인지 살펴보도록 한다.

사용자가 앱을 사용하는 동안에 앱을 업데이트를 진행하고 개발자는 업데이트가 끝나면 이를 반드시 사용자에게 알려야 하는 상황이라고 가정해보자.

개발자가 업데이트가 끝났음을 알리기 위해 내부에는 "업데이트가 정상적으로 진행되었습니다" 라는 문구가 작성되어 있는 `alert`창을 띄운다.

이렇듯 `Completion Handler`는 어떠한 일이 끝났을 떄 진행할 업무를 담당한다.

<br>

## Introduce
---

아래 코드는 모두가 익숙할만한 다음 `ViewController`로 화면을 전환하는 코드다.

```swift
import UIKit

let firstVC = UIViewController()
let nextVC = UIViewContrller()

firstVC.present(nextVC, animated: true, completion: nil)
```

`present` 메서드 마지막 파라미터 `completion`에 주목하자.

`completion` 파라미터에는 `()->()` 또는 `()->Void` 타입의 클로저 블락을 사용할 수 있다.

아래와 같이 `completion` 파라미터에 클로저를 적용시켜본다.

```swift
firstVC.present(nextVC, animated: true, completion: { () in print("화면전환 완료")})
```

위 메서드가 정상적으로 실행되면 nextVC가 팝업되면서 콘솔창에는 "화면전환 완료" 라는 문자열이 출력된다.

위 코드는 아래와 같이 축약 또한 가능하다.

```swift
firstVC.present(nextVC, animated: true, completion: { print("화면전환 완료")})
```

인자값이 없으니 이를 생략하고 `in` 키워드 또한 생략이 가능하니 이 또한 생략했다!

놀라운건 이를 한번 더 축약할 수 있다는 점이다.

```swift
firstVC.present(nextVC, animated: true) { print ("화면전환 완료")}
```

앞서 간단히 살펴봤던 `Trailing Closure`를 이용한 축약법이다.


<br>

## Completion Handler 디자인
---

이제는 위 예시 `present` 메소드처럼 디자인을 해보자.

일이 끝나면 사장님께 퇴근하겠다는 얘기를 전달하는 클로저를 작성해본다!

```swift

let handleBlock: (Bool) -> Void = { doneWork in 
 if doneWork {
     print("퇴근하겠습니다")
 }
}

handleBlock(true)       // 퇴근하겠습니다
```

이를 `$` 사인을 이용해 인자를 처리해보도록 한다.

```swift

let handleBlock: (Bool) -> Void = {
    if $0 {     // replace $0 = doneWork
        print("퇴근하겠습니다")
    }
}
```

<br>

## Completion Handler를 통한 데이터 전달
---

`Alamofire`와 같은 통신 라이브러리를 이용해 `JSON` 형태의 데이터를 받아올 떄 아래와 같은 코드 형태를 많이 살펴봤을 것이다.

```swift
Alamofire.request("https://google.com").responseJSON { response in
    print(response)
}
```

메서드의 `request`가 서버로 전송되어 이에 대한 결과값이 `response` 인자에 담겨 이를 클로저 내부에서 사용할 수 있게된다.

이렇듯 `Swift` 를 이용하다보면 함수의 마지막 인자로 `Closure`를 적용하는 경우를 심심치않게? 살펴볼 수 있다.

이는 함수가 종료된 직후(Called at the back)의 이벤트 처리를 위해 매우 많이 사용된다.

위 개념은 꼭 반복하여 익히도록 하자.

<br>

---
---
(내용 보충)

## Completion Handler가 필요한 이유
---

함수 내부에서 특정 task를 실행한 이후에 실행될 task가 있다고 가정해보자.

예를 들면, 데이터가 다운로드 된 이후 UI를 업데이트해야 한다고 했을때, task의 순서를 보장하며 순차적으로 실행되도록 하기 위한 방법이 무엇이 있을까?

`Completion Handler`는 위와 같은 작업을 가능하도록 해준다.

<br>

## Completion Handler 만들기
---

`Completion Handler`를 만들고 함수의 인자로 전달해본다.

```swift

let simpleCompletionHandler: () -> Void = {
    print("From the Completion Handler")
}
```

위와 같이 `() -> Void` 타입의 `Completion Handler`를 생성하였다.

이제 해당 타입의 `Completion Handler`를 인자로 받는 함수를 생성한다.

```swift

func takeCompletionHandler(completion: () -> Void) {
    print("From the Function Body")
    completion()
}
```

해당 함수에 인자로 생성한 completion handler를 전달하며 호출해본다.
```swift
takeCompletionHandler(completion: simpleCompletionHandler)

// From the Function Body
// From the Completion Handler
```

함수 body가 실행되면서 completion 또한 함께 호출되는 결과값을 확인할 수 있다.

다른 예시를 살펴보도록 한다.

서버로부터 데이터를 다운받고 다운로드가 완료되면 유저에게 notification 해주는 함수를 작성한다.

```swift
func download(success: Bool, completionHandler: (Bool) -> Void) {
    for _ in 0...5 {
        print("downloading data from server...")
    }

    completionHandler(success)
}
```

해당 함수는 `Bool` 타입의 인자와 `(Bool) -> Void` 타입의 인자를 전달받으며 다운로드가 완료되면 `completionHandler` 를 호출한다.

함수 호출시 전달할 클로저를 생성한다.

```swift

let completionHandler: (Bool) -> Void = { (success) in
    if success {
        print("Completed")
    } else {
        print("Failed")
    }
}

download(success: True, completionHandler: completionHandler)

// downloading data from server...
// downloading data from server...
// downloading data from server...
// downloading data from server...
// downloading data from server...
// Completed
```

위와 같이 download 함수를 호출하면서 전달한 인자값에 따라 분기 작업을 Completion Handler에서 처리할 수 있다.

또한, 클로저를 생성하지 않고 함수 호출과 동시에 Trailing Closure로 처리할 수도 있다.

```swift

download(success: True) { success in
    if success {
        print("Completed")
    } else {
        print("Failed")
    }
}
```

이렇게 Completion Handler를 이용하면 함수를 호출하여 함수 내부 로직 실행 이후 특정 작업을 이어서 진행할 수 있도록 이어줄 수 있다.

<br>

## 데이터 전달 Completion Handler
---
```swift
func passingData(completion: ([String]) -> Void) {
    for _ in 1...10 {
        print("Do some work")
    }
    completion(["Daily Swift", "Making List", "The Dreammer"])
}

passingData { (task) in
    print("Finish \(task[0])")
}
```

<br>

## Reference
- [Swift docs: Closures](https://docs.swift.org/swift-book/LanguageGuide/Closures.html)