# args and kwargs

kwargs는 키워드된 n개의 변수들을 함수의 인자로 보낼 때 사용한다. \(args 키워드되지않은 n개의 변수\)

_나 \*\*는 패킹, 언패킹과도 관련이 있는데 위처럼 리스트나 딕셔너리 앞에 쓰면 언패킹, 흩어져있는 값들을_ args, \*\*kwagrs로 묶는 다면 패킹이라고 한다.

\(아마\) OOP에서 자주 사용하는데, 이렇게 하면 함수를 오버라이딩할 때, original function의 arugments들을 갯수에 상관없이 전부 다 부를 수 있음.

args랑 kwargs를 _랑 \*_없이 사용할 수는 없음. 그리고 굳이 이름 저렇게 안 붙여도 됨 그냥 컨벤션임.

\("kwargs" means "keyword arguments"\)

```text
def print_value(A, B, C):
    value = "{}/{}/{}".format(A, B, C)
    print(value)


args = (1, 2, 3)
kwargs = {"A": 5, "B": 11, "C": 99}


print_value(*args) # 1/2/3

print_value(**kwagrs) # 5/11/99
```

## django 모델에서의 kwargs

### Example 1

```text
# Model
from django.db import models


class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.CharField(max_length=20)

    def __str__(self):
        return self.title

# Admin
from .models import Book


kwargs = {'title': 'AAA', 'name': 'Seul'}

Book.objects.create(**kwargs) 

Book.objects.filter(**kwargs)
```

### Example 2

```text
class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

    def save(self, *args, **kwargs):
        do_something()
        super(Blog, self).save(*args, **kwargs) # Call the "real" save() method.
        do_something_else()
```

* \*args - list of unnamed arguments
* \*\*kwargs - dictionary \(named arguments\)

따라서...

```text
save(1,2,3,4,a=20,b=30,c=40)
```

* \*args: \(1, 2, 3, 4\)
* \*\*kwargs: {'a':20,'b':30,'c':40}

### Example3

```text
def foo(a, b, c, d):
  print a, b, c, d

l = [0, 1]
d = {"d":3, "c":2}

foo(*l, **d)
```

prink: `0 1 2 3`

### Example4

> > > def print\_keyword\_args\(\*\*kwargs\): ... \# kwargs is a dict of the keyword args passed to the function ... for key, value in kwargs.iteritems\(\): ... print "%s = %s" % \(key, value\) ... print\_keyword\_args\(first\_name="John", last\_name="Doe"\) first\_name = John last\_name = Doe

