# chapter 10. 제네릭

- TS는 제네릭을 사용해 타입 간의 관계를 알아냄
- TS에서 함수와 같은 구조체는 제네릭 타입 매개변수를 원하는 만큼 선언 가능
- 제네릭 타입 매개변수는 제네릭 구조체의 각 사용법에 따라 타입이 결정
  → 매개변수는 구조체의 각 인스턴스에서 서로 다른 일부 타입을 나타내기 위해 구조체의 타입으로 사용

### 타입 매개 변수

- 구조체의 각 인스턴스에 대해 타입 인수라고 하는 서로 다른 타입 제공이 가능하지만, 타입 인수가 제공된 인스턴스 내에서는 일관성을 유지
- 전형적으로 `T`와 `U` 같은 단일 문자 이름 또는 `key`와 `Value` 같은 파스칼 케이스 이름을 갖음
- 모든 구조체에서는 `<`,`>`를 사용해 `someFunction<T>` 또는 `SomeInterface<T>`처럼 제네릭 선언

# 제네릭 함수

- 매개변수 괄호 바로 앞에 `<`,`>`로 묶인 타입 매개변수에 별칭을 배치해 함수를 제네릭으로 만듦
  → 해당 타입 매개변수를 함수의 본문 내부의 매개변수 타입 애너테이션, 반환 타입 애너테이션, 타입 애너테이션에서 사용 가능
  ```tsx
  function v<T>(input: T) {
    return input;
  }

  const st = v("hi"); //타입: hi
  const num = v(123); //타입: 123

  //화살표 함수 사용하기
  const v = <T,>(input: T) => input;

  v(123);
  ```

## 명시적 제네릭 호출 타입

- 제네릭 함수를 호출 할 때 대부분의 TS는 함수가 호출되는 방식에 따라 타입 인수를 유추
- 타입 인수를 해석하기 위해 TS에 알려줘야 하는 함수 호출 정보가 충분하지 않을 수 있음
  → 타입 인수를 알 수 없는 제네릭 구문이 다른 제네릭 구문에 제공된 경우 주로 발생
- 기본값이 `unknown`으로 설정되는 것을 피하기 위해 TS에 해당 타입 인수가 무엇인지 명시적으로 알려주는 명시적 제네릭 타입 인수를 사용해 함수 호출 가능
- TS는 매개변수가 타입 인수로 제공된 것과 일치하는지 확인하기 위해 제네릭 호출에서 타입 검사를 수행

## 다중 함수 타입 매개변수

- 임의의 수의 타입 매개변수를 쉼표로 구분해 함수 정의
- 제네릭 함수의 각 호출은 각 타입 매개변수에 대한 자체 값 집합을 확인 가능
- 함수가 여러 개의 타입 매개변수를 선언하면 해당 함수에 대한 호출은 명시적으로 제네릭 타입을 모두 선언하지 않거나 모두 선언해야 함

# 제네릭 인터페이스

- 인터페이스는 함수와 유사한 제네릭 규칙을 따르며 인터페이스 이름 뒤 `<`,`>` 사이에 선언된 임의의 수의 타입 매개변수를 갖음
  → 해당 제네릭 타입은 나중에 멤버 타입과 같이 선언의 다른 곳에서 사용 가능

```tsx
interface v<T> {
  input: T;
}

let stV: v<string> = {
  input: "hi",
};

let stV: v<number> = {
  input: 123,
};
```

- 내장 Array 메서드는 제네릭 인터페이스로 정의됨

## 유추된 제네릭 인터페이스 타입

- 선언된 위치에서 제공된 값의 타입에서 타입 인수를 유추
- 인터페이스가 타입 매개변수를 선언하는 경우, 해당 인터페이스를 참조하는 모든 타입 애너테이션은 이에 맞는 타입 인수를 제공해야 함

# 제네릭 클래스

- 클래스의 각 인스턴스는 타입 매개변수로 각자 다른 타입 인수 집합을 가짐
- 제네릭 인터페이스와 마찬가지로 클래스를 사용하는 타입 애너테이션은 해당 클래스의 제네릭 타입이 무엇인지를 TS에 나타내야 함

## 명시적 제네릭 클래스 타입

- 제네릭 클래스 인스턴스화는 제네릭 함수를 호출하는 것과 동일한 타입 인수 유추 규칙을 따름
- 함수 생성자에 전달된 매개변수의 타입으로부터 타입 인수를 유추 가능하다면 유추된 값 사용
- 생성자에 전달된 인수에서 클래스 타입 인수를 유추 할 수 없는 경우 타입 인수의 기본값은 `unknown`
- 클래스 인스턴스는 다른 제네릭 함수 호출과 동일한 방식으로 명시적 타입 인수를 제공해 기본값 `unknown`이 되는 것을 피할 수 있음

## 제네릭 클래스 확장

- `extends` 키워드 다음에 오는 기본 클래스로 사용 가능
- 기본값이 없는 모든 타입 인수는 명시적 타입 애너테이션을 사용하여 지정해야 함
- 제네릭 파생 클래스는 자체 타입 인수를 기본 클래스에 번갈아 전달 가능

## 제네릭 인터페이스 구현

- 제네렉 기본 클래스를 확장하는 것과 유사하게 작동
- 기본 인터페이스의 모든 타입 매개변수는 클래스에 선언되어야 함

## 메서드 제네릭

- 클래스 메서드는 클래스 인스턴스와 별개로 자체 제네릭 타입 선언 가능
- 제네릭 클래스 메서드에 대한 각각의 호출은 각 타입 매개변수에 대해 다른 타입 인수를 갖음

## 정적 클래스 제네릭

