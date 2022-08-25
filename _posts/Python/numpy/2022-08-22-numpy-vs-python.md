---
title:  "[numpy] python vs numpy 속도 비교" 

categories: 
  - numpy
tags:
  - [python, numpy]

toc: true
toc_sticky: true

date: 2022-08-24
last_modified_at: 2022-08-24

---



## python과 Numpy  데이터 타입 비교

 파이썬의 경우 실행중에 타입이 바뀔 수 있는 인터프리터 언어이다. 이로 인한 간단함과 직관성은 장점이지만, 다른 정적 타입 컴파일 언어에 비해 속도가 많이 느리다는 단점이 있다. 그런 단점을 극복하기 위해 생겨난 라이브러리이다. C 언어로 컴파일 되어 있으며, 속도는 수백배가 차이난다.

### numpy의 array에 비해 Python이 느린 이유

같은 데이터를 표현하더라도, 동적타입을 사용하는 파이썬은 변수를 생성하더라도 객체 하나가 가져야 하는 header값이 만다.

```C
/* C언어의 int형 */
int result = 0;

/* Python의 객체 */
struct _longobject {
    long ob_refcnt;
    PyTypeObject *ob_type;
    size_t ob_size;
    long ob_digit[1];
};
```

c언어는 4바이트, python은 최소 20바이트 이상이므로 몇 배나 많은 공간을 차지하고 있다.

그런 파이썬에 비해 numpy의 array는 데이터 하나가 차이하는 공간이 C언어로 변수를 생성한 만큼만 데이터를 차지하고 있어 훨씬 빠르게 동작한다.

![image](https://user-images.githubusercontent.com/26536985/186685578-1e6776fc-3c94-4c01-8e11-a1305533ee06.png)



다음에 추가..





---



[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

