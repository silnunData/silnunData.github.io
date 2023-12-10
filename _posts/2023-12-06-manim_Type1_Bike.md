---
layout: single
title: 'datamanim Type1. 서울시 따릉이 이용정보 데이터'
categories: datamanim
tag: datamanim, preprocessing
---

# 서울시 따릉이 이용정보 데이터

## Q33. 각 요일별 가장 많이 이용한 대여소의 이용횟수와 대여소 번호를 데이터 프레임으로 출력하라


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
      <th>대여일자</th>
      <th>대여시간</th>
      <th>대여소번호</th>
      <th>대여구분코드</th>
      <th>성별</th>
      <th>연령대코드</th>
      <th>이용건수</th>
      <th>운동량</th>
      <th>탄소량</th>
      <th>이동거리</th>
      <th>사용시간</th>
      <th>day_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>3541</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>8</td>
      <td>Tuesday</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>765</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>27.21</td>
      <td>0.35</td>
      <td>1526.81</td>
      <td>19</td>
      <td>Tuesday</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>2637</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>41.40</td>
      <td>0.37</td>
      <td>1608.56</td>
      <td>18</td>
      <td>Tuesday</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>2919</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>75</td>
      <td>Tuesday</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>549</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>13.04</td>
      <td>0.17</td>
      <td>731.55</td>
      <td>6</td>
      <td>Tuesday</td>
    </tr>
  </tbody>
</table>
</div>




```python
# idea : day_name, 대여소번호로 grouping 한 후 내립차순으로 정렬하고, day_name 의 중복을 첫번째만 남기고 없앤다.

# to_frame 한 상태에서는 day_name 이 index로 들어가 있기 때문에, drop_duplicates를 할 수가 없다.
df2 = df.groupby(['day_name','대여소번호']).size().sort_values(ascending = False).to_frame('size')
df2
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
      <th></th>
      <th>size</th>
    </tr>
    <tr>
      <th>day_name</th>
      <th>대여소번호</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Saturday</th>
      <th>502</th>
      <td>378</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Sunday</th>
      <th>502</th>
      <td>372</td>
    </tr>
    <tr>
      <th>152</th>
      <td>338</td>
    </tr>
    <tr>
      <th>Saturday</th>
      <th>207</th>
      <td>323</td>
    </tr>
    <tr>
      <th>Sunday</th>
      <th>207</th>
      <td>318</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>Saturday</th>
      <th>2659</th>
      <td>1</td>
    </tr>
    <tr>
      <th>Friday</th>
      <th>2094</th>
      <td>1</td>
    </tr>
    <tr>
      <th>Sunday</th>
      <th>2425</th>
      <td>1</td>
    </tr>
    <tr>
      <th>Tuesday</th>
      <th>2094</th>
      <td>1</td>
    </tr>
    <tr>
      <th>Wednesday</th>
      <th>2288</th>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>16874 rows × 1 columns</p>
</div>




```python
# reset_index() 에서 drop=True 옵션이 index를 col으로 빼고 없애는건데, 지금의 경우에는 없애면 안된다.
df2.reset_index().drop_duplicates('day_name', keep = 'first')
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
      <th>day_name</th>
      <th>대여소번호</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Saturday</td>
      <td>502</td>
      <td>378</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sunday</td>
      <td>502</td>
      <td>372</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Wednesday</td>
      <td>502</td>
      <td>282</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Friday</td>
      <td>502</td>
      <td>277</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Tuesday</td>
      <td>502</td>
      <td>267</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Monday</td>
      <td>502</td>
      <td>242</td>
    </tr>
    <tr>
      <th>298</th>
      <td>Thursday</td>
      <td>2715</td>
      <td>137</td>
    </tr>
  </tbody>
