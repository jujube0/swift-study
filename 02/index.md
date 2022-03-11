# 2. 스위프트 시작하기

# 흐름 제어

## switch

- `fallthrough` 바로 다음 case 문을 실행한다.

### tuple

- wildcard `_`

    ```swift
    typealias NameAge = (name: String, age: Int)

    let tupleValue: NameAge = ("yagom", 99)

    switch tupleValue {
    case ("yagom", _):
        print("이름만 맞았습니다")
    default:
        print("누굴 찾나요?")
    }
    ```

- 값 바인딩

    ```swift
    typealias NameAge = (name: String, age: Int)

    let tupleValue: NameAge = ("yagom", 99)

    switch tupleValue {
    case (let name, 99):
        print("나이만 맞았습니다. 이름은 \(name)입니다")
    default:
        print("누굴 찾나요?")
    }
    ```

- where을 통한 확장

    ```swift
    let 직급: String = "사원"
    let 연차: Int = 1
    let 인턴인가: Bool = false

    switch 직급 {
    case "사원" where 인턴인가 == true:
        print("인턴입니다")
    default:
        print("누구십니까?)
    }
    ```

- 열거형처럼 한정된 범위의 값을 입력 값으로 받는 경우에는 default를 구현하지 않아도 된다.
- 차후에 열거형의 추가된 case를 해결해주는 것을 원한다면 `unknown` 속성을 사용할 수 있다.
- `@unknown case _` 를 이용하면, 남은 case를 대비해주면서 이에 대한 경고를 띄워준다.

## 반복문

- do-while 문은 repeat-while 문을 이용해야한다.

    ```swift
    var i = 3
    repeat {
    	print("bye)
    	i--
    } while i > 0
    ```

    - repeat 블록 코드를 1회 실행한 후 블록 내부 코드를 반복 실행한다.

## 구문 이름표

- 반복문 앞에 이름과 함께 colon을 붙여 구문의 이름을 지정
- 제어 키워드와 구문 이름을 이용하여 지정된 구문을 제어한다.

    ```swift
    var numbers: [Int] = [3, 2342, 6, 3252]

    numbersLoop: for num in numbers {
    if num > 5 || num < 1 {
        continue numbersLoop
    }
    var count = 0

    printLoop: while true {
        print(num)
        count += 1

        if count == num {
            break printLoop
        }
    }

    removeLoop: while true {
        if numbers.first != num {
            break numbersLoop
        }
        numbers.removeFirst()
    }
    }
    ```


# 함수

- 구조체, 클래스, 열거형 등 **특정 타입에 연관되어 사용하는 함수를 메서드**
- **모듈 전체에서 전역적으로 사용할 수 있는 함수를 그냥 함수**라고 부른다.

## 매개변수

- 가변 매개변수
    - 매개변수로 몇 개의 값이 들어올지 모를 때
    - 0개 이상의 값을 받아올 수 있으며
    - 인자 값은 배열처럼 사용할 수 있다.
    - 함수마다 가변 매개변수는 항상 하나만 가질 수 있다.

    ```swift
    func sayHelloToFriends(me: String, friends names: String...) {
        for friend in names {
            print("Hello \(friend)")
        }
        print("I'm \(me)")
    }

    sayHelloToFriends(me: "lauren", friends: "nylah", "terry", "river")
    ```

- 입출력 매개변수
    - 값이 아닌 참조를 전달할 때
    - `inout` 매개변수로 전달될 변수 또는 상수 앞에 `&` 를 붙여서 표현한다.

    ```swift
    var numbers: [Int] = [1,2,3]

    func referenceParameter(_ arr: inout [Int]) {
        arr[1] = 1
    }

    referenceParameter(&numbers)
    print(numbers[1])
    ```

    - default 값을 가질 수 없고, 가변 매개변수로도 사용될 수 없다.
    - memory safety 를 위협할 수 있다.
- 데이터 타입으로서의 함수
    - 전달인자 label은 함수 타입의 구성 요소가 아니므로 함수 타입을 작성할 때에는 이용할 수 없다.
    - `let someFunction: (lhs: Int, rhs: Int) -> Int` // 오류
    - `let someFunction: (_ lhs: Int, _ rhs: Int) -> Int` // OK
    - `let someFunction: (Int, Int) -> Int` // OK

## 중첩함수

- 특별한 위치에 들어가지 않은 함수는 모두 전역함수이다.
- 함수 내에 함수를 선언함으로써 사용 범위를 한정할 수도 있다.
- 중첩 함수를 반환하게 하여 함수 외부에서 이를 이용하게 할 수도 있다.

## 종료(return)되지 않는 함수

- nonreturning function(or method)
- `Never` 타입을 리턴하는 함수.
- 오류를 던지거나, 중대한 시스템 오류를 보고하는 등의 일을 한 후 프로세스를 종료한다.
- 이 함수를 실행하면 프로세스의 동작은 멈추게 된다.

## 반환 값을 무시할 수 있는 함수

- 반환값을 사용하지 않았다는 경고를 없애는 것
- `@discardableResult` 키워드를 함수 선언 시 사용한다.

# 옵셔널

: 값이 있을 수도, 없을 수(nil)도 있음

- 데이터 타입 뒤에 물음표를 붙여 표현해준다.

```swift
var myName: String? = "yagom"
```

## 옵셔널 enum의 정의

```swift
public enum Optional<Wrapped> : ExpressibleByNilLiteral {
		case none
		case some(Wrapped)
		public init(_ some: Wrapped)
		// 중략
}
```

→ enum이기 때문에 switch를 통해 값의 유무를 판단 가능하다.

```swift
switch optionalValue {
	case .none:
		//
	case .some(let value):
		//
}
```

## 옵셔널 언래핑

1. 강제추출

느낌표를 붙여주면 된다. 간단하지만 위험

1. 옵셔널 바인딩

if 또는 while 문과 결합하여 사용한다.

```swift
var myName: String? = "yagom"

if let name = myName {
	print("not optional)
}
```

1. 암시적 추출 옵셔널implicit unwrapped optional

nil을 할당 가능하지만, 이용할 때는 옵셔널이 아닌 것처럼 이용한다.

타입 뒤에 느낌표를 붙여주면 되고,

로직상 nil 때문에 런타임 오류가 일어나지 않을 것이 확실할 때만 이용해야 한다.

옵셔널이기 때문에 옵셔널 바인딩을 사용할 수는 있다.

```swift
var myName: String! = "yagom"
print(myName)
myName = nil

if let name = myName {
	print("not nil")
}
// not nil
myName.isEmpty // error
```
