# 제2회 소방 안전 AI 예측 경진 대회

- 수행 기간 : 2022. 11. 14. ~ 11. 30.

- 제출일 : 2022. 11. 30.

- [**바로 가기**](http://www.dataslab.co.kr/aicon)

---

## 💁‍♂️ 프로젝트 소개

<details><summary><h3>문제의 이해</h3></summary>

</details>

<details><summary><h3>제공된 데이터 셋의 이해</h3></summary>

- `dataset.csv`
  - **자료(row)** : 총 302,168개

  - **변수(column)** : 총 65가지
    - 지역정보 (5개 칼럼)
    - 발생일자정보
    - 유동인구정보 (28개 칼럼)
    - 사고유형별 소방지수정보 (31개 칼럼)

  - **결측치** : 존재하지 않음

</details>

<details><summary><h3>풀이 과정</h3></summary>

1. **DF_OG** : 원 데이터 셋 파악하기

2. **DF_CLEAN** : 데이터 클렌징

3. **DF_EDA** : 탐색적 자료 분석 및 시각화

4. **DF_API** : 가설 설정 및 해당 가설에 부합하는 외부 API 추가
  
5. **DF_PRE** : 분류분석을 위한 전처리
  
6. **DF_ML** : 분류분석 알고리즘을 통한 분류 모델 설계
  
7. **DF_PREDICT** : 설계된 모델을 통한 예측

</details>

---

## 🔎 DF_OG; 원 데이터 셋 탐색하기

<details><summary><h3>수치형 변수</h3></summary>

- **유동인구정보 (28개 칼럼)**
  - 자료형 : 숫자형
  
  - 정보 : 통신사 제공 자료를 토대로 측정한 해당 row의 성별 및 연령별 유동인구수
  
  - 이상치 존재함
    - 4사분위수와 1, 2, 3사분위수 간 격차가 상당함
    - 모든 칼럼의 4사분위수가 네 자릿수 이상임
    - 이에 반해 3사분위수는 한 자릿수임
  
  - 세부 칼럼 목록
    - 남성 : `M00`, `M10`, `M15`, …, `M70` (14개)
    - 여성 : `F00`, `F10`, `F15`, …, `F70` (14개)

- **사고유형별 소방지수정보 (31개 칼럼)**
  - 자료형 : 숫자형
  
  - 정보 : 해당 row의 사고유형별 소방차량 출동횟수
  
  - 이상치 존재함
    - 모든 칼럼의 4사분위수는 2건을 넘지 않음
    - 모든 칼럼의 3사분위수는 0임
    - 즉, 0건의 비율이 매우 높음
    - 또한 하루 동안 동일 격자에 동일 사고유형으로 소방차량이 출동한 횟수는 최대 2건을 넘지 않음
  
  - 세부 칼럼 목록
<div align="center">

| 순번 | 사고유형(영문) | 사고유형(한글) | 비고 |
|---|---|---|---|
| 0 | HGTPOJ_ACDNT_OCRN_CNT | 고온체사고 |  |
| 1 | PNTRINJ_OCRN_CNT | 관통상 |  |
| 2 | MCHN_ACDNT_OCRN_CNT | 기계사고 |  |
| 3 | ETC_OCRN_CNT | 기타 |  |
| 4 | BLTRM_OCRN_CNT | 둔상 |  |
| 5 | ACDNT_INJ_OCRN_CNT | 사고부상 |  |
| 6 | EXCL_DISEASE_OCRN_CNT | 질병외 |  |
| 7 | VHC_ACDNT_OCRN_CNT | 탈것사고 |  |
| 8 | HRFAF_OCRN_CNT | 낙상 |  |
| 9 | AGRCMCHN_ACDNT_OCRN_CNT | 농기계사고 | 연간 총소방지수 0 |
| 10 | DRKNSTAT_OCRN_CNT | 단순주취 |  |
| 11 | ANML_INSCT_ACDNT_OCRN_CNT | 동물곤충사고 |  |
| 12 | FLPS_ACDNT_OCRN_CNT | 동승자사고 |  |
| 13 | UNKNWN_OCRN_CNT | 미상 |  |
| 14 | PDST_ACDNT_OCRN_CNT | 보행자사고 |  |
| 15 | LACRTWND_OCRN_CNT | 열상 |  |
| 16 | MTRCYC_ACDNT_OCRN_CNT | 오토바이사고 |  |
| 17 | THML_DAMG_OCRN_CNT | 온열손상 |  |
| 18 | DRV_ACDNT_OCRN_CNT | 운전자사고 |  |
| 19 | DRWNG_OCRN_CNT | 익수 |  |
| 20 | PRGNTW_ACDNT_OCRN_CNT | 임산부사고 |  |
| 21 | BCYC_ACDNT_OCRN_CNT | 자전거사고 |  |
| 22 | ELTRC_ACDNT_OCRN_CNT | 전기사고 |  |
| 23 | POSNG_OCRN_CNT | 중독 |  |
| 24 | ASPHYXIA_OCRN_CNT | 질식 |  |
| 25 | FALLING_OCRN_CNT | 추락 |  |
| 26 | FLAME_OCRN_CNT | 화염 |  |
| 27 | CHMC_SBSTNC_ACDNT_OCRN_CNT | 화학물질사고 |  |
| 28 | WETHR_ACDNT_OCRN_CNT | 날씨사고 | 연간 총소방지수 0 |
| 29 | SXAL_ASALT_OCRN_CNT | 성폭행 | 연간 총소방지수 0 |
| 30 | BURN_OCRN_CNT | 화상 |  |  

</div>
</details>

<details><summary><h3>범주형 변수</h3></summary>

- **지역정보 (5개 칼럼)**
  
  - `GRID_ID`
    - 자료형 : 숫자형
    - 정보 : 공모전 주최 측에서 임의로 설정한 강원도 원주시 지역구분코드
    - 고유값 856개
  
  - `GRID_X_AXIS`
    - 자료형 : 숫자형
    - 정보 : 해당 row의 `GRID_ID`가 가리키는 X축 좌표
    - 고유값 41개
  
  - `GRID_Y_AXIS`
    - 자료형 : 숫자형
    - 정보 : 해당 row의 `GRID_ID`가 가리키는 Y축 좌표
    - 고유값 40개
  
  - `DONG_NM`
    - 자료형 : 문자열
    - 정보 : 해당 row의 실제 지역구분명(동/리 단위)
    - 고유값 74개
  
  - `DONG_CD`
    - 자료형 : 숫자형
    - 정보 : 해당 row의 실제 지역구분코드
    - 고유값 73개

- **발생일자정보 (1개 칼럼)**
  
  - `OCRN_YMD`
  
    - 자료형 : 문자열
    - 정보 : 해당 row의 발생일자 (년/월/일)
    - 12월에 해당하는 자료가 누락되어 있음

</details>
  
<details><summary><h3>설명변수와 종속변수 구분</h3></summary>

- **설명변수(Feature Columns)**
  
  - 지역정보
  - 발생일자정보
  - 유동인구정보

- **종속변수(Target Columns)**
  
  - 각 사고유형별 소방지수
  
</details>

---

## DF_CLEAN

<details><summary><h3>지역정보</h3></summary>

- `GRID_ID` : 최종으로 예측해야 할 정보이므로 남겨둠

- `GRID_X_AXIS`, `GRID_Y_AXIS` : 지도 시각화 이후 삭제 예정

- `DONG_NM`, `DONG_CD` : 외부 API 추가 이후 삭제 예정

</details>

<details><summary><h3>발생일자정보</h3></summary>

- 변수 `OCRN_YMD`를 네 개 변수로 세분화함
  - `MONTH`, `WEEKDAY`, `SEASON`, `HOLIDAY`

- `YEAR` : 발생년도에 관한 정보
  
  - 예측하고자 하는 일자에 대하여 유의미한 정보를 제공한다고 볼 수 없으므로 삭제함
    - 모든 자료의 발생년도는 2021년임

- `MONTH` : 발생월에 관한 정보

      1, 2, 3, ..., 12

- `DAY` : 발생일에 관한 정보
  - 해당 변수는 범주형 변수에 해당하므로 값의 크기가 하니라 고유값이 중요함
  - 예측하고자 하는 일자에 대하여 유의미한 정보를 제공한다고 볼 수 없으므로 삭제함
    - 예측하고자 하는 일자
    
          28(2월)
          30(4, 6, 9, 11월)
          31(1, 3, 5, 7, 8, 10, 12월)
    
    - 해당 변수가 제공하고 있는 정보
    
          1, 2, ..., 27(1~12월)
          28, 29(1, 3, ..., 12월)
          30(1, 2, 3, 5, 7, 8, 10, 12월)

- `SEASON` : 발생계절에 관한 정보

      0 : 봄(3, 4, 5월)
      1 : 여름(6, 7, 8월)
      2 : 가을(9, 10, 11월)
      3 : 겨울(12, 1, 2월)

- `HOLIDAY` : 휴일여부에 관한 정보

      0 : 휴일아님(공휴일이 아닌 평일)
      1 : 휴일임(공휴일 및 주말)
    
</details>

<details><summary><h3>유동인구정보</h3></summary>
  
</details>

---

## DF_EDA

<details><summary><h3>설명변수와 총소방지수 간 상관관계 확인</h3></summary>
  
</details>

<details><summary><h3>사고유형별 총소방지수 비교</h3></summary>
  
</details>

<details><summary><h3>사고유형당 기간별 소방지수 비교</h3></summary>
  
</details>

<details><summary><h3>사고유형 건당 평균 유동인구수 비교</h3></summary>
  
</details>

---

## DF_API

<details><summary><h3>산업단지정보</h3></summary>
  
</details>

<details><summary><h3>유흥시설정보</h3></summary>
  
</details>

<details><summary><h3>교통 관련 정보</h3></summary>
  
</details>

---

## DF_PRE

<details><summary><h3>주요설명변수 선별 전 불필요한 변수 삭제</h3></summary>
  
</details>

<details><summary><h3>범주형 변수 전처리</h3></summary>
  
</details>

<details><summary><h3>수치형 변수 전처리</h3></summary>
  
</details>

<details><summary><h3>종속변수를 이진범주형으로 변환</h3></summary>
  
</details>

<details><summary><h3>데이터 셋 분할</h3></summary>
  
</details>

<details><summary><h3>로지스틱 회귀분석을 통한 주요설명변수 채택</h3></summary>
  
</details>

---

## 👭 WORKMATE

👨 [**IT`S ME**](https://github.com/jayarnim)

    - Exploratory Data Analysis
    - Predict
    - README

👩 [**김효정**](https://github.com/410am)

    - Logistic Regression
    - Predict
    - README

👨 [**인찬휘**](https://github.com/wassaa-1)

    - DecisionTree Classifier
    - Random Forest Classifier
    - GBM Classifier
    - Light GBM Classifier

<br>

---

## 🛠 SKILL USED

### 👉 LANGUAGE

<img alt="Python" src="https://img.shields.io/badge/python%20-%2314354C.svg?style=for-the-badge&logo=python&logoColor=white"/>

### 👉 IDE

<img src="https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=Google Colab&logoColor=white"/>

### 👉 LIBRARY

<img src="https://img.shields.io/badge/numpy-013243?style=for-the-badge&logo=numpy&logoColor=white"/> &nbsp;
<img src="https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white"/> &nbsp;
<img src="https://img.shields.io/badge/scikitlearn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white"/>

<br>

---

## 💡 자료 출처

- [**bike-sharing-demand (kaggle)**](https://www.kaggle.com/c/bike-sharing-demand/overview)
