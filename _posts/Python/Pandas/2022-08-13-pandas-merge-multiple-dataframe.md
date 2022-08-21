---
title:  "[python] pandas merge 여러개 합치기" 

categories: 
  - pandas
tags:
  - [python, Padnas, dataframe]

toc: true
toc_sticky: true

date: 2022-08-13
last_modified_at: 2022-08-13
---



#### merge

~~~python
#데이터 프레임 합치기
#두 데이터 프레임을 각 데이터에 존재하는 고유값(key)을 기준으로 병합할 때 사용한다.
#how = inner -> 교집합으로 공통 column을 기준으로 join을 하게된다.
#      outer -> 옵션을 줘서 id기준으로 합치되, 어르한쪽에라도 없는 데이터가 없는경우 NaN값이 지정된다.
#on = column_name
pd.merge(df_left, df_right, how = 'inner', on=None) #default

#n개이상 데이터 프레임하는 방법
from functools import reduce
#여러 개의 데이터를 대상으로 주로 누적 집계를 내기 위해서 사용한다.
#reduce(집계 함수, 순회 가능한 데이터[, 초기값])

data_frames = [df1, df2, df3]
df_merged = reduce(lambda left, right: pd.merge(left, right, on = ['column_key'], how = 'outer'), data_frames)
~~~







---



[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}