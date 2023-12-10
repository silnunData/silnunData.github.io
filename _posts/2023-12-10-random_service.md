---
layout: single
title: 'Threshold(경계값)에 따른 분류평가지표 구하기 (서비스 이탈 예측 데이터)'
categories: 'MachineLearning'
tag: [machinelearning,classifier,randomforest,datamanim,Type2]
---

# 서비스 이탈 예측 데이터
## RandomForestClassifier 활용, Binarizer로 threshold 값에 따른 평가 지표 확인 


```python
import pandas as pd
#데이터 로드
x_train = pd.read_csv("https://raw.githubusercontent.com/Datamanim/datarepo/main/churnk/X_train.csv")
y_train = pd.read_csv("https://raw.githubusercontent.com/Datamanim/datarepo/main/churnk/y_train.csv")
x_test= pd.read_csv("https://raw.githubusercontent.com/Datamanim/datarepo/main/churnk/X_test.csv")


display(x_train.head())
display(y_train.head())
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
      <th>CustomerId</th>
      <th>Surname</th>
      <th>CreditScore</th>
      <th>Geography</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Tenure</th>
      <th>Balance</th>
      <th>NumOfProducts</th>
      <th>HasCrCard</th>
      <th>IsActiveMember</th>
      <th>EstimatedSalary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15799217</td>
      <td>Zetticci</td>
      <td>791</td>
      <td>Germany</td>
      <td>Female</td>
      <td>35</td>
      <td>7</td>
      <td>52436.20</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>161051.75</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15748986</td>
      <td>Bischof</td>
      <td>705</td>
      <td>Germany</td>
      <td>Male</td>
      <td>42</td>
      <td>8</td>
      <td>166685.92</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>55313.51</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15722004</td>
      <td>Hsiung</td>
      <td>543</td>
      <td>France</td>
      <td>Female</td>
      <td>31</td>
      <td>4</td>
      <td>138317.94</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>61843.73</td>
    </tr>
    <tr>
      <th>3</th>
      <td>15780966</td>
      <td>Pritchard</td>
      <td>709</td>
      <td>France</td>
      <td>Female</td>
      <td>32</td>
      <td>2</td>
      <td>0.00</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>109681.29</td>
    </tr>
    <tr>
      <th>4</th>
      <td>15636731</td>
      <td>Ts'ai</td>
      <td>714</td>
      <td>Germany</td>
      <td>Female</td>
      <td>36</td>
      <td>1</td>
      <td>101609.01</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>447.73</td>
    </tr>
  </tbody>
</table>
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
      <th>CustomerId</th>
      <th>Exited</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15799217</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15748986</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15722004</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>15780966</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>15636731</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>


## 1. 데이터 파악
### 1.1 결측치 확인


```python
x_train.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 6499 entries, 0 to 6498
    Data columns (total 12 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   CustomerId       6499 non-null   int64  
     1   Surname          6499 non-null   object 
     2   CreditScore      6499 non-null   int64  
     3   Geography        6499 non-null   object 
     4   Gender           6499 non-null   object 
     5   Age              6499 non-null   int64  
     6   Tenure           6499 non-null   int64  
     7   Balance          6499 non-null   float64
     8   NumOfProducts    6499 non-null   int64  
     9   HasCrCard        6499 non-null   int64  
     10  IsActiveMember   6499 non-null   int64  
     11  EstimatedSalary  6499 non-null   float64
    dtypes: float64(2), int64(7), object(3)
    memory usage: 609.4+ KB


### 1-2. 컬럼 분류
HasCrCard, IsActiveMember 의 경우 분류의 성향이 더 짙으므로 문자형으로 분류  
NumOfProducts, Tenure 의 경우 숫자가 유의미 할 수도 있기 때문에 숫자형으로 분류


```python
COL_DEL = ['CustomerId','Surname']
COL_NUM = ['CreditScore','Age','Tenure','NumOfProducts','Balance','EstimatedSalary']
COL_CAT = ['Geography','Gender','HasCrCard','IsActiveMember']
```


```python
x_train.drop(columns = COL_DEL, inplace = True)
x_test.drop(columns = COL_DEL, inplace = True)
y_train.drop(columns = 'CustomerId', inplace= True)
```

### 1-3 수치형 데이터 확인


```python
x_train[COL_NUM].describe()
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
      <th>CreditScore</th>
      <th>Age</th>
      <th>Tenure</th>
      <th>NumOfProducts</th>
      <th>Balance</th>
      <th>EstimatedSalary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>6499.000000</td>
      <td>6499.000000</td>
      <td>6499.000000</td>
      <td>6499.000000</td>
      <td>6499.000000</td>
      <td>6499.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>650.396830</td>
      <td>38.957070</td>
      <td>5.041545</td>
      <td>1.519772</td>
      <td>76836.581068</td>
      <td>100346.564524</td>
    </tr>
    <tr>
      <th>std</th>
      <td>96.618957</td>
      <td>10.502803</td>
      <td>2.891779</td>
      <td>0.578975</td>
      <td>62407.570894</td>
      <td>57944.655305</td>
    </tr>
    <tr>
      <th>min</th>
      <td>350.000000</td>
      <td>18.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>11.580000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>584.000000</td>
      <td>32.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>50907.565000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>651.000000</td>
      <td>37.000000</td>
      <td>5.000000</td>
      <td>1.000000</td>
      <td>97560.160000</td>
      <td>100496.840000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>718.000000</td>
      <td>44.000000</td>
      <td>8.000000</td>
      <td>2.000000</td>
      <td>127844.690000</td>
      <td>150480.155000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>850.000000</td>
      <td>92.000000</td>
      <td>10.000000</td>
      <td>4.000000</td>
      <td>238387.560000</td>
      <td>199970.740000</td>
    </tr>
  </tbody>
</table>
</div>



skew 값이 3을 넘지 않으므로 편차가 심하다고 볼 수 없다.  
만약, 편차가 심하다면 np.log1p(x)를 써서 log 변환을 해주도록 한다.


```python
x_train[COL_NUM].skew()
```




    CreditScore       -0.091121
    Age                1.027313
    Tenure             0.000274
    NumOfProducts      0.770079
    Balance           -0.149005
    EstimatedSalary   -0.001932
    dtype: float64



IQR 이상치 확인해봤을때, 수치상으로 'Age' 컬럼이 이상치가 가장 많은 것으로 나오고 있다.  
어떤 구성을 가지고 있는 boxplot을 통해 정확하게 보도록 해보자


```python
def IQR_chk(x) : 
    Q1, Q3 = x.quantile([0.25,0.75])
    IQR = Q3 - Q1
    lower = Q1-1.5*IQR
    upper = Q3+1.5*IQR
    return str(round(len(x[(x <= lower) | (x >= upper)])/len(x) * 100,2)) + '%'
    
x_train[COL_NUM].apply(IQR_chk)
```




    CreditScore        0.23%
    Age                4.12%
    Tenure              0.0%
    NumOfProducts       0.6%
    Balance             0.0%
    EstimatedSalary     0.0%
    dtype: object



boxplot을 통해 확인해봤을때, 60대 이상에서 이상치들이 많이 포함되고 있는것이 보여진다.  
실제 나이가 많은 사람들이 이용할 수도 있기 때문에, 따로 이상치 제거등은 하지 않도록 하겠다.


```python
import seaborn as sns
sns.boxplot(x_train.Age)
```




    <Axes: ylabel='Age'>




    
![png](output_14_1.png)
    


## 2. 데이터 변환
### 2-1 수치형 정규화, 표준화


```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
scaler.fit(x_train[COL_NUM])
x_train[COL_NUM] = scaler.transform(x_train[COL_NUM])
x_test[COL_NUM] = scaler.transform(x_test[COL_NUM])
```


```python
x_train.head(3)
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
      <th>CreditScore</th>
      <th>Geography</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Tenure</th>
      <th>Balance</th>
      <th>NumOfProducts</th>
      <th>HasCrCard</th>
      <th>IsActiveMember</th>
      <th>EstimatedSalary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.455346</td>
      <td>Germany</td>
      <td>Female</td>
      <td>-0.376792</td>
      <td>0.677301</td>
      <td>-0.391014</td>
      <td>-0.897814</td>
      <td>1</td>
      <td>0</td>
      <td>1.047721</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.565183</td>
      <td>Germany</td>
      <td>Male</td>
      <td>0.289748</td>
      <td>1.023136</td>
      <td>1.439829</td>
      <td>0.829508</td>
      <td>1</td>
      <td>1</td>
      <td>-0.777233</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-1.111636</td>
      <td>France</td>
      <td>Female</td>
      <td>-0.757672</td>
      <td>-0.360202</td>
      <td>0.985234</td>
      <td>-0.897814</td>
      <td>0</td>
      <td>0</td>
      <td>-0.664527</td>
    </tr>
  </tbody>
</table>
</div>



### 2-2 문자형 인코딩 (Label, OneHot, getdummies)


```python
x_train_col = pd.get_dummies(x_train[COL_CAT])
x_test_col = pd.get_dummies(x_test[COL_CAT])
```


```python
x_train = pd.concat([x_train[COL_NUM], x_train_col], axis = 1)
x_test = pd.concat([x_test[COL_NUM], x_test_col], axis = 1)
```

## 3. 모델링
### 3-1 데이터 분할


```python
from sklearn.model_selection import train_test_split

x_tr, x_val, y_tr, y_val = train_test_split(x_train, y_train, test_size = 0.3, stratify = y_train)
```

### 3-2 모델링


```python
from sklearn.ensemble import RandomForestClassifier
import warnings
warnings.filterwarnings('ignore')

model_rf = RandomForestClassifier()
model_rf.fit(x_tr, y_tr)
pred = model_rf.predict(x_val)
pred_proba = model_rf.predict_proba(x_val)[:,1]  # 0 : 0일때의 확률, 1: 1일때의 확률, 두개 더하면 1
print(pred, pred_proba)
```

    [0 1 0 ... 1 1 0] [0.02 0.6  0.02 ... 0.84 0.85 0.09]


## 4. 평가


```python
from sklearn.metrics import confusion_matrix, accuracy_score, recall_score, precision_score, f1_score, roc_auc_score
```


```python
confusion = confusion_matrix(y_val, pred)
confusion
```




    array([[1501,   52],
           [ 213,  184]])




```python
accuracy = accuracy_score(y_val, pred)
accuracy
```




    0.8641025641025641




```python
recall = recall_score(y_val, pred)
recall
```




    0.4634760705289673




```python
precision = precision_score(y_val, pred)
precision
```




    0.7796610169491526




```python
f1 = f1_score(y_val, pred)
f1
```




    0.5813586097946288




```python
roc = roc_auc_score(y_val, pred_proba)
roc
```




    0.8476070528967254



평가 지표들을 확인해 봤을때, Exit 값이 기본적으로 0이 많기 때문에 정확도 또한 높게 나온것이므로, 신뢰할 수 없다.  
recall 값이 0.4로 낮고 precision 값이 0.77이 나오는게 편차가 크므로 thresholds를 수정해보도록 한다.

### Binarizer 
임계값 x를 기준으로 각 값이 x보다 크거나 같으면 1, 작으면 0을 반환하는 객체  
from sklearn.preprocessing import Binarizer  
binarizer = Binarizer(threshold = custom_thresholds).fit(pred_proba)  
custom_pred = binarizer.transfrom(pred_proba)  # pred 라는 것이 결국 proba의 값이 경계값보다 높은것을 1로 반환한 것이기 떄문


```python
import numpy as np
from sklearn.preprocessing import Binarizer

custom_thresholds = np.arange(0.1,0.9,0.1)  # thresholds 값 설정

for i in custom_thresholds :
    print(i)
    binarizer = Binarizer(threshold = i).fit(pred_proba.reshape(-1,1))
    custom_pred = binarizer.transform(pred_proba.reshape(-1,1))
    # print(pred_proba)
    # print(custom_pred)
    print(f'thresholds : {i} 일때, 재현율 : {recall_score(y_val, custom_pred)}')
    print(f'thresholds : {i} 일때, 정확도 : {precision_score(y_val, custom_pred)}')
```

    0.1
    thresholds : 0.1 일때, 재현율 : 0.8765743073047859
    thresholds : 0.1 일때, 정확도 : 0.31607629427792916
    0.2
    thresholds : 0.2 일때, 재현율 : 0.7783375314861462
    thresholds : 0.2 일때, 정확도 : 0.43829787234042555
    0.30000000000000004
    thresholds : 0.30000000000000004 일때, 재현율 : 0.672544080604534
    thresholds : 0.30000000000000004 일때, 정확도 : 0.5816993464052288
    0.4
    thresholds : 0.4 일때, 재현율 : 0.5541561712846348
    thresholds : 0.4 일때, 정확도 : 0.6875
    0.5
    thresholds : 0.5 일때, 재현율 : 0.4634760705289673
    thresholds : 0.5 일때, 정확도 : 0.7796610169491526
    0.6
    thresholds : 0.6 일때, 재현율 : 0.3602015113350126
    thresholds : 0.6 일때, 정확도 : 0.8511904761904762
    0.7000000000000001
    thresholds : 0.7000000000000001 일때, 재현율 : 0.23929471032745592
    thresholds : 0.7000000000000001 일때, 정확도 : 0.8962264150943396
    0.8
    thresholds : 0.8 일때, 재현율 : 0.12090680100755667
    thresholds : 0.8 일때, 정확도 : 0.8888888888888888


threshold 값이 증가할수록, recall 은 감소하며, precision 값은 증가하는 것을 알 수 있다.  
다만, 0.4정도로 값을 변경해도 재현율과 정밀도가 충분히 높지 않아서 전처리 및 모델링을 다르게 해야 할 것 같다.
