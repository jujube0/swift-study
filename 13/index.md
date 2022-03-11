# 13. 클로저

세가지 형태

1. 이름이 있으면서 어떤 값도 획득하지 않는 전역함수의 형태
2. 이름이 있으면서 다른 함수 내부의 값을 획득할 수 있는 중첩된 함수의 형태
3. 이름이 없고 주변 문맥에 따라 값을 획득할 수 있는 축약 문법으로 작성된 형태

# 1. 기본 클로저

```swift
let reversed: [String] = names.sorted(by: {(first: String, second: String -> Bool in
	return first > second
})
```

# 2. 후행 클로저

함수/메서드의 마지막 전달인자에 넣을 클로저는 함수/메서드의 소괄호를 닫은 후 작성할 수 있다. 매개변수로 클로저가 여러개 전달되는 경우에는 맨 마지막 클로저만 후행 클로저로 사용 가능하고, 단 하나의 클로저만 전달하는 경우에는 소괄호를 생략해줄 수도 있다.

```swift
let reversed: [String] = names.sorted() {(first: String, second: String -> Bool in
	return first > second
}
// 소괄호 생략하는 경우

let reversed: [String] = names.sorted {(first: String, second: String -> Bool in
	return first > second
}
```

# 3. 간소화된 클로저

**매개변수의 타입이나 반환값의 타입 생략**

→ 문맥을 통해 유추한다.

```swift
let reversed = names.sorted { (first, second) in
	return first > second
}
```

**매개변수 이름 생략**

→ 첫 번째 전달인자부터 $0, $1 로 접근할 수 있다. 이 경우, in도 생략 가능함

```swift
let reversed = names.sorted {
	return $0 > $1
}
```

**return 키워드 생략**

→ 클로저 내부 실행문이 한 줄이라면 해당 실행문을 반환값으로 사용 가능

```swift
let reversed = names.sorted { $0 > $1 }
```

**연산자 이용하기**

→ 연산자를 그대료 이용할 수도 있다. 매개변수의 반환타입과 연산자를 구현한 함수의 모양이 동일할 때 가능하다.

```swift
let reversed = names.sorted(by: >)
```

# 4. capturing value

클로저 외부에서 정의한 상수나 변수를 이용할 경우, 해당 상수나 변수가 더 이상 존재하지 않는 경우에도 자신 내부에서 참조하거나 수정할 수 있다. 클로저는 비동기 콜백에 많이 이용되는데, 이러한 경우에 클로저가 실행되는 시기에는 참조한 상수나 변수가 존재하지 않을 수 있기 떄문

**참조 타입**

함수와 클로저는 참조 타입이다.

# 5. 탈출 클로저

함수의 전달인자로 전달한 클로저가 함수 종료 후에 호출될 때, 클로저가 함수를 **탈출**한다고 표현한다.

이 경우, 매개변수 이름의 콜론(:) 뒤에 *@escaping* 키워드를 사용하여 클로저가 탈출하는 것을 허용한다고 명시해줘야한다.

예를 들어, 비동기 작업을 실행하는 함수가 completionHandler로 클로저를 받을 때, 해당 클로저는 비동기 작업으로 함수가 종료되고 난 후 호출되므로 탈출 클로저가 된다. 이러한 경우에는 *@escaping* 키워드를 이용해야 한다. 또한, 전달인자로 받은 클로저를 다시 반환하거나, 함수 외부의 변수에 저장되는 경우 탈출 클로저를 이용해야 한다.

*@escaping* 키워드를 따로 명시하지 않는 경우에는 비탈출 클로저가 된다. 함수로 전달된 클로저가 함수의 동작이 끝난 후에는 사용할 필요가 없을 때, 이러한 비탈출 클로저를 이용한다.

```swift
// 전달인자로 받은 클로저를 함수 외부의 변수에 저장하는 경우
var closures: [Closure] = []

func appendClosure(closure: @escaping Closure) {
	closures.append(closure)
}
```

비탈출 키워등서는 클로저 내부에서 타입 외부의 프로퍼티, 메서드, 서브스크립트 등에 접근할 때 *self* 키워드를 써주지 않아도 되지만 탈출 클로저에서는 명시적으로 *self* 를 써줘야한다.

**withoutActuallyEscaping**

**비탈출 클로저로 전달한 클로저가 탈출 클로저인 척 해야 하는 경우?** 비탈출 클로저를 다른 함수의 탈출 클로저 매개변수로 이용하고 싶을 때

**withoutActuallyEscaping(_:do:)** 를 이용할 수 있다. 비탈출 클로저를 임시로 탈출 클로저로 이용하게 하는 함수로, 첫 번째 매개변수로 탈출 클로저로 이용하고 싶은 비탈출 클로저를 넣는다. 두 번째 클로저는 첫 번째 매개변수를 탈출 클로저로서 이용 가능한 클로저이다. 매개변수로 탈출 클로저를 받는다. 두 번째 do에서 값을 리턴하면 withoutActuallyEscaping에서 이를 그대로 리턴하게 된다.

# 자동 클로저 Auto Closure

expression을 자동으로 매개변수가 없는 클로저로 만들어준다. 그리고 호출될 경우 해당 expression의 결과값을 리턴하게 된다.

클로저가 호출되기 전까지는 내부 expression이 샐행되지 않으므로 연산이 자동으로 지연된다.

```swift
func giveMeClosure(_ closure: () -> String) {
	//
}
```

위처럼 클로저를 매개변수로 받는 함수가 있다고 하자.

```swift
var stringArray = ["a", "b", "c"]
```

그리고, string으로 된 배열도 있다. 해당 배열에서 첫번째 값을 리턴하고, 리턴 후에는 이를 제거하는 클로저를 giveMeClosure의 매개변수로 넣어보자.

첫번째 값을 리턴하고, 제거하는 함수는 배열의 기본 함수 `removeFirst()` 를 이용하면 된다. 하지만 우리가 원하는 건 클로저다. array.removeFirst() 가 리턴하는 것은 String이지 클로저는 아니다. 그렇기 때문에 실제로 이용할 때는 다음과 같이 이용한다.

```swift
giveMeClosure({ stringArray.removeFirst() })
```

중괄호를 통해 Void → String 타입의 클로저를 생성해줬다. autoclosure 키워드는 여기에서 중괄호를 생략할 수 있게 해준다.

```swift
func giveMeAutoClosure(_ closure: @autoclosure () -> String) {
	//
}
giveMeAutoClosure(stringArray.removeFirst())
```

위와 같이 함수를 정의하면 클로저를 직접 만들어줄 필요 없이 리턴 값만 넣어주면 내부에서 알아서 클로저로 만들어준다. giveMeAutoClosure 함수 내부에서는 실제로 클로저처럼 이용해주면 된다.
