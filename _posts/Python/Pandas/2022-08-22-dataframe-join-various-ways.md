---
title:  "[python] pandas dataframe 합치기" 

categories: 
  - pandas
tags:
  - [python, Padnas, dataframe]

toc: true
toc_sticky: true

date: 2022-08-22
last_modified_at: 2022-08-22
---



# Dataframe 합치기

대부분의 데이터는 여러 파일들이나 데이터베이스로 날짜 등으로 기록하기 때문에 나눠져 있다. 
 분석 또는 모델링에 사용을 하려면 원하는 결과가 나오게 조합을 할 줄 알아야 된다. 그리고 앞으로 데이터를 다루면서 많이 사용하게 되는 부분이다. 

 

## 1.dataframe 붙이기: pd.concat()

pd.concat() 함수의 경우 dataframe을 물리적으로 이어 붙여주는 함수이다. 그렇기 때문에 따로 중복제거는 되지 않는다.

```python
df_1 = pd.DataFrame(np.arange(36).reshape(6,6))
df_2 = pd.DataFrame(np.arange(25).reshape(5,5))
```



| df_1                                                         | df_2                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img width="192" alt="스크린샷 2022-08-22 오후 10 04 03" src="https://user-images.githubusercontent.com/26536985/185927987-dfb0a5f2-b500-4148-882d-1d9b0021bf23.png"> | <img width="158" alt="스크린샷 2022-08-22 오후 10 04 45" src="https://user-images.githubusercontent.com/26536985/185928120-d76dd8e3-6d23-4352-ad46-5e8c2019bfb9.png"> |



합쳐질 때 고정으로 오른쪽에 있는 dataframe이 왼쪽에 이어 붙이게 되어있고, default값으로 axis=0이 적용되어 있기 때문에, 아래로 dataframe을 이어 붙인다. 그런데 df_2에는 column '5' 가 없어서 NaN으로 채워진 것을 볼 수 있다. 이는 join의 default가 outer로 되어있기 때문이다.  그냥 붙이는 것이기 때문에 인덱스가 겹치는데 ignore_index=True를 추가해줌으로써 인덱스를 리셋할 수 있다. 

```python
pd.concat([df_1, df_2])
```



<img width="193" alt="스크린샷 2022-08-22 오후 10 37 38" src="https://user-images.githubusercontent.com/26536985/185934527-5ec8ed28-72be-4b05-8fa3-9f6ac9e3400a.png">

```python
pd.concat([df_1, df_2], ignore_index = True)
```

<img width="198" alt="스크린샷 2022-08-22 오후 10 39 35" src="https://user-images.githubusercontent.com/26536985/185934971-d95cf448-6d2d-4bf8-bf83-188d25e69f9e.png">

pd.concat() 교집합 적용

```python
pd.concat([df_1,df_2], ignore_index=True, join='inner')
```

<img width="163" alt="스크린샷 2022-08-22 오후 11 13 15" src="https://user-images.githubusercontent.com/26536985/185942521-a059cf08-be94-436e-b579-98b09301c416.png">



## 2.dataframe 합치기 : df.append()

pd.concat()함수와 같이 행 기준으로 합치는 방법이지만 append가 처리속도는 더 빠르다. df.append()함수는 같은 컬럼명이 존재하면 저장하고, 다른 컬럼명이면 해당 컬럼에 있는 Dataframe저장, 없는 곳은 결측치 처리를 한다. 

 ```python
 df_1.append(df_2)
 ```

<img width="192" alt="스크린샷 2022-08-23 오전 12 08 33" src="https://user-images.githubusercontent.com/26536985/185955223-26cdfd25-50f6-46f3-a879-7885357d60c0.png">





## 3. dataframe 병합 : pd.merge()

pd.merge()함수는 공통의 열을 기준으로 두 개의 dataframe을 합쳐준다. sql에서 join과 같은 역할이다. 보통 시간을 기준으로 여러 데이터를 합치고 싶을 때 사용한다.

pd.merge(df_left, df_right, how= '조인방식', on= 기준열).

```python
df_left = pd.DataFrame({'key': ['K0', 'K4', 'K2', 'K3'],
                        'A': ['A0', 'A4', 'A2', 'A3'],
                        'B': ['B0', 'B4', 'B2', 'B3']})

df_right = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
                          'C': ['C0', 'C1', 'C2', 'C3'],
                          'D': ['D0', 'D1', 'D2', 'D3']})
```

 기준열 name이 같을 때

```python
pd.merge(df_left, df_right, how='inner', on='key')
```

<img width="164" alt="스크린샷 2022-08-22 오후 11 34 39" src="https://user-images.githubusercontent.com/26536985/185947549-84c0f42e-596b-4b18-bd83-4d93eb0d185a.png">



기준열 name이 다를 때

Pd.merge(df_left, df_right, left_on = ‘왼쪽 열’, right_on = ‘오른쪽 열’, how = ‘조인방식’)

이때 둘 다 how 기본 값은 inner이다.

```python
df_right = df_right.rename(columns={'key':'key_'})
pd.merge(df_left, df_right, left_on='key', right_on='key_')
```

병합을 하고난 뒤 사용하지 않는 키는 따로 drop을 해서 제거하면 된다.

<img width="209" alt="스크린샷 2022-08-22 오후 11 47 26" src="https://user-images.githubusercontent.com/26536985/185950488-7154f2c0-094c-4613-9f39-8655df7d66f6.png">





### n개이상 dataframe merge 하는 방법

pd.merge의 경우 단점이 두 개의 dataframe만 합칠 수 있다는 것인데 이를 reduce라는 함수를 통해 여러 dataframe도 병합할 수 있다.

~~~python
from functools import reduce
#여러 개의 데이터를 대상으로 주로 누적 집계를 내기 위해서 사용한다.
#reduce(집계 함수, 순회 가능한 데이터[, 초기값])

df_1 = pd.DataFrame({'key': ['K0', 'K4', 'K2', 'K3'],
                        'A': ['A0', 'A4', 'A2', 'A3'],
                        'B': ['B0', 'B4', 'B2', 'B3']})

df_2 = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
                          'C': ['C0', 'C1', 'C2', 'C3'],
                          'D': ['D0', 'D1', 'D2', 'D3']})

df_2 = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
                          'E': ['E0', 'E1', 'E2', 'E3'],
                          'F': ['F0', 'F1', 'F2', 'F3']})

data_frames = [df1, df2, df3]
df_merged = reduce(lambda left, right: pd.merge(left, right, on = ['key'], how = 'inner'), data_frames)
~~~

<img width="222" alt="스크린샷 2022-08-22 오후 11 54 02" src="https://user-images.githubusercontent.com/26536985/185952057-5e12bb73-7998-49f0-b27d-2ef830370d38.png">



---



[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}