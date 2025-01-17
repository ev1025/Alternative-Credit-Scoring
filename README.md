# 대안 신용평가 모델을 이용한 신용카드 연체여부 예측   

### 프로젝트 기간
- 2024년 12월 27일 ~ 2025년 1월 8일 (약 5일)

### 참여자
- 안정윤, 강성규, 조서형, 이진우

## 1. 프로젝트 배경

&nbsp; IT 기술의 발전에 따라 금융(Finance)과 기술(Technology)의 결합을 의미하는 **핀테크**(FinTech)가 빠르게 성장하고 있다. 하지만 사회 제도가 이러한 발전 속도를 따라잡지 못하면서, 핀테크의 복잡성을 이해하지 못한 채 소외되는 사람들이 많아졌다. 그 대표적인 예시가 바로 **신용평가**이다.   

### 신용평가란?
&nbsp; **신용평가**란 자금을 빌리는 차입자 등의 부채 상환능력이나 신용도를 일정한 등급으로 평가하는 것을 말한다. 은행 등 금융기관은 고객에게 대출을 해줄 때 고객의 신용등급을 평가한다. **개인신용평가제도**(CSS: Credit Scoring System)는 개인의 인적사항, 소득, 금융기관 거래 및 대출실적 등 신용정보를 토대로 신용등급을 매기고, 대출 여부, 대출 가능 금액, 적용 금리를 산정한다.

### 기존 신용평가의 장점과 한계

- **장점** : 오랜 기간 동안 사용되어 온 기존의 신용평가는 검증된 시스템으로 안정적으로 평가가 가능하고 신뢰도가 높다.
- **한계** : **중저신용자**와 **신파일러**가 배제될 수 있다는 지적이 있다.

> -  **중저신용자**: 과거 신용등급 기준 4등급 이하, 현재는 신용점수 하위 50%에 해당하는 사람
> - **신파일러**: 상환 능력이 충분하나 금융거래 이력이 부족하여로 불리한 조건에 놓이는 사람
> - **나이스평가정보**에 따르면, **2019년 상반기 기준** 신파일러는 전체 신용점수 산정 대상자의 `27.8%`인 `1,278만 7,711명`이다.   
신파일러에는 `취업준비생`, `사회초년생`, `노년층` 등이 포함된다.

<br>

이 프로젝트에서는 **서울시민 통신데이터**와 **신용카드 이용데이터**를 비교하여 **대안신용평가모형**의 가능성을 연구했다.

---

<br>

## 2. 기대효과

### 신용평가의 확장

- **비금융 데이터를 활용**하여 신파일러와 중저신용자를 객관적이고 정확하게 평가
- **금융사**: 신규 고객 유치 및 기존 고객에게 더 나은 대출 조건 제공
- **효과**: 금융사의 수익성 증대

### 포용금융 실현

- 금융 서비스 이용이 어려웠던 사람들의 **접근성**을 높여 **사회적 불평등 완화**
- 경제적 기회 확대를 통해 다양한 고객층 확보
- **효과**: 고객 만족도 향상, 금융사의 성장 및 혁신

---

<br>

## 3. 데이터 개요
### [1) 서울 시민생활 데이터](https://data.seoul.go.kr/dataVisual/seoul/seoulLiving.do)  
**데이터 소개**  
서울시와 SK텔레콤이 공공빅데이터와 통신데이터를 가명 결합하여 추정한 데이터로, 서울 행정동 단위 성·연령별 1인 가구 및 서울시민의 생활 특성 정보를 제공합니다.  

- **데이터 형식**: xlsx  
- **데이터 크기**: (50880, 143)  
- **데이터 한계점**  
  - 각 열이 개별 1인의 정보가 아니라 동네별 평균값으로 기록된 데이터
  - 연체율 평균 이상을 연체자(1), 평균 이하를 미연체자(0)로 분류하여 라벨링  

- **사용 변수**  
```
'야간상주지 변경횟수 평균', '주간상주지 변경횟수 평균', '소액결재 사용횟수 평균', 'SNS 사용횟수', '게임 서비스 사용일수',
'쇼핑 서비스 사용일수', '소액결재 사용금액 평균', '데이터 사용량', '유튜브 사용일수', '넷플릭스 사용일수', '배달 서비스 사용일수',
'금융 서비스 사용일수', '동영상/방송 서비스 사용일수', '배달_브랜드 서비스 사용일수', '배달_식재료 서비스 사용일수'
```

### [2) AI허브 금융합성 데이터](https://www.aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=data&dataSetSn=71792)  
**데이터 소개**  
금융 AI 학습 및 데이터 분석에 활용할 수 있도록, 금융기관으로부터 획득한 데이터의 특성을 인공지능 알고리즘으로 학습해 생성한 합성 데이터입니다.  

- **데이터 형식**: csv  
- **데이터 크기**:  (1,200,000, 14)
- **데이터 한계점**  
  - 데이터 라벨로 사용한 `회원여부_연체` 컬럼에서 불균형 문제 
  - 6개월의 데이터를 병합한 후, 미연체자와 연체자의 비율을 74:26으로 조정하여 120만 개의 데이터를 샘플링  

