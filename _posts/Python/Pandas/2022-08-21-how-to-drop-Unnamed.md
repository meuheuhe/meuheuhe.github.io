---
title:  "[python] pandas Unnamed : 0 컬럼 제거" 

categories: 
  - pandas
tags:
  - [python, Padnas, dataframe]

toc: true
toc_sticky: true

date: 2022-08-21
last_modified_at: 2022-08-21

---



판다스로 read_csv() 함수를 사용하여 csv파일을 열 때, 보통 원치 않지만 컬럼(unnamed: 0) 이 추가가 되는데, 해당 column이 생기는 이유와 제거하는 방법에 대해 알아보자.

## Unnamed: 0 이 생기는 이유

- 보통 csv파일을 불러 올 때, 맨 처음 컬럼의 값이 비어져 있는 경우에 Unnamed: 0컬럼이 생기게 된다.![스크린샷 2022-08-21 오후 10.39.03](/Users/kimkijung/Library/Application Support/typora-user-images/스크린샷 2022-08-21 오후 10.39.03.png)

```python
import pandas as pd

df = pd.read_csv('test.csv')

df
```

![스크린샷 2022-08-21 오후 10.46.45](/Users/kimkijung/Library/Application Support/typora-user-images/스크린샷 2022-08-21 오후 10.46.45.png)

## 제거하는 방법

### 1.  read_csv() 함수의 옵션 설정

- 제목 그대로 read_csv()안에 index_cal = 0 을 추가해주면 된다.

~~~python
df = pd.read_csv('test.csv', index_col=0)

df
~~~

![스크린샷 2022-08-21 오후 11.32.45](/Users/kimkijung/Library/Application Support/typora-user-images/스크린샷 2022-08-21 오후 11.32.45.png)

### 2. 직접 Unnamed 컬럼 제거

- index_col 이라는 옵션이 생각나지 않으면, drop함수를 이용해 직접 컬럼을 제거해줘도 된다.

~~~python
df = pd.read_csv('test.csv')

df.drop(['Unnamed: 0'], axis = 1, inplace = True)

df
~~~

![스크린샷 2022-08-21 오후 11.35.45](/Users/kimkijung/Library/Application Support/typora-user-images/스크린샷 2022-08-21 오후 11.35.45.png)

추가로 함수안에 inplace = True 옵션을 주게 되면, 따로 변수에 직접 넣는 코드를 추가하지 않아도 된다.



---



[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}