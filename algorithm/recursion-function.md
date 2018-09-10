재귀 함수(Recursion Function)
===
* 함수가 스스로를 호출하는 것
* 재귀 함수를 만들 때는 언제 재귀를 멈출지 알려줘야 한다.
* 따라서 모든 재귀 함수는 기본 단계(Base case)와 재귀 단계(Recursion case)라는 두 부분으로 나누어져 있다.
* 여기서 재귀 단계는 함수가 자기 자신을 호출하는 부분이고, 기본 단계는 함수가 자기 자신을 다시 호출하지 않는 경우, 즉 무한 반복으로 빠져들지 않게 하는 부분이다.

```java
public class Main {
	public static void main(String args[]){
		int num = 5;
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