- 클래스의 정적 멤버는 인스턴스 멤버와 구별되고 클래스의 특정 인스턴스와 연결되어 있지 않음
- 클래스의 정적 멤버는 클래스 인스턴스에 접근할 수 없거나 타입 정보를 지정할 수 없음
  → 정적 클래스 메서드는 자체 타입 매개변수를 선언할 수 있지만 클래스에 선언된 어떤 타입 매개변수에도 접근 불가

# 제네릭 타입 별칭

- 타입 인수를 사용해 제네릭을 만드는 TS의 마지막 구조체
- 각 타입 별칭에는 `T`를 받는 `Nullish` 타입과 같은 임의의 수의 타입 매개변수가 주어짐

```tsx
type Nullish<T> = T | null | undefined;
```

- 일반적으로 제네릭 함수의 타입을 설명하는 함수와 함께 사용

## 제네릭 판별된 유니언

- 제네릭 타입과 판별된 타입을 함께 사용하면 재사용 가능한 타입을 모델링 가능

# 제네릭 제한자

- 제네릭 타입 매개변수의 동작을 수정하는 구문 제공

## 제네릭 기본값

- 타입 매개변수 선언 뒤에 = 와 기본 타입을 배치해 타입 인수를 명시적으로 제공 가능
- 기본 값은 타입 인수가 명시적으로 선언되지 않고 유추할 수 없는 모든 후속 타입에 사용됨
- 타입 매개변수는 동일한 선언 안의 앞선 타입 매개변수를 기본값으로 가질 수 있음
- 각 타입 매개변수는 선언에 대한 새로운 타입을 도입하므로 해당 선언 이후의 타입 매개변수에 대한 기본값으로 이를 사용 가능
- 모든 기본 타입 매개변수는 기본 함수 매개변수처럼 선언 목록의 제일 마지막에 와야 함

# 제한된 제네릭 타입

- 제네릭 타입에는 클래스, 인터페이스, 원싯값, 유니언, 별칭 등 모든 타입을 제공할 수 있음
- TS는 타입 매개변수가 타입을 확장해야 한다고 선언할 수 있으며 별칭 타입에만 허용
- 타입 매개변수를 제한하는 구문은 매개변수 이름 뒤에 `extends` 키워드를 배치하고 그 뒤에 이를 제한할 타입 배치

## keyof와 제한된 타입 매개변수

- `extends`와 `keyof`를 함께 사용하면 타입 매개변수를 이전 타입 매개변수의 키로 제한 가능
  → 제네릭 타입의 키를 지정하는 유일한 방법
- 구체적이지 않은 함수 선언은 각 호출이 타입 인수를 통해 특정 key를 가질 수 있음을 TS에 나타내지 못함
- 제네릭 함수 작성 시 매개변수의 타입이 이전 매개변수 타입에 따라 달라지는 경우 올바른 매개변수 타입을 위해 제한된 타입 매개변수를 자주 사용함

# Promise

## JS에서의 Promise

- 네트워크 요청과 같이 요청 이후 결과를 받기까지 대기가 필요한 것을 나타냄
- 각 Promise는 대기 중인 작업이 `resolve`또는 `reject`하는 경우 콜백을 등록하기 위한 메서드 제공

## TS에서의 Promise

- Promise는 TS 타입 시스템에서 최종적으로 `resolve`된 값을 나타내는 단일 타입 매개변수를 가진 Promise 클래스로 표현

### Promise 생성

- 단일 매개변수로 받도록 작성
- 해당 매개변수의 타입은 제네릭 Promise 클래스에 선언된 타입 매개변수에 의존
- 결과적으로 값을 resolve하려는 Promise를 만들려면 Promise의 타입 인수를 명시적으로 선언해야 함
- TS는 명시적 제네릭 타입 인수가 없다면 기본적으로 매개변수 타입을 `unknown`으로 가정
- Promise 생성자의 타입 인수를 명시적으로 제공 시 TS가 결과로서 생기는 Promise 인스턴스의 `resolve`된 타입 이해
- Promise의 제네릭 `.then` 메서드는 반환되는 Promise의 `resolve`된 값을 나타내는 새로운 타입 매개변수를 받음

### async

- JS에서 `async` 키워드를 사용해 선언한 모든 함수는 Promise로 반환
- Promise를 명시적으로 언급하지 않아도 `async` 함수에서 수동으로 선언된 반환타입은 항상 Promise 타입이 됨

# 제네릭 올바르게 사용하기

- 제네릭 코드에서 타입을 설명하는데 많은 유연성을 제공하지만 코드가 복잡해질 수 있음

## 제네릭 황금률

- 함수에 타입 매개변수가 필요한지 여부를 판단할 수 있는 방법은 타입 매개변수가 최소 두번 이상 사용되었는지 확인하기
- 제네릭은 타입 간의 관계를 설명하므로 제네릭 타입 매개변수가 한 곳에만 나타나면 여러 타입 간의 관계 정의 불가
  → 각 함수 타입 매개변수는 매개변수에 사용되어야 하고, 적어도 하나의 다른 매개변수 또는 함수의 반환 타입에서도 사용되어야 함

## 제네릭 명명 규칙

- 첫 번째 타입 인수를 `T`로 사용하고, 후속 타입 매개변수가 존재하면 `U`, `V` 등을 사용
- 타입 인수가 어떻게 사용되어야 하는지 맥락과 관련된 정보가 알려진 경우 명명 규칙은 경우에 따라 해당 용어의 첫 글자를 사용하는 것으로 확장됨
- 구조체가 여러 개의 타입 매개변수를 갖거나 단일 타입 인수의 목적이 명확하지 않을 때마다 단일 문자 약어 대신 가독성을 위해 오나전히 작성된 이름을 사용하는 것이 좋음
