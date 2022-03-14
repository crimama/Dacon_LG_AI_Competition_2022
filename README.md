# Dacon_LG_AI_Competition_2022

#Dacon 농업 환경 변화에 따른 작물 병해 진단 AI 경진대회
https://dacon.io/competitions/official/235870/overview/description

#프로젝트 개요 
- 농작물의 이미지와 내부온도, 내부습도, 이슬점 등이 담긴 속성 데이터를 이용해 해당 작물이 어떤 병충해에 걸렸는지 분류하는 대회 
# 데이터 
- 데이터는 크게 3개로 나뉘어 있음 - 이미지, Annotation, 환경 데이터 
- 이미지는 해당 작물의 사진 
- Annotation은 해당 작물의 코드, 상태 코드, 촬영 부위, 생육 단계, 질병 피해 정도 등 작물에 관한 정보가 담겨 있음 

# 모델 설계 
- Input 데이터로 환경 데이터와 Image를 사용 함 
- 환경 데이터
  - 해당 작물의 사진이 찍히기 전 48시간 동안의 환경 데이터로 시계열 데이터 형태를 갖고 있음 
  - Stacked Bidirectional LSTM을 이용한 모델을 설계 함 
- 이미지 데이터 
  - Efficientnet b0 모델을 사용 함 
- ![image](https://user-images.githubusercontent.com/92499881/158140201-7e5aba53-f2fc-46b9-9723-b978fc21487d.png)
- **output Class** 
  - 최초 모델 설계 시 작물 코드, 병충해 종류, 병충해 정도 세개를 모두 각각 예측해 결합하는 방식으로 진행 함 
  - 하지만 각각 예측해 결합할 경우 되려 경우의 수가 더 많아져 예측 정확도가 떨어지는 결과를 초래 함 
  - 그래서 Output Class를 Train 데이터에 존재하는 경우의 수로만 Class를 제한하여 정확도를 높임 

# Test 결과 
![image](https://user-images.githubusercontent.com/92499881/158141028-7601875c-3167-47e8-98fa-e7b21187b0c4.png)
- Test 데이터를 이용해 평가한 결과 대부분의 Class는 높은 정확도를 보였지만 특정 Class 6 만 낮은 정확도를 보임 
- Submission Macro F1-score의 경우 0.91 정도의 Score가 나오는 것으로 보아 해당 Class의 정확도를 높이는 것이 성능 상승의 Key Factor로 보임 

# 최종 결과 ![image](https://user-images.githubusercontent.com/92499881/158141892-5b67186e-6d1e-4299-85d9-4132ad8cbad6.png)

# 피드백 
- Train의 F1-score가 0.96이 나온것에 반해 Submission F1-score는 0.91정도가 나옴 --> Overfitting이 됨 
  - Augmentation을 Albumentation을 이용한 Random Augmentation을 사용했을 경우 조금 더 일반화 된 모델을 만들 수 있을 것이라 생각 됨 
- 데이터의 특성을 잘 반영시키지 못함 
  - 작물의 특성 특히 Class 6번의 병해에 대한 특성을 적용 했더라면 더 좋은 성능이 나오지 않았을까 하는 아쉬움이 듬 
- 옵티마이저, Learning rate 
  - SGD, Adam, Rmsprop 정도만 사용하고 Learning rate 최적화 or 스케줄러를 사용했다면 더 좋은 결과가 있을 수 있다고 생각 됨 
- 제너레이터 
  - 제너레이터를 사용하지 않고, 데이터를 통으로 사용 했음 
  - 제너레이터가 성능에 영향을 주지는 않지만 유연한 모델 변경, 데이터 처리가 가능하기 때문에 더 많은 경우의 수를 만들어 모델에 시도할 수 있었다고 생각 됨 
