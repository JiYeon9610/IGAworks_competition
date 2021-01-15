# IGAworks_competition

2020.01 ~2020.02

-> 자세한 시각화 및 분석 내용은 '아이스얼그레이라떼.pdf' 파일 참고


## 분석 방향 및 시각화
- 분석 목적: 클릭율(CTR) 예측 모델 (광고 관련 정보 데이터와 사용자 데이터를 활용하여, 주어진 광고를 클릭할 확률을 예측하는 것)
- 모델의 지향점: 1.예측력(데이터에 적합한 모델 선택)  2.속도(데이터 차원 축소)  3.수집 정보 활용(정보 손실에 유의)

- 종속변수 Y: Click 변수(클릭한 경우 1, 안한 경우 0) -> 1인 경우가 10% 가량으로, unbalanced 데이터
- 설명변수 X: 총 22개로, 모두 암호화된 범주형 변수이기 때문에 one-hot-encoding 작업이 필요. 데이터의 대다수가 0인 Sparse 데이터


## 전처리
1. 줄이기 : 모델링에 사용될 각 input 변수의 범주 개수를 줄인다.(one-hot-encoding으로 차원이 크게 늘어나는 문제를 해결하기 위해)
2. 합치기 : 차원은 줄이되 정보 손실을 막기 위해 합친다

### 1. 줄이기

- 유사한 의미를 가지고 있는 변수들 중, 변수 중요도에 따라 낮은 경우 제거
- Feature Selection: 모델링에 사용할 변수의 범주를 줄이기 위해 information gain과 발생횟수를 고려하여 feature selection 후, 선택하지 않은 범주는 묶어 기타로 처리

*** information gain: Y 예측에 얼마나 기여하는가를 나타내는 정보.

### 2. 합치기

- AutoEncoder: 앱의 종류 약 242개에 대하여 사용자가 평점을 부여한 변수들(1.응답 유무에 대한 변수, 2.부여한 점수에 대한 변수)에 한하여, 각각 AutoEncoder를 사용하여 5차원으로 축소

*** 응답 유무를 cate_answer 컬럼으로 0, 1 처리 / 부여한 점수에 따라 cate_like 컬럼으로 무응답, 1, 2, 3점은 0으로, 4, 5점은 1로 처리한 후 오토인코더 진행


## 모델링
### xDeepFM 모델
extreme Deep Factorization Machine

deepFM에서 개선된 모델로, 명시적인 방식과 벡터단위로 feature interaction을 계산한다. deepFM에 비해 노이즈에 강하고, 성능이 좋은 편이다.
- 장점: 1.범주형과 연속형을 다룰 수 있다.  2.다차원의 sparse data에 대해서도 feature interaction 계산이 가능하다.
- 고차원의 spare 데이터에 강한 딥러닝 기반의 xDeepFM을 최종 모델로 선택
- batch size와 epoch를 튜닝하면서 최적의 파라미터 조합을 찾기 위해 노력
- 최종 리더보드 log loss 결과 0.24834



## 총정리
1. 주어진 데이터의 특징에 최적화된 모델을 사용하여 예측 시간을 단축
2. 고차원의 범주형 데이터의 차원축소와 정보손실 최소화를 동시에 이룸
3. 다양한 경우의 수를 조합하여 보다 정교한 모델링을 진행함
