# 1. 스위프트 기초

# 함수형 프로그래밍 패러다임

## → 함수가 일급 객체이다.

- 함수를 argument로 전달 가능하고
- 동적 프로퍼티 할당이 가능하고
- 변수나 데이터 구조에 담을 수 있고,
- 반환 값으로 사용 가능하고
- 이름과 관계없이 고유한 객체로 구별 가능

## 함수형 프로그래밍 패러다임의 장점

- 여러 가지 연산 처리 작업이 동시에 일어나는 프로그램을 만들기 쉽다
- 멀티 코어 혹은 여러 개의 연산 프로세서를 사용하는 시스템에서 효율적인 프로그램을 만들기 쉽다.
- 상태변화에 따른 부작용에서 자유로워지므로 순수하게 기능 구현에만 초점을 맞추어 개발할 수 있다.

# 프로토콜 지향

- 참조 타입인 클래스 인스턴스보다는 value-type을 더 효율적으로 사용

# 기본 명명 규칙

- 함수, 메서드, 인스턴스의 이름은 lower-camel-case를 사용한다
- 클래스, 구조체, 익스텐션, 프로토콜, 열거형 이름은 타입의 이름이기 때문에 첫 글자를 대문자로 사용하는 upper-camel-case를 사용

# 콘솔 로그

## print()

```swift
public func print(items: Any...,
	seperator: String = default,
	terminator: String = default)
```

- 자동으로 줄바꿈 문자를 마지막에 삽입해준다.

## dump()

- print보다 자세한 내용을 출력해준다.
- 인스턴스 내부의 콘텐츠까지 함께 출력한다.

## 문자열 보간법String interpolation

```swift
"My Name Is \(var or let)"
```

- 기본적으로 인스턴스의 description 프로퍼티를 사용하여 문자열로 치환한다.
- CustomStringConvertible 플고토콜을 준수할 때 구현하면 된다.

## 변수와 상수

- 변수: var
    - 생성 후 값을 변경할 수 있다
- 상수: let
    - 생성 후 값을 변경 불가하다.

# 데이터 타입 기본

## Int와 UInt

- Int는 +, - 부호를 포함한 정수
- UInt는 양을 포함한 양의 정수 only
- 비트에 따라 Int8, Int16..Int64, UInt8, UInt16...UInt64
- 플랫폼이 64비트 환경이면 Int64가 Int 타입이 된다.
- 같은 정수라 해도 Int, UInt가 전혀 다른 타입으로 인식하기 때문에 두 타입간 교환에서 자원이 소모된다.
- → 통일하는 것이 좋다.

### 진수

- 2진수: 접두어 `0b`
- 8진수: 접두어 `0o`
- 16진수: 접두어 `0x`

```swift
var binaryInteger: Int = 0b11100
print(binaryInteger) // 28
```

## Bool

## Float과 Double

- 64비트 Double, 32비트 Float
- 64비트 환경에서 Double은 최소 15자리의 십진수를 표현 가능, Float은 6자리만 가능
- 헷갈리면 Double

## Character

## String

- character의 나열

```swift
let greeting = """
안녕하세요!
잘하고 싶어요 스위프트
"""
```

- 큰따옴표 세개를 사용해서 여러 줄의 문자열을 표현할 수 있다. 이 경우 큰 따옴표를 쓰고 한 줄을 내려쓰고, 마지막 줄에도 한 줄 내려서 큰 따옴표를 써줘야한다.

## 특수문자

- `\n` 줄바꿈
- `\\` 문자열 내에서 백슬래시
- `\"`
- `\t` 탭 문자
- `\0` 문자열이 끝났음을 알리는 null 문자

## Any, AnyObject와 nil

- Any
    - 모든 데이터 타입이 가능하다.
- AnyObject
    - Any보다는 조금 한정된 의미
    - 클래스의 인스턴스만 할당 가능하다.

# 데이터 타입 고급

## typealias

```swift
typealias MyInt = Int
typealias YourInt = Int
```

- 모든 데이터 타입에 임의로 다른 이름을 부여할 수 있다.

## tuple

- 지정된 데이터의 묶음
- 파이썬의 튜플과 유사하다

```swift
var person: (String, Int, Double) = ("yagom", 100, 182.5)

print(person.0)

person.1 = 99
```

