---
layout: single
title: 'datamanim Type1. 전세계 행복도 지표 데이터'
categories: datamanim
tag: [datamanim,Type1,preprocessing]
---

# 전세계 행복도 지표 데이터


```python
import pandas as pd
df =pd.read_csv('https://raw.githubusercontent.com/Datamanim/datarepo/main/happy2/happiness.csv',encoding='utf-8')
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
      <th>행복랭킹</th>
      <th>나라명</th>
      <th>점수</th>
      <th>상대GDP</th>
      <th>사회적지원</th>
      <th>행복기대치</th>
      <th>선택의 자유도</th>
      <th>관대함</th>
      <th>부패에 대한인식</th>
      <th>년도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Finland</td>
      <td>7.769</td>
      <td>1.340</td>
      <td>1.587</td>
      <td>0.986</td>
      <td>0.596</td>
      <td>0.153</td>
      <td>0.393</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Denmark</td>
      <td>7.600</td>
      <td>1.383</td>
      <td>1.573</td>
      <td>0.996</td>
      <td>0.592</td>
      <td>0.252</td>
      <td>0.410</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Norway</td>
      <td>7.554</td>
      <td>1.488</td>
      <td>1.582</td>
      <td>1.028</td>
      <td>0.603</td>
      <td>0.271</td>
      <td>0.341</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Iceland</td>
      <td>7.494</td>
      <td>1.380</td>
      <td>1.624</td>
      <td>1.026</td>
      <td>0.591</td>
      <td>0.354</td>
      <td>0.118</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Netherlands</td>
      <td>7.488</td>
      <td>1.396</td>
      <td>1.522</td>
      <td>0.999</td>
      <td>0.557</td>
      <td>0.322</td>
      <td>0.298</td>
      <td>2019</td>
    </tr>
  </tbody>
</table>
</div>



---
## Q44. 2018년도와 2019년도의 행복랭킹이 변화하지 않은 나라명의 수를 구하여라


datamanim 에서는 엄청 간단하게 풀이하고 있다.  
2018,2019 두 개의 구분자 밖에 없으니 ['행복랭킹', '나라명'] 을 중복제거 했을떄 없어지는게 변화하지 않은 것이다..


```python
df[df[['행복랭킹','나라명']].duplicated()].sort_values('행복랭킹')  # .shape[0]을 찍으면 15개 인것을 알 수 있다.
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
      <th>행복랭킹</th>
      <th>나라명</th>
      <th>점수</th>
      <th>상대GDP</th>
      <th>사회적지원</th>
      <th>행복기대치</th>
      <th>선택의 자유도</th>
      <th>관대함</th>
      <th>부패에 대한인식</th>
      <th>년도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>156</th>
      <td>1</td>
      <td>Finland</td>
      <td>7.632</td>
      <td>1.305</td>
      <td>1.592</td>
      <td>0.874</td>
      <td>0.681</td>
      <td>0.202</td>
      <td>0.393</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>159</th>
      <td>4</td>
      <td>Iceland</td>
      <td>7.495</td>
      <td>1.343</td>
      <td>1.644</td>
      <td>0.914</td>
      <td>0.677</td>
      <td>0.353</td>
      <td>0.138</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>163</th>
      <td>8</td>
      <td>New Zealand</td>
      <td>7.324</td>
      <td>1.268</td>
      <td>1.601</td>
      <td>0.876</td>
      <td>0.669</td>
      <td>0.365</td>
      <td>0.389</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>177</th>
      <td>22</td>
      <td>Malta</td>
      <td>6.627</td>
      <td>1.270</td>
      <td>1.525</td>
      <td>0.884</td>
      <td>0.645</td>
      <td>0.376</td>
      <td>0.142</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>189</th>
      <td>34</td>
      <td>Singapore</td>
      <td>6.343</td>
      <td>1.529</td>
      <td>1.451</td>
      <td>1.008</td>
      <td>0.631</td>
      <td>0.261</td>
      <td>0.457</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>208</th>
      <td>53</td>
      <td>Latvia</td>
      <td>5.933</td>
      <td>1.148</td>
      <td>1.454</td>
      <td>0.671</td>
      <td>0.363</td>
      <td>0.092</td>
      <td>0.066</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>211</th>
      <td>56</td>
      <td>Jamaica</td>
      <td>5.890</td>
      <td>0.819</td>
      <td>1.493</td>
      <td>0.693</td>
      <td>0.575</td>
      <td>0.096</td>
      <td>0.031</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>215</th>
      <td>60</td>
      <td>Kazakhstan</td>
      <td>5.790</td>
      <td>1.143</td>
      <td>1.516</td>
      <td>0.631</td>
      <td>0.454</td>
      <td>0.148</td>
      <td>0.121</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>220</th>
      <td>65</td>
      <td>Peru</td>
      <td>5.663</td>
      <td>0.934</td>
      <td>1.249</td>
      <td>0.674</td>
      <td>0.530</td>
      <td>0.092</td>
      <td>0.034</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>231</th>
      <td>76</td>
      <td>Hong Kong</td>
      <td>5.430</td>
      <td>1.405</td>
      <td>1.290</td>
      <td>1.030</td>
      <td>0.524</td>
      <td>0.246</td>
      <td>0.291</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>278</th>
      <td>123</td>
      <td>Mozambique</td>
      <td>4.417</td>
      <td>0.198</td>
      <td>0.902</td>
      <td>0.173</td>
      <td>0.531</td>
      <td>0.206</td>
      <td>0.158</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>294</th>
      <td>139</td>
      <td>Togo</td>
      <td>3.999</td>
      <td>0.259</td>
      <td>0.474</td>
      <td>0.253</td>
      <td>0.434</td>
      <td>0.158</td>
      <td>0.101</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>298</th>
      <td>143</td>
      <td>Madagascar</td>
      <td>3.774</td>
      <td>0.262</td>
      <td>0.908</td>
      <td>0.402</td>
      <td>0.221</td>
      <td>0.155</td>
      <td>0.049</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>308</th>
      <td>153</td>
      <td>Tanzania</td>
      <td>3.303</td>
      <td>0.455</td>
      <td>0.991</td>
      <td>0.381</td>
      <td>0.481</td>
      <td>0.270</td>
      <td>0.097</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>310</th>
      <td>155</td>
      <td>Central African Republic</td>
      <td>3.083</td>
      <td>0.024</td>
      <td>0.000</td>
      <td>0.010</td>
      <td>0.305</td>
      <td>0.218</td>
      <td>0.038</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
