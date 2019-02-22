자바스크립트 자료형
---

* 자료형 선언하지 않아도 됨
* 그래도 자료형은 존재한다
* 동적 자료형(dynamically Typed)과 약타입(weakly type)이 존재
* 정적 자료형은 아닌데 타입스크립트 사용해서 정적 자료형처럼 사용 가능 (여기서 정적 자료형이란 자료형이 강제되고 쉽게 바꿀 수 없다는 것을 의미)
* 동적 자료형은 런타임 시점에 변수의 자료형을 추론. 일단 코드가 실행되면 컴파일러나 인터프리터는 변수를 먼저 읽고 난 뒤 자료형을 결정함. 자료형이 강제되는 것은 같지만 자료형을 결정하는 시점이 다름. 다시 말해 자료형 결정은 자바스크립트 엔진이 함
* 약한 자료형 언어는 한번 정해진 자료형을 또다른 자료형으로 바꾸는 것이 가능

## 기본 자료형

* typeof 연산자로 데이터 타입 확인 가능
* 객체가 아니고 메소드도 가지지 않음
* 기본 자료형은 생성자 혹은 부모 객체를 가짐

* Boolean - true/false
* Null - value가 없음
* Undefined - 변수가 선언되었지만 아직 값할당이 안 됨
* Number - int, float 등..
* String - characters의 배열 (예를 들면 단어)
* Symbol - 다른 값들과 같지 않은 유니크한 값

### 'abc'.length
기본 자료형은 메소드를 가지지 않는다고 했는데, 위 코드는 정상적으로 작동이 됨. 왜냐면 기본 자료형은 생성자 혹은 부모 객체를 가지고 있어서 기본 자료형의 객체를 만들기 위해 성상자를 사용. 자바스크립트는 문자열 객체에 있는 메소드에 접근하기 위해 기본 자료형을 문자열 객체로 형변환 시도, 그리고 필요한 작업이 끝나면 임시로 형변환된 객체는 가비지 컬렉터에 의해 제거됨

```javascript
const str = 'a';
str instanceof String; // false
typeof str // 'string'
const arr = [1, 2, 3]
arr instanceof Array; // true
typeof arr; // object
```

## null은 객체다
결론적으로는 버그임. typeof에서 null일 경우를 처리해줘야 하는데 처리를 안 해줘서 그냥 객체라고 출력하는 것임. 

## NaN은 숫자다
NaN은 숫자형으로 정의되어있지만 숫자는 아니다. 그냥 스펙이 ECMAscript에 그렇게 정의되어있을 뿐...

# refs
* https://codeburst.io/javascript-essentials-types-data-structures-3ac039f9877b
