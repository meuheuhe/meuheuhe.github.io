---
title:  "[numpy] numpy를 사용해야하는 이유" 

categories: 
  - numpy
tags:
  - [python, numpy]

toc: true
toc_sticky: true

date: 2022-08-13
last_modified_at: 2022-08-13
---



## numpy 란?

- Numerical Python의 약자로 Python에서 대규모 다차원 배열을 다룰 수 있게 도와주는 라이브러리이다.
  데이터 대부분은 숫자 배열로 볼 수 있기 때문이다.



## numpy를 사용하는 이유

- Python에서 list를 사용하기위해 제공하는 함수보다 효율적인 메모리 관리와 빠른 연산을 지원하기 때문이다.



## List를 numpy array로 만드는 방법



~~~python
import numpy as np

np.array([1,2,3,4,5]) #list를 넣어주면 array로 반환해준다.
~~~







---



[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}