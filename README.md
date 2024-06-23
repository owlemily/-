# 가계대출 연체율 예측을 통한 시도별 맞춤 정책 제안 readme 파일

## 1) 프로젝트 개요

이 프로젝트는 빅데이터 분석을 활용하여 지역별 연체율을 예측하는 것을 목표로 합니다. 또한  지역별 연체율과 지역 특성을 조사하여 연체율에 영향을 미치는 요인들을 식별하고, 이를 기반으로 머신러닝 모델을 구축합니다.

 데이터 불러오기 -> 데이터 전처리 -> 변수 선택 및 추출 -> 머신러닝 모델 구축 및 평가 ->결과 도출

이 프로세스를 각 지역별로 적용하여 연체율을 예상해봅니다.

## 2) 설치 및 환경 설정

 1.이 코드는 코랩환경에서 작동합니다.

 2.필요한 패키지는 numpy, pandas 등이 있습니다. 자세한 패키지 및 라이브러리는 코드에 적어놓았으니 참고해주세요.

## 3) 사용 방법

3-1) 데이터 파일을 통합문서.xlsx로 준비합니다. 데이터는 다음과 같은 형식으로 구성되어야 합니다:

총 16개의 지역이 있어야 합니다. 지역은 서울특별시 인천광역시 광주광역시 부산광역시 울산광역시 대구광역시 대전광역시 강원도 경기도 경상남도 경상북도 전라남도 전라북도 충청북도 충청남도 제주도가 있습니다.
각 지역별로 갖고 있어야 할 변수는 다음과 같습니다. 

지역명 날짜 실업률 고용률 어음부도율 지가변동률 상장회사수 비은행예금취급기관_금액 주택거래량 예금은행 기타 가계 대출금 월세수급동향 매매수급동향 전세수급동향 예금은행 주택담보 대출금 아파트매매가격지수 소비자물가지수 이혼율 교육비지출전망지수 부동산지수 부동산시장소비심리지수 가계대출연체율 통화량 기준금리 

종속변수인 가계대출 연체율은 총 42개월치의 데이터가(2019.12~2023.5), 나머지 독립 변수는 총 54개월치의 데이터가(2018.12 ~ 2023.5) 각 지역별로 있어야 합니다. 

3-2) 코랩에서 가계대출연체율_분석.ipynb를 열고 순차적으로 셀을 실행해봅니다. 코드에 관한 부분은 아래에 자세히 적어놨습니다.

3-3) 예측 결과는 화면에 출력되며, 필요한 경우 결과를 파일로 저장할 수 있습니다.

## 4) 데이터 수집

통계청, 한국 은행 등 공공 기관으로부터 지역별 연체율 및 독립변수에 대한 데이터를 수집하였습니다. 가계대출 연체율은 2019년 12월부터 2023년 5월까지 수집하였고 연체율을 제외한 나머지 데이터는 2018년 12월부터 2023년 5월까지의 기간을 포함합니다. 자료 수집의 근거는 연구 논문 및 신문 기사입니다.

## 5) 데이터 전처리

데이터 전처리 단계에서는 결측치를 처리하고, 이상치를 제거하였습니다. 또한 변수들을 표준화하여 데이터의 일관성을 확보했습니다. 마지막으로 데이터에 시차를 적용해봤습니다.

예를 들어 12개월의 시차를 둔다면 2018 ~ 2022년의 독립변수 데이터와 2019 ~ 2023년의 연체율 데이터가 매칭이 될 수 있게끔 독립변수 데이터를 아래로 내렸습니다. (파이썬의 shift 함수를 통해) 

 동일한 독립변수라도 시차에 따라 가계대출 연체율에 다르게 영향을 미칠 수 있기 때문입니다.

회귀분석을 통하여 3,6,12개월의 시차를 둔 독립변수들과 연체율 간의 관계를 모델링하고 그 중 가장 높은 결정계수 (r-square)을 보여준 시차를 선택하여 독립변수들에게 적용하였습니다. 

## 6) 변수 선택 및 추출

랜덤포레스트 회귀 모델을 생성하여 변수 중요도를 평가하였고 연체율에 가장 큰 영향을 미치는 상위 7개의 독립변수들을 선택하였습니다.

## 7) 머신러닝 모델 구축

회귀분석을 통해 선택한 독립변수들 간의 다중공선성을 확인하고 다중공선성 문제를 일으키는 변수를 제거합니다. (vif>10 이상인 독립 변수를 제거)  남은 변수들을 기반으로 선형회귀모델, 릿지선형회귀모델, 랜덤포레스트 회귀 모델, xgboost 회귀모델, lstm 모델을 구축하였습니다. 랜덤 포레스트 회귀모델, xgboost 회귀모델의 경우 gridsearchCV를, lstm 모델의 경우 parameterSearch를 사용해 최적의 파라미터를 찾는 '하이퍼파라미터튜닝 기법'을 적용하여 모델 예측 성능을 향상시켰습니다.

## 8) 모델 평가

평가 지표로는 평균 제곱근 오차(MSE), 결정계수( R-squared) 등을 사용하였습니다. 모델의 예측 성능을 평가하고, 다양한 모델 간의 비교를 수행하였습니다.

## 9) 결과 해석

모델 결과를 해석하여 경제적 요소와 지역 특성이 연체율에 미치는 영향을 파악하였습니다. 이를 통해 정책 제안에 도움을 줄 수 있습니다.

## 10) Anova 검증

세 개 이상의, 여러 그룹 간의 평균 차이를 비교하는 데 사용되는 방법입니다. 동일한 시차를 적용한 지역 세 개를 군집화 시키고 군집화된 세 부류의 종속변수(연체율)의 차이가 유의미한 것을 확인합니다. 또한 군집화된 세 부류의 독립변수의 차이가 유의미한 것을 확인합니다.

## 11) 저작권 및 저작자 정보

이 코드는 상명대학교_2023_데이터청년캠퍼스 1분반 3조에 의해 개발되었습니다. 저작권은 상명대학교_2023_데이터청년캠퍼스 1분반 3조에게 있습니다.