- **사용 변수**
```
‘201807_회원정보’와 ‘201807_청구정보’에서 ‘최근 3개월 이용금액 체크카드’, ‘최근 3개월 이용금액 신용카드’,
‘1순위 카드 이용건수’, ‘이용카드수_신용’, ‘이용카드수_체크’ , ‘당월 청구금액’, ‘최근 6개월 청구금액’,
‘회원여부_연체’, ‘최근 6개월 연체건수’, ‘이용거절여부_카드론’, ‘신용카드 소지여부’, ‘카드신청건수’, 
```   
---

<br>

## 4. 데이터 분석
### 1) 데이터 전처리
- 1차적으로 팀원들이 모여 불필요한 변수 제거
- 이상치 일괄 제거    
![image](https://github.com/user-attachments/assets/685f05e1-e465-4f9f-a9c7-c30b52ac2aa0)


- **데이터 스케일링** : 완전히 다른 지표를 비교하는 것이기 때문에 `MinMaxScaling` 진행
- **데이터 분리** : train(60%), validation(20%), test(20%)    
- **불균형 데이터 처리** : `Undersampling`을 진행하여 라벨 불균형을 해소    
- 학습 데이터와 예측 데이터의 `공통 핵심 지표`를 설계한 뒤 핵심지표에 해당하는 변수를 포함
- 이후 성별과 연령 데이터를 원핫 인코딩 하여 병합

**학습 데이터**
```
alter_mapping = {
    '소비 빈도 및 활동성': [
        '야간상주지 변경횟수 평균', '주간상주지 변경횟수 평균', '소액결재 사용횟수 평균', 
        'SNS 사용횟수', '게임 서비스 사용일수', '쇼핑 서비스 사용일수'
    ],
    '소비 금액 및 규모': [
        '소액결재 사용금액 평균', '데이터 사용량', '유튜브 사용일수', 
        '넷플릭스 사용일수', '배달 서비스 사용일수'
    ],
    '서비스 이용 다양성': [
        '금융 서비스 사용일수', '동영상/방송 서비스 사용일수', 
        '배달_브랜드 서비스 사용일수', '배달_식재료 서비스 사용일수'
    ]
}
```
![image](https://github.com/user-attachments/assets/37b96707-7555-45bd-906a-78a8cfe2de81)

**예측 데이터**
```
credit_mapping = {
    '소비 빈도 및 활동성': [
        '이용금액_R3M_신용', '이용금액_R3M_체크', '_1순위카드이용건수',
        '이용카드수_신용', '이용카드수_체크'
    ],
    '소비 금액 및 규모': [
        '이용금액_R3M_신용_가족', '이용금액_R3M_체크_가족', '_1순위카드이용금액',
        '청구금액_B0', '청구금액_R6M'
    ],
    '서비스 이용 다양성': [
        '카드신청건수', 'KT', 'LGU+', 'SK'
    ]
}
```
![image](https://github.com/user-attachments/assets/4304da5c-121b-4a5a-9361-e28843cfdc5e)



### 2) 모델링
- **사용 모델** : `XGBoostclassifier`, `RandomForestClassifier`   
  - 두 모델은 데이터의 노이즈나 이상치에 강하다.
  - 다양한 특성을 가진 데이터를 효과적으로 학습할 수 있어 대안 신용 평가 모델에 적절하다고 판단   
- **하이퍼 파라미터 튜닝** : `GridSearchCV`를 사용하여 최적의 하이퍼 파라미터를 튜닝    



---

<br>

## 5. 모델 평가 및 결론
### 1) 직접 선별한 칼럼에 대한 학습결과
![Frame 4](https://github.com/user-attachments/assets/fe059274-8a14-4962-8e87-d0c2b4f35936)

### 2) 상관계수가 높은 칼럼들을 선별한 학습결과
![Frame 5](https://github.com/user-attachments/assets/902cf4fd-ffbc-4e4c-a064-d1bd084062a5)

### 3) 불균형 데이터 언더 샘플링 테스트 결과
![Frame 7](https://github.com/user-attachments/assets/34b7d2a1-439c-4d2f-83eb-187bd1791711)

### 4) 평가지표
![image](https://github.com/user-attachments/assets/1e6323ec-f854-4af3-a1ad-39671222b061)

### 5) 결론
- 서울 시민생활 데이터를 이용한 신용카드 연체 여부 예측은 불가능한 것으로 결론을 내렸다.
- 대안 데이터의 유효성을 검증을 위해서는 실제 신용 정보 데이터와 연결할 식별자가 있어야 하는데 데이터의 한계가 있었다.
- 서울 시민 생활 데이터는 개인 데이터가 아닌 동네 평균 데이터로 개인의 특성을 담아내기 어려워 설명력이 낮았다.
- 핵심지표 설정과 훈련 데이터의 라벨 생성을 임의로 설정하면서 실질적인 연관성을 도출하지 못했다.
