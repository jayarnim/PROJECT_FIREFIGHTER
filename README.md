# 제2회 소방 안전 AI 예측 경진 대회

- 수행 기간 : 2022. 11. 14. ~ 11. 30.

- 제출일 : 2022. 11. 30.

- [**바로 가기**](http://www.dataslab.co.kr/aicon)

---

## 💁‍♂️ 프로젝트 소개

<details><summary><h3>문제의 이해</h3></summary>

</details>

<details><summary><h3>제공된 데이터 셋의 이해</h3></summary>

- **`dataset.csv`**
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

## 🔎 DF_OG

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

- **`GRID_ID`**
  - 2021년 동안 사고가 발생한 적이 없는 격자 616개
  - 2021년 동안 사고가 발생한 적이 있는 격자 240개
  - 연간 사고발생이 0건인 격자 삭제

- **`GRID_X_AXIS`, `GRID_Y_AXIS`** : 지도 시각화 이후 삭제 예정

- **`DONG_NM`, `DONG_CD`** : 외부 API 추가 이후 삭제 예정

</details>

<details><summary><h3>발생일자정보</h3></summary>

- **변수 `OCRN_YMD`를 네 개 변수로 세분화함**
  
      MONTH, WEEKDAY, SEASON, HOLIDAY

- **`YEAR`** : 발생년도에 관한 정보  
  - 예측하고자 하는 일자에 대하여 유의미한 정보를 제공한다고 볼 수 없으므로 삭제함
    - 모든 자료의 발생년도는 2021년임

- **`MONTH`** : 발생월에 관한 정보

      1, 2, 3, ..., 12

- **`DAY`** : 발생일에 관한 정보
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

- **`SEASON`** : 발생계절에 관한 정보

      0 : 봄(3, 4, 5월)
      1 : 여름(6, 7, 8월)
      2 : 가을(9, 10, 11월)
      3 : 겨울(12, 1, 2월)

- **`HOLIDAY`** : 휴일여부에 관한 정보

      0 : 휴일아님(공휴일이 아닌 평일)
      1 : 휴일임(공휴일 및 주말)
    
</details>

<details><summary><h3>유동인구정보</h3></summary>

- **성별 및 유사생활패턴에 따라 적절히 결합하여 7개 변수로 재구분함**
  
      PEOPLE, MAN, WOMAN, CHILD, YOUTH, MIDDLE, OLDER

- **`PEOPLE`** : 총유동인구

- **성별에 따른 구분**
  - `MAN` : 남성유동인구; `M00`, `M10`, `M15`, …, `M70`
  - `WOMAN` : 여성유동인구; `F00`, `F10`, `F15`, …, `F70`

- **유사생활패턴에 따른 구분**
  - `CHILD` : 미성년
    - 20세 미만 남성 및 여성
    - 미취학아동 및 초/중/고등학생으로서 보호자에 의해 활동이 제약되는 나이
  - `YOUTH` : 청년
    - 20세 이상 35세 미만 남성 및 여성
    - 자기결정권을 지니고 직장 등으로부터 비교적 자유로운 나이
  - `MIDDLE` : 중장년
    - 35세 이상 60세 미만 남성 및 여성
    - 경제권을 지니고 주로 직장에 상주하는 나이
  - `OLDER` : 노년
    - 60세 이상 남성 및 여성
    - 직장에서 은퇴하고 신체가 쇠약한 나이

</details>

<details><summary><h3>설명변수 종합</h3></summary>

- **범주형 변수**
  - 지역정보 : `GRID_ID`, ~`GRID_X_AXIS`~, ~`GRID_Y_AXIS`~, ~`DONG_NM`~, ~`DONG_CD`~
  - 발생일자정보 : `MONTH`, `WEEKDAY`, `SEASON`, `HOLIDAY`

- **수치형 변수**
  - 유동인구정보 : `PEOPLE`, `MAN`, `WOMAN`, `CHILD`, `YOUTH`, `MIDDLE`, `OLDER`

</details>

---

## DF_EDA

<details><summary><h3>설명변수와 총소방지수 간 상관관계 확인</h3></summary>

![상관계수](https://user-images.githubusercontent.com/116495744/206858084-63bd7d86-5ac2-4b07-a07a-4d45836689a3.png)

![1_기간별 총소방지수 비교](https://user-images.githubusercontent.com/116495744/206858096-a82cb69a-c431-4c41-af1e-e912a967f636.png)

</details>

<details><summary><h3>사고유형별 연간 총소방지수 비교</h3></summary>

![2-1_사고유형별 연간 총소방지수 비교](https://user-images.githubusercontent.com/116495744/206866048-fc723d71-2e5b-4e95-81a7-75e12f43901c.png)

- **`낙상(HRFAF_OCRN_CNT)` 연간 총소방지수가 다른 사고유형의 연간 총소방지수보다 압도적으로 높음**
  - `낙상(HRFAF_OCRN_CNT)` 연간 총소방지수는 `총소방지수(TOTAL_CNT)`의 약 34.70%를 차지함
    - 모든 사고유형의 소방지수를 종합한 `총소방지수(TOTAL_CNT)`는 2827건임
    - `낙상(HRFAF_OCRN_CNT)` 연간 총소방지수는 981건임
  - 첫 번째로 높은 사고유형과 두 번째로 높은 사고유형의 소방지수 간에는 약 4배의 격차가 있음
    - 두 번째로 높은 `질병외(EXCL_DISEASE_OCRN_CNT)` 총소방지수는 239건임

- **연간 총소방지수가 0건인 사고유형이 존재함**
  - `농기계사고(AGRCMCHN_ACDNT_OCRN_CNT)`
  - `날씨사고(WETHR_ACDNT_OCRN_CNT)`
  - `성폭행(SXAL_ASALT_OCRN_CNT)`

</details>

<details><summary><h3>사고유형별 건당 총유동인구수 비교</h3></summary>
  
![3_사고유형별 건당 유동인구수 비교](https://user-images.githubusercontent.com/116495744/206866096-b2cf2a9b-1552-49f1-a690-bc71feabc148.png)

- **사고유형별 연간 총소방지수와 건당 평균유동인구수 간 양의 상관관계가 존재한다고 볼 수 없음**

  - 연간 총소방지수 하위권 사고유형이 평균유동인구수도 반드시 하위권으로 나타나지는 않음
    - 연간 총소방지수가 1건인 `관통상(PNTRINJ_OCRN_CNT)`의 평균유동인구수가 가장 높음
  - 연간 총소방지수 상위권 사고유형이 평균유동인구수도 반드시 상위권으로 나타나지는 않음
    - 반면, 연간 총소방지수가 가장 높은 `낙상(HRFAF_OCRN_CNT)`의 평균유동인구수는 중위권임

</details>

<details><summary><h3>사고유형당 기간별 소방지수 비교</h3></summary>

- **계절별**

  ![4_사고유형당 기간별 소방지수 비교](https://user-images.githubusercontent.com/116495744/206866127-677e2b81-7b1e-49db-a621-1901dcc0ec8a.png)  

- **월별**
  
  ![4-1](https://user-images.githubusercontent.com/116495744/206866184-6727bb86-32ee-482e-b5f2-f8229af58548.png)

- **요일별**
  
  ![4-2](https://user-images.githubusercontent.com/116495744/206866200-e1217f56-7230-4f22-a00f-c0ef1aca8708.png)

- **휴일여부**  
  
  ![4-3](https://user-images.githubusercontent.com/116495744/206866201-3ab3cfc2-1430-430e-946b-e6e4362eb146.png)

- **전반적으로 봤을 때, 각 사고유형 소방지수의 기간별 양상이 `총소방지수(TOTAL_CNT)`와 유사하다고 볼 수 없음**
  - 계절별 : 가을, 여름, 봄, 겨울 순으로 소방지수가 높은 양상을 보이는 사고유형이 많지 않음
  - 월별 : 7월 소방지수가 높은 사고유형이 많지 않음
  - 요일별 : 수요일 소방지수가 가장 낮고, 수요일에서 멀어질수록 소방지수가 높아지는 양상을 보이는 사고유형이 많지 않음
  - 단, 휴일여부의 경우, 모든 사고유형에 대하여 휴일인 경우가 휴일아님인 경우보다 소방지수가 높지 않음

- **`낙상(HRFAF_OCRN_CNT)` 소방지수의 기간별 양상이 `총소방지수(TOTAL_CNT)`와 유사함**
  - 앞서 `낙상(HRFAF_OCRN_CNT)` 연간 총소방지수가 압도적으로 높다는 점을 확인했음
  - `총소방지수(TOTAL_CNT)` 집계 및 시각화 결과가 `낙상(HRFAF_OCRN_CNT)`에 편향된 결과로 나타난 것으로 보임
  - 다만, `낙상(HRFAF_OCRN_CNT)` 월별 소방지수의 경우, `총소방지수(TOTAL_CNT)`와 달리 7월이 아닌 8월이 가장 높았음
  
  
</details>

<details><summary><h3>사고유형 건당 평균 유동인구수 비교</h3></summary>

- **성별**
  
  ![5-1](https://user-images.githubusercontent.com/116495744/206866508-2b6b6595-10f6-4e9b-8034-3f944e02f9d0.png)

- **연령 및 생활패턴별**
  
  ![5_사고유형 건당 유동인구수 비교](https://user-images.githubusercontent.com/116495744/206866520-0fdf24ba-7d42-435f-9cb6-b6e352fc218d.png)

  
</details>

---

## DF_API

<details><summary><h3>산업단지정보</h3></summary>

- **가설**
  - 지역 내 산업단지 존재 여부가 해당 지역의 특정 사고유형 발생 횟수에 영향을 미칠 것임  
    - `고온체사고(HGTPOJ_ACDNT_OCRN_CNT)`
    - `관통상(PNTRINJ_OCRN_CNT)`
    - `기계사고(MCHN_ACDNT_OCRN_CNT)`
    - `둔상(BLTRM_OCRN_CNT)`
    - `전기사고(ELTRC_ACDNT_OCRN_CNT)`
    - `화학물질사고(CHMC_SBSTNC_ACDNT_OCRN_CNT)` 등

- **강원도 원주시내 지역별 산업단지 입주업체 현황에 관한 외부 API 활용**
  - 2022년 6월 15일 강원도 원주시 기업지원일자리과에서 배포한 ‘산업농공단지 입주업체 현황’ 자료를 활용함
    - 출처 : [**2020년 말 기준 강원도 원주시 산업농공단지 입주업체 현황**](https://www.wonju.go.kr/www/selectBbsNttView.do?key=2637&bbsNo=181&nttNo=360570&searchCtgry=&searchCnd=all&searchKrwd=&pageIndex=1&integrDeptCode=)

- **이진범주형 변수 `INDUSTRY` 추가**
  - 산업단지가 위치한 곳은 1, 산업단지가 위치하지 않는 곳은 0으로 표기함
  - 원 데이터 셋 `02.dataset.csv`의 변수 `DONG_NM`을 참조함

</details>

<details><summary><h3>유흥시설정보</h3></summary>

- **통념**
  - 유흥시설에서 만취고객이 발생할 가능성이 높음
  - 만취 고객은 이성적 판단이 결여되어 사고에 휘말릴 가능성이 높음

- **가설**
  - 지역 내 유흥시설 존재 여부가 특정 사고유형 발생 횟수에 영향을 미칠 것임
    - `단순주취(DRKNSTAT_OCRN_CNT)` 등
  - 유흥시설을 유흥주점과 단란주점으로 구체화함

- **강원도 원주시내 지역별 유흥주점 및 단란주점 입점 현황에 관한 외부 API 활용**
  - 2022년 10월 13일 강원도 원주시 정보통신과에서 배포한 ‘강원도 원주시 유흥주점 정보' 자료를 참조함
    - 출처 : [**강원도 원주시 유흥주점 입점 현황**](https://www.data.go.kr/data/3069208/fileData.do?recommendDataYn=Y)
  - 2022년 8월 23일 강원도 원주시 정보통신과에서 배포한 ‘강원도 원주시 단란주점 정보' 자료를 참조함
    - 출처 : [**강원도 원주시 단란주점 입점 현황**](https://www.data.go.kr/data/3069204/fileData.do?recommendDataYn=Y)

- **수치형 변수 `BAR` 추가**
  - 유흥시설의 경우 여러 지역에 두루 분포하고 있고, 특정 지역에 밀집되어 있음
  - 때문에 1개 이상 입점한 지역을 1로 처리하면 밀집 지역에 대하여 차별을 둘 수 없음
  - 반면, 지역별 입점갯수를 값으로 부여하면 밀집지역에 가중치를 두는 효과를 낼 수 있음
  - 따라서 변수를 이진범주형이 아니라 수치형으로 생성함
  - 원 데이터 셋 `02.dataset.csv`의 변수 `DONG_NM`을 참조함

</details>

<details><summary><h3>교통 관련 정보</h3></summary>

- **현상**
  - 특정 유형의 공간에 대하여 유독 소방지수가 높았음
    - 주거시설 : 집(61.7%), 집단거주시설(2.2%)
    - 교통시설 : 도로(12.5%), 도로외교통지역(6.4%)
  - 2021년 6월 1일 소방청에서 배포한 ‘2021년 119구급서비스 통계 연보’ 78쪽을 참조함
    - 참고 자료 : [**2021년 소방청 통계 연보 78쪽**](https://www.nfa.go.kr/nfa/releaseinformation/statisticalinformation/main/?boardId=bbs_0000000000000019&mode=view&cntId=34&category=&pageIdx=&searchCondition=&searchKeyword=)

- **가설**
  - 지역 내 특정 공간유형의 존재 여부가 특정 사고유형 발생 횟수에 영향을 미칠 것임
  - 주거시설과 관련하여 연상되는 사고유형이 마땅히 존재하지 않음
  - 교통시설과 관련하여 연상되는 사고유형은 아래와 같음
    - `탈것사고(VHC_ACDNT_OCRN_CNT)`
    - `동승자사고(FLPS_ACDNT_OCRN_CNT)`
    - `보행자사고(PDST_ACDNT_OCRN_CNT)`
    - `오토바이사고(MTRCYC_ACDNT_OCRN_CNT)`
    - `운전자사고(DRV_ACDNT_OCRN_CNT)`

- **역가설**
  - 위 다섯 가지 사고유형이 빈번하게 발생한 지역을 교통시설이 밀집된 곳이라고 가정할 수 있음

- **수치형 변수 `TRAFFIC` 추가**
  - 유흥시설정보의 경우와 동일한 이유로 수치형으로 생성함
  - 원 데이터 셋 `02.dataset.csv`에서 교통 관련 사고유형의 소방지수에 대하여 격자를 기준으로 합계함
  - 각 자료마다 격자에 해당하는 값을 변수 `TRAFFIC`에 표기함

</details>

---

## DF_PRE

<details><summary><h3>주요설명변수 선별 전 전처리</h3></summary>

- **불필요한 변수 삭제**
  - `GRID_ID` : 주요설명변수 선별을 위한 로지스틱 회귀분석 수행 시 불필요하므로 삭제함
  - `GRID_X_AXIS`, `GRID_Y_AXIS` : 탐색적 자료 분석 작업을 마쳤으므로 삭제함
  - `DONG_NM`, `DONG_CD` : 외부 API 추가 작업을 마쳤으므로 삭제함
  - `TOTAL_CNT` : 탐색적 자료 분석 작업을 마쳤으므로 삭제함
  
- **범주형 변수 전처리**
  - Label Encoding : 모든 변수가 이미 라벨 인코딩 되어 있으므로 생략함
  - One-Hot Encoding : 라이브러리 `pandas`의 함수 `dummies` 활용
        
        SEASON : SEASON_0, SEASON_1, SEASON_2, SEASON_3
        MONTH : MONTH_1, MONTH_2, MONTH_3, ..., MONTH_12
        WEEKDAY : WEEKDAY_0, WEEKDAY_1, WEEKDAY_2, ..., WEEKDAY_6
        HOLIDAY : HOLIDAY_0, HOLIDAY_1
        INDUSTRY : INDUSTRY_0, INDUSTRY_1
        
- **수치형 변수 전처리**
  
      PEOPLE, MAN, WOMAN, CHILD, YOUTH, MIDDLE, OLDER, BAR, TRAFFIC

  - 이상치 처리 : 라이브러리 `scikit-learn`의 함수 `RobustScaler` 활용  
  - 정규화 : 라이브러리 `scikit-learn`의 함수 `Standardscaler` 활용
  - 표준화 : 라이브러리 `scikit-learn`의 함수 `MinMaxscaler` 활용

- **종속변수를 이진범주형으로 변환**
  - 자료별 소방지수 분포
    - 자료별 소방지수 값은 최소 0건, 최대 2건임
    - 0건인 자료의 수가 1, 2건인 자료보다 압도적으로 높음
  - 예측 시 중요한 것은 출동 횟수가 아니라 출동 여부임
    - `💁‍♂️ 프로젝트 소개`의 `문제의 이해` 참고
  - 따라서 종속변수를 이진범주형으로 변환함
    - 소방지수가 1건 이상인 경우 그 값을 ‘참’으로 변경함
    - 소방지수가 0건인 경우 그 값을 ‘거짓’으로 변경함
  
- **데이터 셋 분할**
  - 설명변수와 1개 사고유형의 소방지수로 구성된 데이터프레임 31개 생성
    - `acc0`, `acc1`, `acc2`, ..., `acc30`
    - 각 사고유형의 순번에 따라 해당 사고유형을 종속변수로 하는 데이터프레임의 숫자를 매김
      - `🔎 DF_OG`의 `수치형 변수` 참고

</details>

<details><summary><h3>로지스틱 회귀분석을 통한 주요설명변수 채택</h3></summary>

- **각 데이터프레임별로 로지스틱 회귀분석 수행**
  - 라이브러리 `scikit-learn`의 알고리즘 `LogisticRegressior` 활용

- **각 데이터프레임별로 변별력 없는 설명변수를 삭제함**
  - 각 설명변수의 승산비(Odds Ratio; OR) 신뢰구간을 기준으로 함

</details>

---

## 🖥 DF_ML

<details><summary><h3>종속변수 범주 불균형 문제</h3></summary>

- **종속변수 범주 불균형**
  - 작년 대상 수상작의 경우 예측 정확도가 30%를 넘지 않음
  - 하지만 본 팀의 초기 설계 모델 예측 정확도는 99%에 근접함
  - 종속변수의 TRUE와 FALSE 비율이 약 1:100임
  - `FALSE`를 기입하면 예측 정확도가 높게 측정됨
  - 때문에 모델이 `FALSE`를 예측하도록 설계됨

- **OverSampling 기법**
  - OverSampling 기법을 통해 두 범주 간 비율을 유사한 정도로 맞춤
  - 라이브러리 `imblearn`의 모듈 `over_sampling`의 함수 `SMOTE` 적용

</details>

<details><summary><h3>성능평가지표</h3></summary>

</details>

<details><summary><h3>최적 모델 설계</h3></summary>

<div align="center">

| 알고리즘명 | 하이퍼파라미터 | 최적계수 | 정밀도(%) |
|---|---|---|---|
| **의사결정나무** | criterion | entropy | 7.86% |
| | max_depth | None | |
| | min_samples_leaf | 4 | |
| **KNN**| | |
| **랜덤 포레스트** | max_depth | None | 10.96% |
| | max_features | auto | |
| | n_estimators | 1500 | |
| **GBM** | | |
| **Light GBM** | learning_rate | 0.3 (임의 설정) | 15.0% |
| | max_depth | -1 | |
| | n_estimators | 100 | |
| | num_leaves | 128 | |

</div>
  
</details>
  
---

## 🎇 DF_PREDICT


---


## 📚 프로젝트를 수행하면서 학습한 내용

<details><summary><h3>Odds Ratio</h3></summary>

</details>

<details><summary><h3>Smote</h3></summary>

</details>

---

## 👭 WORKMATE

👨 [**IT`S ME**](https://github.com/jayarnim)

    - Exploratory Data Analysis
    - Predict
    - README

👩 [**김효정**](https://github.com/410am)

    - Preprocessing
    - API
    - Predict

👨 [**인찬휘**](https://github.com/wassaa-1)

    - Model Design
    - Predict

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
