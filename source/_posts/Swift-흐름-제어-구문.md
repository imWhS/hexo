---
title: Swift 흐름 제어 구문
date: 2020-11-17 08:36:37
categories: [프로그래밍 언어, Swift]
tags: [Swift, 프로그래밍 언어]
toc: true
widgets:
  - type: toc
    position: left
---
단순 구문: 변수 및 상수 등의 선언문, 연산자를 이용한 처리문, 함수 및 구조체 정의문 등

흐름 제어 구문: 반복문, 조건문, 제어 전달문 등

## for

```swift
for 루프 상수 in 순회 대상 {
	반복 실행할 구문
}
```

루프 상수: `for`을 이용해 구문이 반복될 때마다 순회 대상이 가지는 아이템들을 하나 씩 새로 넘겨 받아 구문 내에서 순회 대상 개별 아이템을 대표하는 상수를 선언하는 영역이다. 매 번 자동으로 상수로 재선언되기 때문에 `let` 키워드를 코드에 포함시킬 필요는 없다.

순회 대상: 구문이 반복될 때마다 루프 상수에 전달될 아이템들을 가지는 데이터로, 해당 데이터의 타입이 포함하고 있는 아이템 개수만큼 구문이 반복된다. 

순회 대상으로 쓸 수 있는 타입: 배열, 딕셔너리, 집합, 범위 연산자 데이터, 문자열 등 

```swift
var str = "Hello world"
for char in str.characters {
	print("str이 포함하는 개별 char: \(char)")
}
```

String 문자열 타입의 데이터를 순회 대상으로 사용할 경우 String의 characters 속성을 사용해 `char` 루프 상수로 Character 타입으로 넘겨주어야 한다. String 자체는 순회 처리를 지원하지 않기 때문이다.

루프 상수가 굳이 필요하지 않다면 생략의 의미로 `_`로 대체할 수도 있다.

## While

```swift
while 조건문 {
	반복 실행할 구문
}
```

#### repeat~while

```swift
repeat {
	반복 실행할 구문(단, 반드시 최소 한 번은 실행할 구문)
}
while 조건문
```

## if~else/else if

```swift
if 조건문 A {
	조건문 A가 참일 때 실행할 구문
} else if 조건문B { //필요한 경우
	조건문 B가 참일 때 실행할 구문
} else { //필요한 경우
	앞 if 조건문들이 모두 거짓일 때 실행할 구문
}
```

### guard~else

```swift
guard 조건문 else {
	조건문이 거짓일 때 실행할 구문
	return 혹은 break 조기 종료 구문
}
```

다음 코드들이 실행되기 전, 특정한 조건을 만족하지 않아 오류를 발생하는 경우를 방지하기 위해 사용하기 때문에 조건식이 `false`일 때에만 내부 구문이 실행된다.

그래서 반드시 블록 내부에 (특정한 조건을 만족하지 않아 다음 코드를 실행하지 못하게 조기 종료 시키는) `return`이나 `break` 구문이 포함되어야 한다.

함수/메서드에서 주로 사용된다. 

## switch

```swift
switch 비교 대상 {
	case 비교 패턴 A:
		비교 대상의 값이 비교 패턴 A와 같은 경우 실행할 구문
	case 비교 패턴 B:
		비교 대상의 값이 비교 패턴 B와 같은 경우 실행할 구문
	default:
		비교 대상의 값이 비교 패턴 A, 패턴 B 모두 해당하지 않을 경우 실행할 구문
}
```

기존 C언어, Java와 달리, 비교 패턴 A나 패턴 B와 일치해 해당 구문을 실행한 경우 이 외/아래 라인에 위치한 `case` 구문은 실행하지 않는다. 패턴 A, 패턴 B 모두 일치하더라도 더 상위 라인에 위치한 패턴 A에 대한 `case` 구문만 실행한 후 패턴 B에 대한 `case` 구문은 실행하지 않는다.

즉, 기존 대부분 언어의 `switch` 문과 달리 각 `case` 블록에 `break` 구문을 굳이 포함시킬 필요가 없다. 

Swift의 경우 반드시 `switch` 내 하나의 블록이라도 실행되어야 한다. 물론 단 하나의 비교 패턴과도 일치하지 않을 수도 있기 때문에, 이 경우를 대비해 `default` 구문을 추가한다.

### fallthrough

Swift는 각 `case` 블록을 비워두어선 안 되며, 특히 암시적 fallthrough를 지원하지 않는다. 그래서 처음 일치한 `case` 구문이 아닌, 다음 라인(이후)의 `case` 구문을 실행하고자 한다면 fallthrough 구문을 명시해야 한다.

암시적 fallthrough: 다른 언어의 경우 `break` 구문이 없다면 처음 비교 패턴이 일치한 `case` 구문부터 마지막 `default`까지 모두 실행된다.

### 튜플을 이용한 활용

```swift
var tup = (2, 3)
switch tup {
	case let (x, 3):
		print("tup의 두 번째 값 3과 일치합니다. 이 때 첫 번째 값은 \(x)입니다.");
		//실제 콘솔 출력: tup의 두 번째 값 3과 일치합니다. 이 때 첫 번째 값은 2입니다.
	case let (2, y):
		print("tup의 첫 번째 값 2와 일치합니다. 이 때 두 번째 값은 \(y)입니다.");
		//실제 콘솔 출력: tup의 첫 번째 값 2와 일치합니다. 이 때 두 번째 값은 3입니다.
	case let (x, y):
		print("tup의 첫 번째 값은 \(x), 두 번째 값은 \(y)입니다.");
		//실제 콘솔 출력: tup의 첫 번째 값은 2, 두 번째 값은 3입니다.
}
```

비교 대상으로 튜플을 사용할 경우, 비교 패턴의 튜플과 '일부 패턴'만 일치하더라도 같다고 판단해 해당 `case` 구문을 실행한다.

### where을 이용한 활용

```swift
var couple = (3, -3)
switch couple {
	case let (x, y) where x == y:
		print("\(x), \(y)의 값이 같습니다.")
	case let (x, y) where x == -y:
		print("\(x), \(y) 서로 절대값만 같습니다.")
	case let (x, y):
		print("\(x), \(y) 서로 값이 다릅니다.");
}
```

`switch` 문에서 임시로 사용되는 `x`, `y` 값에 대한 조건을 `where` 구문으로 추가할 수 있다.

---
**Reference**
* [꼼꼼한 재은 씨의 스위프트(Swift): 문법편](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791186710234&orderClick=JAH)
