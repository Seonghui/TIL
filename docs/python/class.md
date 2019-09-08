# class

## class

* 클래스 이름은 CamelCase로
* 객체 생성할 때 new 없이 이렇게 `a = FourCal()`

## 메소드 작성하기 예시

```text
class Flight:
    def number(self):
        return 'SN060'
```

## 생성자와 초기화자\(**init**\)

```text
class Complex:
    def __init__(self, realpart, imagpart):
        self.r = realpart
        self.i = imagpart

x = Complex(3.0, -4.5)
x.r, x.i
```

생성자로 객체생성을 호출받으면 먼저 **new** 를 호출하여 객체를 생성할당하고, **new** 메소드가 **init**메소드를 호출하여 객체에서 사용할 초기값들을 초기화

self 말고도, 인자들을 더 가질 수 있음.

**init**메소드는 생성자가 아니다. 왜냐면 **init**메소드가 호출될 때 object는 이미 constructed

### **init**과 C++의 생성자

파이썬에서 새 객체는 **init**메소드가 실행되기 전 has no attribute

C++에서 새 객체는 생성자가 실행될 때 이미 declared fields를 가지고 있지만, garbage를 포함하고 있음.

자바에서 새 객체는 생성자가 실행될 때 이미 초기값과 declared fields를 가지고 있음.

아무튼 장고의 **init**과 C++/Java의 생성자는 위와 같은 차이를 가지고 있음. 그래서 겉보기에는 c++와 Java의 생성자와 비슷함. 둘다 in-creation 객체를 첫번째 인자\(python에서는 self, c++에서는 this\)로 받음. 그리고 생성자와 **init**은 아무것도 return하지 않음.

하지만 둘의 차이점은, **init**은 상속받은 클래스인 **init**로부터 불려져야 되고, C++은 내재적이고 자동적임. \(뭔소리?\) 다시말해 파이썬에서 클래스를 만들 때 **init**메소드만 오버라이딩해서 객체 초기화에만 이용.

어쨌든, **init**은 생성자가 아니다. initializer임. 그런데 몇몇 튜토리얼에서는 생성자라고 하고.. 모르게따 [참고 링크1](https://stackoverflow.com/questions/6578487/init-as-a-constructor) [참고 링크2](https://stackoverflow.com/questions/8985806/python-constructors-and-init)

## self

```text
def setdata(self, first, second):   # ① 메서드의 매개변수
    self.first = first              # ② 메서드의 수행문
    self.second = second            # ② 메서드의 수행문
```

setdata라는 메서드는 self, first, second라는 총 3개의 매개변수를 필요로 하는데 실제로는 a.setdata\(4, 2\) 처럼 4와 2라는 2개의 값만 전달한 것이다.

왜 그럴까?

그 이유는 a.setdata\(4, 2\)처럼 호출하면 setdata 메서드의 첫 번째 매개변수 self에는 setdata메서드를 호출한 객체 a가 자동으로 전달되기 때문이다. 다시 말해, a. 부분이 b나 c로 바뀔 수도 잇기 때문에, self라는 이름을 사용하는 것임. 이렇게하면 객체 이름에 상관없이 마구마구 호출 가능

