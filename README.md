# 한국어 단발성 대화 데이터셋에 대한 다분류 감정분석
SWCON32101 데이터분석 캡스톤디자인   
한 개의 문장과 그 문장에 대해 [놀람, 분노, 슬픔, 행복, 혐오, 중립, 공포] 총 7개 종류로 레이블링된 데이터셋으로 다분류 분석을 실시한다. 여러 가지 모델을 사용해서 각 모델을 평가함으로써 가장 좋은 모델을 선택하고, 그 모델로 실제 주위에서 자주 쓰이는 감정이 담긴 문장을 분류해본다.

## 데이터셋 소개
본 데이터셋은 AI HUB의 KETI 지능정보 플래그십 R&D 데이터임을 밝힌다.     
**한국어 감정 정보가 포함된 단발성 대화 데이터셋** https://aihub.or.kr/opendata/keti-data/recognition-laguage/KETI-02-009   
데이터는 밑 이미지와 같이 한 문장과 그에 맞는 레이블로 이루어져 있고, 각 레이블당 4800 ~ 6000 개의 데이터셋이 존재한다. 합해서 총 38594개의 데이터가 있다.     
![Alt text](https://aihub.or.kr/sites/default/files/2019-12/%ED%95%9C%EA%B5%AD%EC%96%B4%20%EA%B0%90%EC%A0%95%20%EC%A0%95%EB%B3%B4%EA%B0%80%20%ED%8F%AC%ED%95%A8%EB%90%9C%20%EB%8B%A8%EB%B0%9C%EC%84%B1%20%EB%8C%80%ED%99%94%20%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%85%8B.png "예시 데이터")

## NB&SVM.ipynb
가장 기본적인 방법으로 감성 분류 진행. 
* **KoNLPy**의 **Okt**를 이용하여 텍스트 토큰화. 
* 불용어 제거, 이모티콘 정규화
* **BoW** & **Tf-idf** 사용
* **Naive Bayes, Support Vector Machine** 등 여러 가지 모델 사용
### 결과
* Naive Bayes: 0.4862
* SVM: 0.4936
* Logistic Regression: 0.4911   
> SVM이 정확도 49.36%로 가장 성능이 좋았다.    
자연어 처리에서 잘 쓰이지 않는 logistic regression도 비슷하게 좋은 성능을 보였다. 

## LSTM.ipynb
앞에서 전처리를 한 데이터를 가지고 더 복잡한 분류방법 사용. 
* 워드 임베딩: 케라스의 기본 **Embedding()** 함수, **Word2Vec, FastText**
* **LSTM, BiLSTM** 사용
### 결과
* LSTM + Embedding() = 0.4881
* BiLSTM + Embedding() = 0.4815
* LSTM + Word2Vec(자체훈련) = 0.3236
* LSTM + FastText(훈련된 모델) = 0.4672      
> 워드 임베딩을 쓰지 않았을 때에 비해 결과가 비슷하거나 더 안좋았음.    
전처리 방법의 개선, 임베딩 방법의 개선이 필요해보임.    
