# 재귀 함수(Recursive Function)

* 함수가 스스로를 호출하는 것
* 재귀 함수를 만들 때는 언제 재귀를 멈출지 알려줘야 한다.
* 따라서 모든 재귀 함수는 기본 단계(Base case)와 재귀 단계(Recursion case)라는 두 부분으로 나누어져 있다.
* 여기서 재귀 단계는 함수가 자기 자신을 호출하는 부분이고, 기본 단계는 함수가 자기 자신을 다시 호출하지 않는 경우, 즉 무한 반복으로 빠져들지 않게 하는 부분이다.

```java
public class Main {
    public static void main(String args[]){
        int num = 3;
        System.out.println(factorial(num));
    }

    public static int factorial(int n) {
        if(n <= 1) { // 기본 단계
            return 1;
        } else { // 재귀 단계
            return n * factorial(n-1);
        }
    }
}
```

## 재귀 함수와 콜스택(호출 스택)

![http://www.thinkingincrowd.me/2016/06/06/How-to-avoid-Stack-overflow-error-on-recursive/](https://raw.githubusercontent.com/kenspirit/blog-cdn-data/master/factorial_stack_change_flow.png)

콜스택은 재귀 함수를 사용할 때 중요한 개념이다. 콜스택은 LIFO이다. 따라서 나중에 들어간 게 먼저 나온다. 함수가 실행되면 먼저 실행한 함수부터 콜스택에 차곡차곡 쌓이게(Push) 된다. 이때 변수의 값도 메모리에 같이 저장이 된다. 그리고 실행이 끝난 함수는 콜스택에서 나오게(Pop) 된다. 여기서 가장 중요한 포인트는, 함수가 완전히 실행을 완료하기 전이라고 해도 그 함수를 잠시 멈추고 다른 함수를 호출할 수 있다는 점이다. 중지된 함수의 변수값들은 모두 메모리에 저장되어 있다. 따라서 위에 쌓인 함수의 실행이 완료되면, 아래에 있는 함수들은 자신의 차례가 되었을 때 멈추었던 위치부터 다시 실행할 수 있다.

## References

* 그림으로 개념을 이해하는 알고리즘