</table>
</div>




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
      <th>대여일자</th>
      <th>대여시간</th>
      <th>대여소번호</th>
      <th>대여구분코드</th>
      <th>성별</th>
      <th>연령대코드</th>
      <th>이용건수</th>
      <th>운동량</th>
      <th>탄소량</th>
      <th>이동거리</th>
      <th>사용시간</th>
      <th>day_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>3541</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>8</td>
      <td>Tuesday</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>765</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>27.21</td>
      <td>0.35</td>
      <td>1526.81</td>
      <td>19</td>
      <td>Tuesday</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>2637</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>41.40</td>
      <td>0.37</td>
      <td>1608.56</td>
      <td>18</td>
      <td>Tuesday</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>2919</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>75</td>
      <td>Tuesday</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>549</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>13.04</td>
      <td>0.17</td>
      <td>731.55</td>
      <td>6</td>
      <td>Tuesday</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.대여구분코드.unique()
```




    array(['정기권', '일일권', '단체권', '일일권(비회원)'], dtype=object)



## Q34. 나이대별 대여구분 코드의 (일일권/전체횟수) 비율을 구한 후 가장 높은 비율을 가지는 나이대를 확인하라. 일일권의 경우 일일권 과 일일권(비회원)을 모두 포함하라


```python
df.head(3)
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
      <th>대여일자</th>
      <th>대여시간</th>
      <th>대여소번호</th>
      <th>대여구분코드</th>
      <th>성별</th>
      <th>연령대코드</th>
      <th>이용건수</th>
      <th>운동량</th>
      <th>탄소량</th>
      <th>이동거리</th>
      <th>사용시간</th>
      <th>day_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>3541</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>8</td>
      <td>Tuesday</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>765</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>27.21</td>
      <td>0.35</td>
      <td>1526.81</td>
      <td>19</td>
      <td>Tuesday</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-06-01</td>
      <td>0</td>
      <td>2637</td>
      <td>정기권</td>
      <td>F</td>
      <td>~10대</td>
      <td>1</td>
      <td>41.40</td>
      <td>0.37</td>
      <td>1608.56</td>
      <td>18</td>
      <td>Tuesday</td>
    </tr>
  </tbody>
</table>
</div>




```python
daily = df[df.대여구분코드.isin(['일일권','일일권(비회원)'])].groupby('연령대코드').size().sort_index()
daily
```




    연령대코드
    20대     59393
    30대     31127
    40대     12695
    50대      4607
    60대       819
    70대~      158
    ~10대    11540
    dtype: int64




```python
total = df.연령대코드.value_counts().sort_index()
total  # pandas series type
```




    연령대코드
    20대     247561
    30대     186722
    40대     114799
    50대      70428
    60대      19288
    70대~      3227
    ~10대     36925
    Name: count, dtype: int64




```python
# series type 끼리 서로 연산이 가능함
(daily/total).sort_values(ascending = False)
```




    연령대코드
    ~10대    0.312525
    20대     0.239913
    30대     0.166702
    40대     0.110585
    50대     0.065414
    70대~    0.048962
    60대     0.042462
    dtype: float64



## Q38. 평일 (월~금) 출근 시간대(오전 6,7,8시)의 대여소별 이용 횟수를 구해서 데이터 프레임 형태로 표현한 후 각 대여시간별 이용 횟수의 상위 3개 대여소와 이용횟수를 출력하라


