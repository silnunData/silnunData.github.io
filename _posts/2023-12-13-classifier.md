---
title: '분류(Classifier)의 이해와 결정트리(Decision Tree), 앙상블(Ensemble)'
categories: MachineLearning
tag: [machinelearning, classifier, decisiontree, randomforest]
---


# 분류

## 결정트리(Decision Tree)

많은 규칙이 있다는 것은, 분류를 결정하는 방식이 복잡해진다는 것이고, 이것은 과적합으로 이어진다. (즉, 트리의 깊이(depth)가 깊어질수록 예측 성능이 떨어진다.)  
그래서 데이터를 분류할 떄 최대한 많은 데이터 세트가 해당 분류에 속할 수 있도록 해야한다. (즉, **최대한 균일한** 데이터 세트를 구성할 수 있도록 분할 해야한다.)

### 지니계수
정보의 균일도를 측정하는 대표적인 방식으론 엔트로피, 지니 계수가 있는데,    
Decision Tree 에서는 **지니계수(GIni)** 사용한다.
> 지니계수 : 불평등을 나타내는 지수, 0이 가장 평등 1로 갈수록 불평등, 즉 지니계수가 낮을 수록 균일하다.

### 결정 트리의 장단점
장점 : 쉽고 직관적이며, 피쳐의 스케일링이나 정규화 등의 사전 가공 영향도가 적다.  
단점 : 과적합으로 성능이 떨어진다. (트리의 크기를 사전에 제한하는 튜닝 필요)

### 결정 트리의 파라미터
1. min_samples_split : 노드를 분할하기 전, 분할하기 위한 최소한의 샘플 데이터 수 (default = 2)
2. min_samples_leaf : 노드를 분할한 후, 좌우 노드가 가지는 최소한의 샘플 데이터 수 (충족이 안되면 분할을 안한다.)
3. max_features : 분할을 위해 고려하는 최대 피처(컬럼) 개수 (default = NONE, 전체)
4. max_depth : 트리의 최대 깊이 (default = NONE, 끝까지)
5. max_leaf_nodes : 말단 노드의 최대 개수
- feature_importances_ : 피처별(컬럼별) 알고리즘에서의 중요도

## 앙상블(ensemble)
여러 개의 분류기를 생성하고 그 예측을 결합하여 보다 정확한 최종 예측을 도출하는 기법  
(캐글에서 가장 매력적인 알고리즘이 부스팅 기법인 XGBoost 와 LightGBM 이다.)  

### 보팅, 배깅
보팅 : 동일 dataset에서, 서로 다른 알고리즘 가진 분류기 결합  
배깅 : 다른 data sample에서(bootstrap된), 모두 같은 유형의 분류기 결합 (대부분 결정트리 알고리즘, 대표적으로 랜덤포레스트)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FkAWZ0%2FbtrFLCyDRkE%2FKfjMkKzgImKVJWrQwinEk1%2Fimg.png)


#### 랜덤포레스트(RandomForest) 파라미터
1. n_estimators : 결정 트리의 개수 (default = 10)
2. max_features : 분할을 위해 고려하는 최대 피처 개수 **(default = auto(sqrt))** 
3. max_depth, min_samples_split, min_samples_leaf

### 부스팅
여러개의 분류기가 순차적으로 학습, 이전 분류기에서 예측이 틀린것에 대하여 올바르게 예측할 수 있도록 다음 분류기에 가중치 부여 (대부분 결정트리 알고리즘)  
(부스팅에 대해서는 다음번에 포스팅 하도록 하겠다.)
