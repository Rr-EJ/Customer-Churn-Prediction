# 전기 요금 기반 이탈 예측

##  프로젝트 배경 및 목적

파워코(PowerCo)는 BCG의 클라이언트로, 중소기업(SME)과 주거 고객에게 가스와 전기를 공급하는 에너지 회사입니다.  
최근 유럽 시장의 전력 자유화(Power Liberalization)와 가격 경쟁 심화로, 특히 SME 고객층에서 고객 이탈(Churn)률이 증가하고 있습니다.

<img width="659" alt="reuter-1" src="https://github.com/user-attachments/assets/eda46139-5a46-462e-b157-9920afbfc204" />


### 프로젝트 목표

- 고객의 계약, 이용 패턴, 요금, 마진, 마케팅 이력 등 다양한 특성과 이탈 간 관계를 데이터 기반으로 분석  
- 고객 이탈 확률(Churn Probability)을 예측하는 분류 모델 개발  
- 예측 결과를 활용해 효과적인 마케팅 전략(예: 할인 캠페인 타겟팅) 수립에 필요한 인사이트 도출


 
##  기술 스택

| 영역 | 사용 기술 |
|------|-----------|
| **AI & 데이터 처리** | ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![](https://img.shields.io/badge/numpy-013243?style=for-the-badge&logo=numpy&logoColor=white) ![](https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white) ![](https://img.shields.io/badge/scikitlearn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white) |
| **머신러닝 모델** | ![](https://img.shields.io/badge/RandomForest-00B050?style=for-the-badge) ![](https://img.shields.io/badge/DecisionTree-228B22?style=for-the-badge) ![](https://img.shields.io/badge/XGBoost-EC0000?style=for-the-badge&logo=xgboost&logoColor=white) ![](https://img.shields.io/badge/LightGBM-3C3C3C?style=for-the-badge&logo=lightgbm&logoColor=white) ![](https://img.shields.io/badge/CatBoost-FFCC00?style=for-the-badge) ![](https://img.shields.io/badge/SGDClassifier-006699?style=for-the-badge) ![](https://img.shields.io/badge/Logistic%20Regression-1E90FF?style=for-the-badge) ![](https://img.shields.io/badge/SVM-8A2BE2?style=for-the-badge) |
| **버전 관리 및 협업** | ![](https://img.shields.io/badge/github-181717?style=for-the-badge&logo=github&logoColor=white) ![](https://img.shields.io/badge/discord-5865F2?style=for-the-badge&logo=discord&logoColor=white) |

---

## 데이터셋 소개
본 프로젝트에서는 **Kaggle PowerCo 고객 이탈 예측** 데이터셋을 활용하였습니다.  
유럽 지역 SME(중소기업)와 주거용 고객을 대상으로 전기·가스를 공급하는 PowerCo사의 실제 거래 및 이탈 이력 데이터를 기반으로 합니다.  
이탈 예측 모델링을 통해 **“이탈 위험 고객”에게 할인(예: 20%) 등 차별적 마케팅 전략을 적용할지 판단**하고자 합니다.

### 데이터 구성 파일
| 파일명 | 설명 |
|--------|------|
| **ml_case_training_data.csv** | 고객 기본 정보 |
| **ml_case_training_hist_data.csv** | 고객별 월별 계약 가격 정보 (변동·고정 단가 포함) |
| **ml_case_training_output.csv** | 타깃 변수 (이탈 여부) |



##  주요 변수 요약
- **범주형 변수**: `channel_sales`, `activity_new`, `origin_up`, `has_gas` 등  
- **수치형 변수**: `cons_last_month`, `cons_12m`, `nb_prod_act`, `net_margin`, `margin_net_pow_ele`, `num_years_antig` 등  
- **타깃 변수**: `churn` (0 = 유지, 1 = 이탈, 불균형 분포 9:1)



## 데이터 인사이트
- **소비량**: 최근 사용량이 적은 고객에서 이탈률 높음  
- **상품 개수**: 상품 수가 적을수록 이탈률 높음  
- **마진**: 순마진이 낮은 고객에서 이탈률 높음  
- **가입 연수**: 신규 고객일수록 이탈 위험이 큼  

##  기존 사례 비교
| Dataset | 주요 특징 | 적용 모델 | 성능 |
|---------|-----------|-----------|------|
| PowerCo | 결측치 처리, 이탈률 분석 | XGBoost | Accuracy 0.98 |
| Telco Churn | 계약 유형·요금 분석, SMOTE 적용 | Random Forest, SVM | Accuracy 0.89 |
| Bank Churn | 신용카드 고객 이탈 분석 | Random Forest, AdaBoost | Accuracy 0.90 |

##  지표 설정
- 목표: **Recall**이 높은 모델 → 실제 이탈 고객을 최대한 놓치지 않음  
- 마케팅 비용(c)과 매출 증가(r)을 고려했을 때, **Recall 중심 평가**가 가장 합리적임  

##  모델 성능 비교 (SMOTE 적용 후)
| Model | Accuracy | Recall | Precision | F1-score |
|-------|----------|--------|-----------|----------|
| Random Forest | 0.9999 | 0.9993 | 0.9999 | 0.9988 |
| Extra Trees | 0.9998 | 0.9981 | 0.9999 | 0.9990 |
| Decision Tree | 0.9985 | 0.9931 | 0.9916 | 0.9924 |
| CatBoost | 0.9941 | 0.9822 | 0.9594 | 0.9707 |
| XGBoost | 0.9649 | 0.9132 | 0.7736 | 0.8375 |
| LightGBM | 0.8880 | 0.7693 | 0.4608 | 0.5763 |


##  결론
- **Random Forest, Extra Trees** 모델이 가장 높은 Recall과 안정적인 성능을 보임  
- 불균형 데이터 문제를 해결하기 위해 **SMOTE 증강**을 적용  
- 최종적으로 **Recall 중심 모델 선택 → 마케팅 전략 타겟팅에 활용 가능**

---

## 📊 마케팅 가치 평가

###  모델 해석 및 주요 Feature 중요도
모델에서 중요도가 높은 상위 3개 변수:
1. **origin_up** : 최초 가입 전기 캠페인 (원핫 인코딩)  
2. **margin_net_pow_ele** : 전력 가입 순마진  
3. **cons_last_month** : 직전 월 소비량  


###  주요 Feature별 마케팅 인사이트

| Feature | 그래프 해석 | 마케팅 전략 |
|---------|-------------|-------------|
| **origin_up** (최초 가입 캠페인) | 특정 캠페인 경로로 유입된 고객들의 이탈률이 높음 | 해당 경로 고객에게 추가 할인 제공, 다른 안정적인 경로로 유도 |
| **margin_net_pow_ele** (전력 가입 순마진) | 고마진 고객(1구간)의 이탈률이 높음 / 저마진 고객(5구간)의 이탈률은 낮음 | 고마진 고객 → 맞춤형 혜택 제공으로 유지<br>저마진 고객 → 혜택 과잉 여부 분석, 가격 인상 고려 |
| **cons_last_month** (직전 월 소비량) | 사용량 많은 고객은 이탈 거의 없음 / 사용량 적은 고객은 이탈률 높음 | 고사용량 고객 → 우수 고객 관리, 혜택 유지<br>저사용량 고객 → 타겟 리마케팅, 미끼 상품 제안 |


###  결론
- **Recall이 높은 모델**을 목표로 설정하여 실제 이탈 고객을 놓치지 않는 전략이 필요  
- 분석 결과, **가입 경로·마진·소비량**이 이탈 여부와 밀접한 관련이 있음  
- 따라서 마케팅 전략은 **고마진·저소비량 고객을 집중 관리**하고, **특정 가입 경로 고객에게 맞춤형 혜택 제공**하는 방향으로 수립하는 것이 효과적임
