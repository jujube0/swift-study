# 10.프로퍼티와 메서드

# 프로퍼티

프로퍼티는 크게

- 저장 프로퍼티**stored property**
- 연산 프로퍼티**computed property**
- 타입 프로퍼티**type property**

로 나눌 수 있다.

타입 프로퍼티 → 인스턴스가 아니라 특정 타입에 사용되는 프로퍼티

## 1. stored property

`var` 은 변수 저장 프로퍼티, `let` 은 상수 저장 프로퍼티.

클래스 또는 구조체의 **인스턴스**와 연관된 값을 저장한다.

- 지연 저장 프로퍼티 **lazy stored properties**

호출이 있을 때 값을 초기화한다.

불필요한 성능저하나 공간 낭비를 줄일 수 있다.

lazy 키워드를 사용하여 정의하면 된다.

다중 스레드 환경에서는 한 번만 초기화된다는 보장이 없다. → 여러 번 초기화될 수 있음.

## 2. computed property

실제로 값을 저장하지는 않고 인스턴스 내/외부의 값을 연산하여 적절한 값을 돌려주는 getter의 역할이나 은닉화된 내부의 프로퍼티 값을 간접적으로 바꿔주는 setter의 역할을 한다.

```swift
struct CoordinatePoint {
	var x: Int
	var y: Int

	var oppositePoint: CoordinatePoint {
		get {
			return CoordinatePoint(x: -x, y: -y)
		}
		set(opposite) {
			x = -opposite.x
			y = -opposite.y
		}
	}
}
```

소괄호 안에 매개변수 이름을 명시(opposite)하여 이용하거나, 매개변수 표현 없이 newValue라는 변수로 바로 이용할 수도 있다.

```swift
set {
	x = -newValue.x
	y = -newValue.y
}
```

## 3. Property Observers

프로퍼티 값이 새로 할당될때마다 호출된다.

lazy stored property가 아닌 stored property에서만 사용 가능하다.

override를 통해 상속받은 stored/computed property 에도 적용 가능하다.

computed property는 상속 받았을 떄에만 프로퍼트 override로 적용 가능하다.

```swift
class Account {
    var credit: Int = 0 {
        willSet {
            print("start setting")
        }
        didSet {
            print("done setting")
        }
    }
}
```

willSet에서는 newValue, didSet에서는 oldSet 을 적용 가능하다.

## 4. 전역변수와 지역변수

global variables → 함수, 메서드, 클로져, 타입 밖에서 선언된 변수

local variables → 함수, 메서드, 클로져 내에서 선언된 변수

전역 변수, 전역 상수는 lazy stored property처럼 처음 접근할 때 연산이 이루어진다.

반대로 지역번수와 지역 상수는 절대로 지연 연산되지 않는다.

## 5. 타입 프로퍼티

인스턴스 프로퍼티는 인스턴스를 새로 생성할 때마다 초기값으로 해당 프로퍼티를 처음 생성하고, 인스턴스마다 다른 값을 가질 수 있게된다.

인스턴스가 아닌 타입 자체에 속하는 프로퍼티를 타입 프로퍼티라고 한다. 인스턴스의 생성 여부와 상관 없이 타입 프로퍼티의 값은 하나이다.

두 가지 종류가 있다.

1. stored property
2. computed property

stored type property는 반드시 초기값을 설정해야하고, 지연 연산된다. 하지만 lazy stored property와 다르게, 다중 스레드 환경에서도 한 번만 초기화된다는 보장을 받는다. lazy 키워드를 붙일 필요도 없다.

## 6. 키 경로

```swift
\타입이름.경로.경로.경로
```

```swift
struct Stuff {
	var name: String
	var owner: Person
}

let keyPath = \Stuff.owner
let nameKeyPath = keyPath.appending(path: \.name)

let macbook = Stuff(name: "Macbook Pro", owner: "lauren")
print(macbook[keyPath: nameKeyPath]) // Macbook Pro
```

→ 타입 간의 의존성을 낮출 수 있다.

→ 자기 자신은 `\.self`

# 메서드

특정 타입에 관련된 함수

인스턴스 메서드 또는 타입 메서드로 정의할 수 있다.

구조체 열거형도 메서드를 가질 수 있다.

## 1. 인스턴스 메서드

특정 타입의 인스턴스에 속한 함수

인스턴스가 존재할 때에만 사용할 수 있다.

<aside>
💡 특정 타입에 속한 함수를 메서드라고 하고,
전역적으로 사용 가능하게 정의되면 함수라고 한다.

</aside>

**self property**

모든 인스턴스는 암시적으로 생성된 self 프로퍼티를 갖는다.

## 2. 타입 메서드

메서드 앞에 `static` 키워드를 추가해주면 된다.

클래승의 타입 메서드에서는 `static` 또는 `class` 키워드를 사용할 수 있다. `static` 으로 정의하면 상속 후 메서드 재정의가 불가하고, class로 정의하면 가능하다.

타입 메서드에서는 인스턴스 메서드와 달리 self 프로퍼티가 타입 그 자체를 가리킨다.
