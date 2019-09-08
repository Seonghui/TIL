# CSS 팁 모음

## @import 사용하지 말것

> [http://www.stevesouders.com/blog/2009/04/09/dont-use-import/](http://www.stevesouders.com/blog/2009/04/09/dont-use-import/)

`@import`는 `<link>`보다 느리다.

## 불필요한 sass, scss nesting 자제

꼭 필요한 부분에만 쓰자

```text
// Without nesting
.table > thead > tr > th { … }
.table > thead > tr > td { … }

// With nesting
.table > thead > tr {
  > th { … }
  > td { … }
}
```

