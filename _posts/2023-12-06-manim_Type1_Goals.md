---
layout: single
title: "datamanim Type1. 월드컵 출전선수 골기록 데이터"
---


# datamanim Type1. 월드컵 출전선수 골기록 데이터


```python
import pandas as pd

df= pd.read_csv('https://raw.githubusercontent.com/Datamanim/datarepo/main/worldcup/worldcupgoals.csv')
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Player</th>
      <th>Goals</th>
      <th>Years</th>
      <th>Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Miroslav Klose</td>
      <td>16</td>
      <td>2002-2006-2010-2014</td>
      <td>Germany</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ronaldo</td>
      <td>15</td>
      <td>1998-2002-2006</td>
      <td>Brazil</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Gerd Muller</td>
      <td>14</td>
      <td>1970-1974</td>
      <td>Germany</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Just Fontaine</td>
      <td>13</td>
      <td>1958</td>
      <td>France</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pele</td>
      <td>12</td>
      <td>1958-1962-1966-1970</td>
      <td>Brazil</td>
    </tr>
  </tbody>
</table>
</div>




## Q23. Years 컬럼은 년도 -년도 형식으로 구성되어있고, 각 년도는 4자리 숫자이다. 년도 표기가 4자리 숫자로 안된 케이스가 존재한다. 해당 건은 몇건인지 출력하라


```python
df['yearList'] = df.Years.str.split('-')  # 각 원소는 list type

def check4(x) :  # apply 로 해당 열의 원소가 차례로 들어간다.
    for i in x : 
        if len(str(i)) != 4 : 
            return False
    return True  # if 문 안에 있으면 첫 원소만 비교하고, 밖에 있으면 전체 list 와 비교

df['check'] = df.yearList.apply(check4)
# series 일때는 자동으로 각 원소를 차례로 넣고, dataframe일 떄는 axis 설정 필요

len(df[df.check == False])
```




    45



### 만약, 쪼개어져 있는 list로 구성된 data를 어떻게 합칠 수 있지? (join)


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Player</th>
      <th>Goals</th>
      <th>Years</th>
      <th>Country</th>
      <th>yearList</th>
      <th>check</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Miroslav Klose</td>
      <td>16</td>
      <td>2002-2006-2010-2014</td>
      <td>Germany</td>
      <td>[2002, 2006, 2010, 2014]</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ronaldo</td>
      <td>15</td>
      <td>1998-2002-2006</td>
      <td>Brazil</td>
      <td>[1998, 2002, 2006]</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Gerd Muller</td>
      <td>14</td>
      <td>1970-1974</td>
      <td>Germany</td>
      <td>[1970, 1974]</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Just Fontaine</td>
      <td>13</td>
      <td>1958</td>
      <td>France</td>
      <td>[1958]</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pele</td>
      <td>12</td>
      <td>1958-1962-1966-1970</td>
      <td>Brazil</td>
      <td>[1958, 1962, 1966, 1970]</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['test1'] = df.yearList.apply(lambda x : ','.join(map(str, x)))

# '구분자'.join(x) 형태로 작성
# map(변환함수, 반복객체)
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Player</th>
      <th>Goals</th>
      <th>Years</th>
      <th>Country</th>
      <th>yearList</th>
      <th>check</th>
      <th>test1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Miroslav Klose</td>
      <td>16</td>
      <td>2002-2006-2010-2014</td>
      <td>Germany</td>
      <td>[2002, 2006, 2010, 2014]</td>
      <td>True</td>
      <td>2002,2006,2010,2014</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ronaldo</td>
      <td>15</td>
      <td>1998-2002-2006</td>
      <td>Brazil</td>
      <td>[1998, 2002, 2006]</td>
      <td>True</td>
      <td>1998,2002,2006</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Gerd Muller</td>
      <td>14</td>
      <td>1970-1974</td>
      <td>Germany</td>
      <td>[1970, 1974]</td>
      <td>True</td>
      <td>1970,1974</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Just Fontaine</td>
      <td>13</td>
      <td>1958</td>
      <td>France</td>
      <td>[1958]</td>
      <td>True</td>
      <td>1958</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pele</td>
      <td>12</td>
      <td>1958-1962-1966-1970</td>
      <td>Brazil</td>
      <td>[1958, 1962, 1966, 1970]</td>
      <td>True</td>
      <td>1958,1962,1966,1970</td>
    </tr>
  </tbody>
</table>
</div>



## 28. 이름에 ‘carlos’ 단어가 들어가는 선수의 숫자는 몇 명인가? (대, 소문자 구분 x)


```python
len(df[df.Player.str.lower().str.contains('carlos')])

# 미리 소문자로 전체적으로 다 바꾸고, 그 다음에 소문자와 비교 
```




    13


