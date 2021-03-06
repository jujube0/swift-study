# 맵

매개변수로 전달된 함수를 실행하여 그 결과를 다시 반환해주는 함수이다. 배열, 딕셔너리, 세트, 옵셔널 등에서 사용 가능하다.

for-in 구문도 같은 역할을 할 수 있지만, 다중 스레드 환경일 경우 다른 스레드에서 동시에 값이 변경되는 상황을 방지할 수 있다.

# Reduce

컨테이너 내부의 콘텐츠를 하나로 합하는 기능을 실행한다.

두 가지 형태로 구현되어 있다.

1. 클로저가 각 요소를 전달받아 연산한 후 값을 다음 클로저 실행을 위해 반환하며 컨테이너를 순환

```swift
func reduce<Result>(_ initialResult: Result,
_ nextPartialResult: (Result, Self.Element) throws -> Result)
rethrows -> Result
```

initialResult 매개변수에 초깃값을 지정하고, nextPartialResult로 클로저를 전달받는다. nextPartialClosure는 첫번째 매개변수로는 초깃값 또는 이전 클로저의 결괏값을 받고, 두 번째 매개변수는 리듀스 메서드가 순환하는 컨테이너의 요소 하나이다.

1. 역시 컨테이너를 순환하지만 클로저가 따로 결과값을 반환하지는 않고, *inout* 매개변수를 사용하여 초깃값에 직접 연산을 실행하게 된다.

```swift
func reduce<Result>(into initialResult: Result,
_ updateAccumulatingResult: (inout Result, Self.Element)
throws -> ()) rethrows -> Result
```