</div>



---
좀 복잡하게 푼 내 풀이를 정리해보곘다. (pivot_table)  


```python
df_new = pd.pivot_table(df, index = '행복랭킹', columns = '년도' , values = '나라명', aggfunc = lambda x : '-'.join(x))  # pivot_table 만드는 법 숙지!
df_new.head()
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
      <th>년도</th>
      <th>2018</th>
      <th>2019</th>
    </tr>
    <tr>
      <th>행복랭킹</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Finland</td>
      <td>Finland</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Norway</td>
      <td>Denmark</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Denmark</td>
      <td>Norway</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Iceland</td>
      <td>Iceland</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Switzerland</td>
      <td>Netherlands</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_new['chk'] = df_new[2018] == df_new[2019]
```


```python
df_new[df_new.chk == True]  # .shape[0] 찍으면 15인 것을 알 수 있다.
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
      <th>년도</th>
      <th>2018</th>
      <th>2019</th>
      <th>chk</th>
    </tr>
    <tr>
      <th>행복랭킹</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Finland</td>
      <td>Finland</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Iceland</td>
      <td>Iceland</td>
      <td>True</td>
    </tr>
    <tr>
      <th>8</th>
      <td>New Zealand</td>
      <td>New Zealand</td>
      <td>True</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Malta</td>
      <td>Malta</td>
      <td>True</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Singapore</td>
      <td>Singapore</td>
      <td>True</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Latvia</td>
      <td>Latvia</td>
      <td>True</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Jamaica</td>
      <td>Jamaica</td>
      <td>True</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Kazakhstan</td>
      <td>Kazakhstan</td>
      <td>True</td>
    </tr>
    <tr>
      <th>65</th>
      <td>Peru</td>
      <td>Peru</td>
      <td>True</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Hong Kong</td>
      <td>Hong Kong</td>
      <td>True</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Mozambique</td>
      <td>Mozambique</td>
      <td>True</td>
    </tr>
    <tr>
      <th>139</th>
      <td>Togo</td>
      <td>Togo</td>
      <td>True</td>
    </tr>
    <tr>
      <th>143</th>
      <td>Madagascar</td>
      <td>Madagascar</td>
      <td>True</td>
    </tr>
    <tr>
      <th>153</th>
      <td>Tanzania</td>
      <td>Tanzania</td>
      <td>True</td>
    </tr>
    <tr>
      <th>155</th>
      <td>Central African Republic</td>
      <td>Central African Republic</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>


