Javascript 연산자
=====

단항 연산자
-----
1. 증감연산자 (++, --)
    말 그대로 값을 증가시키거나 값을 감소시키는 연산자이다.
    * 전위형, 후위형?
    전위형은 ++i, 후위형은 i++
    전위형은 대입시에 증가되어 대입, 후위형은 현재 값을 대입 후 증가시킨다.
2. 비트 전환 연산자 (~)  
    
산술 연산자
-----
1. 더하기 (+)
2. 빼기 (-)
3. 곱하기 (*)
4. 나누기 (/)
5. 나머지 나누기 (%)  

시프트 연산자
-----
시프트 연산자는 대상 필드의 값을 비트로 바꾼 후 비트 수만큼 이동시켜서 값을 얻는 연산자이다.
1. 왼쪽 시프트 연산자 (<<)
2. 오른쪽 시프트 연산자 (>>)
3. Unsigned 오른쪽 시프트 연산자 (>>>)  
[참고](http://forum.falinux.com/zbxe/index.php?document_srl=580758&mid=lecture_tip)  

관계 연산자
-----
<, <=, >, >=, ==, ===, !==  

논리 연산자
-----
1. AND (&&)
2. OR (||)
3. NOT (!)  

조건 연산자
-----
3항 연산자(?:)
```javascript
test ? expression1 : expression2
```

복합 대입 연산자
-----
+=, -=, *=, /=  
[참고](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Expressions_and_Operators)  

typeof
-----
데이터 유형을 나타내는 연산자  

delete
-----
메모리에서 변수나 항목을 제거하는 연산자  

References
-----
[MDN](https://msdn.microsoft.com/ko-kr/library/ce57k8d5(v=vs.94).aspx)