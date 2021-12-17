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

## 모델비교분석.ipynb & w2v&ft
형태소 단위 분석 뿐만 아니라 음절단위, 자소단위 등 데이터의 변화를 주었을 때 결과. 
* 형태소 분석을 할 때 불용어를 제거하지 않음. 
* 자소 단위는 성능이 너무 안좋아 제외. 
* 형태소 단위, 음절 단위로 각 모델의 성능 평가. 
* 모델 수립은 위 두개의 파일과 같음. 
* w2v과 ft도 형태소 단위 데이터로 비교분석. (음절 단위는 의미가 없었음.)
### 결과
<img width="307" alt="스크린샷 2021-12-17 오전 11 22 38" src="https://user-images.githubusercontent.com/49444498/146478514-ad238fa3-3450-40db-a4e5-81fc63a10c74.png">
<img width="998" alt="스크린샷 2021-12-17 오전 11 23 01" src="https://user-images.githubusercontent.com/49444498/146478590-a1cc5756-23c9-4e04-9276-d127ce81ca4e.png">       
<img width="476" alt="그림1" src="https://user-images.githubusercontent.com/49444498/146479817-839063d3-4672-4d9b-8512-83baa4e33b8a.png">      

> 모두 40%대의 accuracy.     
그 중 형태소 단위 데이터를 Tf-idf으로 전처리하여 SVM으로 훈련시키는 것이 가장 accuracy가 높았다.      
Word2Vec과 FastText 중에서는 FastText가 성능이 좋았지만, 다른 모델만큼 성능이 좋지는 않았다.      
클래스 중에서는 행복이 가장 잘 분류되었고, 중립, 혐오, 분노는 잘 분류되지 않았다. 
