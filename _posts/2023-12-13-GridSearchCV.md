---
title: 'GridSearchCV를 이용한 하이퍼파라미터 튜닝'
categories: MachineLearning
tags: [machinelearning, gridsearchcv, decisiontree, hyperparameter]
---

# GridSearchCV
교차 검증을 기반으로 하이퍼 파라미터의 최적 값을 찾게 해준다.

``` python
grid_parameters = {'max_depth' : [1,2,3],
                   'min_samples_split' : [2,3]}
```

하이퍼파라미터 조합이 총 2*3 = 6개가 나오게 된다.  
여기서 CV(Cross-Validation) = 3 이라면, 개별 파라미터 조합마다 3개의 폴딩 세트를 학습/평가해 평균값으로 성능을 측정 (총 18번 연산)

## 파라미터
1. estimator : 알고리즘
2. param_grid : 튜닝을 위한 파라미터 딕셔너리 세트
3. scoring : 예측 성능을 측정할 평가 방법 (acurracy 등)
4. cv : 교차 검증을 위해 분할되는 학습/테스트 세트의 개수
5. refit : 가장 최적의 하이퍼 파라미터를 찾은 뒤 입력된 estimator 객체를 해당 하이퍼 파라미터로 재학습 (default = True)

## 예제 (iris)


```python
import pandas as pd
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import GridSearchCV
from sklearn.model_selection import train_test_split

iris_data = load_iris()
x_tr,x_val,y_tr,y_val = train_test_split(iris_data.data, iris_data.target, test_size = 0.2, random_state = 121)

# GridSearchCV 에 쓰일 estimator 생성
dtree = DecisionTreeClassifier()

# GridSearchCV 의 하이퍼파라미터 딕셔너리
hyper_param = {'max_depth' : [1,2,3], 'min_samples_split' : [2,3]}

# dtree 에 최적의 하이퍼파라미터 재학습
grid_dtree = GridSearchCV(dtree, param_grid = hyper_param, cv = 3)

grid_dtree.fit(x_tr,y_tr)

# GridSearchCV 결과 추출
scores_df = pd.DataFrame(grid_dtree.cv_results_)
print(scores_df)
```

       mean_fit_time  std_fit_time  mean_score_time  std_score_time  \
    0       0.000667      0.000943         0.000333    4.714827e-04   
    1       0.000000      0.000000         0.000333    4.713704e-04   
    2       0.000666      0.000471         0.001000    4.899036e-07   
    3       0.000333      0.000471         0.000667    4.714266e-04   
    4       0.000667      0.000471         0.000334    4.719323e-04   
    5       0.000334      0.000472         0.000667    4.714831e-04   
    
      param_max_depth param_min_samples_split  \
    0               1                       2   
    1               1                       3   
    2               2                       2   
    3               2                       3   
    4               3                       2   
    5               3                       3   
    
                                         params  split0_test_score  \
    0  {'max_depth': 1, 'min_samples_split': 2}              0.700   
    1  {'max_depth': 1, 'min_samples_split': 3}              0.700   
    2  {'max_depth': 2, 'min_samples_split': 2}              0.925   
    3  {'max_depth': 2, 'min_samples_split': 3}              0.925   
    4  {'max_depth': 3, 'min_samples_split': 2}              0.975   
    5  {'max_depth': 3, 'min_samples_split': 3}              0.975   
    
       split1_test_score  split2_test_score  mean_test_score  std_test_score  \
    0                0.7               0.70         0.700000    1.110223e-16   
    1                0.7               0.70         0.700000    1.110223e-16   
    2                1.0               0.95         0.958333    3.118048e-02   
    3                1.0               0.95         0.958333    3.118048e-02   
    4                1.0               0.95         0.975000    2.041241e-02   
    5                1.0               0.95         0.975000    2.041241e-02   
    
       rank_test_score  
    0                5  
    1                5  
    2                3  
    3                3  
    4                1  
    5                1  
    

위의 결과 해석
1. params : 수행되는 개별 하이퍼파라미터 값
2. rank_test_score : 하이퍼파라미터별 성능 순위 (1이 가장 높다.)
3. mean_test_score : cv 수만큼 진행한 평가 평균값
4. split_test_score : 각 cv(교차검증)의 평가값


```python
print(f'최적 파라미터 : {grid_dtree.best_params_}')
print(f'최고 정확도 : {grid_dtree.best_score_}')
print(f'refit으로 이미 반영된 estimator : {grid_dtree.best_estimator_}')
```

    최적 파라미터 : {'max_depth': 3, 'min_samples_split': 2}
    최고 정확도 : 0.975
    refit으로 이미 반영된 estimator : DecisionTreeClassifier(max_depth=3)
    

참고로 min_samples_split의 원래 default 값이 2이므로 따로 estimator 에는 나타나지 않은 것 같다.


```python
from sklearn.metrics import accuracy_score

# 테스트 데이터로 예측값을 얻을떄는, 이렇게 얻어진 best_estimator_를 다시 알고리즘에 학습시켜야한다. 
estimator = grid_dtree.best_estimator_
y_pred = estimator.predict(x_val)

accuracy = accuracy_score(y_val, y_pred)
print(f'정확도 : {accuracy:.4f}')

```

    정확도 : 0.9667
    