- 인덱스를 통해 값을 할당할 수도 있고
- 값을 가져올 수도 있다.
- 각 요소에 이름을 지어줄 수도 있다.

    ```swift
    var person: (name: String, age: Int, height: Double) = ("yagom", 100, 182.5)

    print(person.name)

    print(person.0)
    person.2 = 178.1
    ```

    - 이 경우 인덱스와 요소 이름을 통해 각 요소에 접근하고, 값을 할당할 수 있다.
- typealias와 함께 이용하면 좋다.

## collections

- array, dictionary, set
- array
    - `let` 을 사용하면 immutable, `var` 을 사용하면 mutable
    - `Array<Type>` 또는 `[Type]` 으로 선언 가능하다.

    ```swift
    var emptyArray: [Any] = [Any]()
    var emptyArray: [Any] = Array<Any>()
    var emptyArray: [Any] = []
    ```

    - 잘못된 인덱스에 접근하면 exception error가 발생한다.
- dictionary
    - 순서 없는 key-value 쌍
    - `let` 사용하면 immutable, `var` 사용하면 mutable
    - typealias 사용하면 좋다.

    ```swift
    var numberForName: Dictionary<String, Int> = Dictionary<String, Int>()
    var numberForName: [String: Int] = [String: Int]()

    typealias StringDictionary = [String: Int]
    var numberForName: StringDictionary = StringDictionary()

    var numberForName: [String: Int] = [:]
    ```

    - 키는 유일해야 하고, 값은 상관 없다.
    - 내부에 없는 키로 접근하면 nil을 반환한다. *에러가 생기지 않는다.
- set
    - 같은 타입의 데이터를 순서 없이 하나의 묶음으로 저장
    - 세트 요소로는 Hashable 프로토콜을 따르는 값들만 가능하다. 기본 데이터타입은 모두 Hashable이다.

    ```swift
    var names: Set<String> = Set<String>()
    var names: Set<String> = []

    var names: Set<String> = ["yagom", "chulsoo", "yagom"]
    print(names.count) // 2
    ```

- enums
    - 연관된 항목들을 묶어서 표현

    ```swift
    enum School {
    	case primary, elementary,
    	case middle
    }
    ```

    - 한 줄에 나열할 수도 있고, 여러줄에 하나씩 나열할 수도 있다.
    - 각 항목 자체로도 하나의 값이지만, 각 항목에 대응되는 raw value를 가질 수도 있다.
    - `rawValue` 프로퍼티를 통해 접근하자

        ```swift
        enum School: String {
        	case primary = "유치원"
        	case elementary = "초등학교"
        }

        let highestEducationLevel: School = .primary
        print(highestEducationLevel.rawValue)
        ```

    - 일부 항목만 원시 값을 줄 수도 있다.
        - 나머지는 문자열 형식의 원시값을 지정해줬다면 각 항목 이름을 그대로 원시 값으로 갖게되고
        - 정수타입이라면 첫 항목을 기준으로 0부터 1씩 늘어난 값을 갖게된다.
    - 원시값을 통해 enum 변수/상수를 생성해줄 수도 있다. 올바르지 않은 원시값을 이용하면 nil을 반환한다.

        ```swift
        let primary = School(rawValue: "유치원") //primary
        let graduate = School(rawValue: "석박사") // nil
        ```

    - associated values
        - 열거 형 내의 항목(case)이 자신과 연관된 값을 가질 수 있다.
        - 일부만 연관값을 가져도 된다.

        ```swift
        enum MainDish {
        	case pasta(taste: String)
        	case pizza(dough: String, topping: String)
        	case rice
        }

        var dinner: MainDish = .pasta(tase: "크림)
        dinner = .rice
        ```

    - `CaseInterable` 프로토콜을 채택할 경우 `allCases` 라는 타입 프로퍼티를 통해 모든 케이스의 컬렉션을 생성해준다.
        - 플랫폼에 따라 케이스가 달라지거나 associated value를 갖는 경우에는 `allCases` 프로퍼티를 직접 구현해준다.
    - 열거형 항목의 연관 값으로 self(enum)을 사용하고 싶을 때에는 `indirect` 키워드를 사용한다.
        - 특정 항목에만 사용하고 싶은 경우에는 case 키워드 앞에
        - 전체에 적용하고 싶으면 enum 키워드 앞에

# 연산자

## 사용자 정의 연산자

```swift
prefix operator ** // **를 전위 연산자로 정의

prefix func ** (value: Int) -> Int {
	return value * value
}

print(**(-5)) // 25
```
