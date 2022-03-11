## 접근 제어
# 1. 접근 제어

코드끼리 상호작용을 할 때 파일 간 또는 모듈 간에 접근을 제한할 수 있는 기능. 코드의 상세 구현은 숨기고 허용된 기능만 사용하는 인터페이스를 제공할 수 있다.

![83492CAC-0B10-4C28-86E1-B7B1B04A0865.jpeg](83492CAC-0B10-4C28-86E1-B7B1B04A0865.jpeg)

**public**

: 프레임워크에서 외부와 연결된 인터페이스를 구현하는데 주로 쓰인다.

**open**

: public 이상의 접근 수준

- 모듈 밖에서도 상속이 가능(open only)
- 클래스 멤버를 모듈 밖에서 재정의 가능(open only)

**internal**

: default, 소스 파일이 속해 있는 모듈 어디에서든 이용 가능, 하지만 외부 모듈에서는 접근 불가하다.

**fileprivate**

: 그 요소가 구현된 소스파일 내부에서만 사용 가능

**private**

: 기능을 정의하고 구현한 범위 내에서만 사용 가능

*하지만 자신을 확장하는 익스텐션 코드가 같은 파일에 존재하는 경우에는 접근할 수 있다.*


👉 **global varialble/constant로 정의할 때에 fileprivate와 private는 같은 기능을 한다. (같은 파일 내에서만 접근 가능)**

[https://stackoverflow.com/questions/39739813/private-vs-fileprivate-on-declaring-global-variables-consts-in-swift3](https://stackoverflow.com/questions/39739813/private-vs-fileprivate-on-declaring-global-variables-consts-in-swift3)
It really makes no difference at file level, whether you use `private`
 of `fileprivate`
, access control will be the same, for example constants defined this way will be only usable in that file.



## 2. 읽기 전용

setter에게만 더 낮은 접근 수준을 부여하여 읽기 전용으로 만들 수 있다.

```swift
 접근수준(set)
```

프로퍼티, 서브스크립트, 변수 등에 적용 가능하며, 해당 요소의 접근수준보다 같거나 낮은 수준으로 제한해줘야 한다.

```swift
public struct SomeType {
	// 외부에서는 get만 이용 가능
	public private(set) var publicGetOnlyStoredProperty: Int = 0

	// 외부에서는 get만 이용 가능
	internal private(set) var internalGetOnlyComputedProperty: Int {
		get {
			return count
		}
		set {
			count += 1
		}
	}
}
```