```python
con1 = df.day_name.isin(['Saturday','Sunday'])
con2 = df.대여시간.isin([6,7,8])
df_cond = df[~con1 & con2]
df_tg = df_cond.groupby(['대여시간','대여소번호']).size().to_frame('이용 횟수')
df_tg
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
      <th></th>
      <th>이용 횟수</th>
    </tr>
    <tr>
      <th>대여시간</th>
      <th>대여소번호</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">6</th>
      <th>102</th>
      <td>5</td>
    </tr>
    <tr>
      <th>103</th>
      <td>8</td>
    </tr>
    <tr>
      <th>106</th>
      <td>1</td>
    </tr>
    <tr>
      <th>109</th>
      <td>4</td>
    </tr>
    <tr>
      <th>111</th>
      <td>5</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">8</th>
      <th>4864</th>
      <td>4</td>
    </tr>
    <tr>
      <th>4865</th>
      <td>12</td>
    </tr>
    <tr>
      <th>4867</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4868</th>
      <td>3</td>
    </tr>
    <tr>
      <th>9980</th>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>6236 rows × 1 columns</p>
</div>




```python
# sort_values 에 값을 이용횟수만 넣으면 index가 꼬여서 대여시간까지 같이 넣어줘야함
display(df_tg.sort_values(['대여시간','이용 횟수'], ascending = False),df_tg.sort_values(['이용 횟수'], ascending = False))
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
      <th></th>
      <th>이용 횟수</th>
    </tr>
    <tr>
      <th>대여시간</th>
      <th>대여소번호</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">8</th>
      <th>2701</th>
      <td>119</td>
    </tr>
    <tr>
      <th>646</th>
      <td>115</td>
    </tr>
    <tr>
      <th>1152</th>
      <td>92</td>
    </tr>
    <tr>
      <th>1906</th>
      <td>91</td>
    </tr>
    <tr>
      <th>247</th>
      <td>89</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">6</th>
      <th>4804</th>
      <td>1</td>
    </tr>
    <tr>
      <th>4811</th>
      <td>1</td>
    </tr>
    <tr>
      <th>4852</th>
      <td>1</td>
    </tr>
    <tr>
      <th>4860</th>
      <td>1</td>
    </tr>
    <tr>
      <th>4861</th>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>6236 rows × 1 columns</p>
</div>



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
      <th></th>
      <th>이용 횟수</th>
    </tr>
    <tr>
      <th>대여시간</th>
      <th>대여소번호</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">8</th>
      <th>2701</th>
      <td>119</td>
    </tr>
    <tr>
      <th>646</th>
      <td>115</td>
    </tr>
    <tr>
      <th>7</th>
      <th>259</th>
      <td>104</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">8</th>
      <th>1152</th>
      <td>92</td>
    </tr>
    <tr>
      <th>1906</th>
      <td>91</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>6</th>
      <th>1929</th>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <th>1652</th>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <th>1920</th>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <th>1657</th>
      <td>1</td>
    </tr>
    <tr>
      <th>8</th>
      <th>9980</th>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>6236 rows × 1 columns</p>
</div>



```python
# df_tg로 한번 grouping 된 dataframe을 다시 grouping 하면서 head()로 집계함
df_tg.sort_values(['대여시간','이용 횟수'], ascending = False).groupby(['대여시간']).head(3)
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
      <th></th>
      <th>이용 횟수</th>
    </tr>
    <tr>
      <th>대여시간</th>
      <th>대여소번호</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">8</th>
      <th>2701</th>
      <td>119</td>
    </tr>
    <tr>
      <th>646</th>
      <td>115</td>
    </tr>
    <tr>
      <th>1152</th>
      <td>92</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">7</th>
      <th>259</th>
      <td>104</td>
    </tr>
    <tr>
      <th>230</th>
      <td>77</td>
    </tr>
    <tr>
      <th>726</th>
      <td>77</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">6</th>
      <th>2744</th>
      <td>45</td>
    </tr>
    <tr>
      <th>1125</th>
      <td>40</td>
    </tr>
    <tr>
      <th>1028</th>
      <td>36</td>
    </tr>
  </tbody>
</table>
</div>



# Q40. 남성(‘M’ or ‘m’)과 여성(‘F’ or ‘f’)의 이동거리값의 평균값을 구하여라


```python
# map 을 통해 컬럼 값 넣기
df['sex'] = df.성별.map(lambda x : '남' if x.lower() == 'm' else '여')
df.groupby('sex').이동거리.mean().to_frame('이동거리')
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
      <th>이동거리</th>
    </tr>
    <tr>
      <th>sex</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>남</th>
      <td>3209.110871</td>
    </tr>
    <tr>
      <th>여</th>
      <td>3468.575025</td>
    </tr>
  </tbody>
</table>
</div>


