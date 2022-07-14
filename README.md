# Socar_Hackerton
결과물


## 팀원
* 염수웅(팀장)
* 강지우
* 이도영
* 이보영
* 차유경

## 프로젝트 배경 및 목적

쏘카 앱의 누적 다운로드 수가 1000만을 돌파하는 등 카셰어링 업계가 계속해서 성장함에 따라 보험료도 세분화할 필요가 있다고 생각했다. 이에 따라 여러가지 교통사고 발생 원인 중 가장 분쟁이 많은 유형인 차선 변경을 통해 주행 위험도를 감지할 수 있는 프로젝트를 진행하기로 결정했다. 이러한 위험도 수치화는 향후 안전 운전 회원에게는 쿠폰 및 보험료 할인(Benefit)을, 부주의 회원에게는 보험료 인상(Penalty)을 통해 주행 습관 연계 보험 적용에 활용될 수 있다.
 
## 차선 변경 영상에서 위험도 추출

아래 두 예시는 각각 차선 변경 시 위험도를 보여준다. 첫 번째는 위험도 0.41, 두 번째는 위험도 0.31로 측정되었다.

![compressed_safe 0 58](https://user-images.githubusercontent.com/93107210/178995797-129dda5d-6d98-484a-9177-d6b08751947d.gif)

![safe 0 68](https://user-images.githubusercontent.com/93107210/178995930-6042e342-fbae-4c4a-964b-eac8faba80d7.gif)


## 학습에서 중요한 요소
* 주변에 차가 많은가
* 주변에 보행자가 많은가
* 차선 변경 시 옆 차선에 차가 있는가
* 차선 변경 시 앞 차와의 간격이 가까운가


## 모델

# Detectron2 (Mask-RCNN)
Mask-RCNN 구조를 활용한 Detectron2를 활용하여 각 프레임의 Instance Segmentation 진행. 사람, 차, 신호등 등을 정확히 감지하는 등 우수한 object detection 성능을 보였다.

# CNN-LSTM
CNN-LSTM 모델을 통해 데이터의 공간적 특성과 시계열 특성을 학습시켰다.

## 데이터
* UC Berkeley Deep Dive Dataset
* 5명의 팀원이 주행자의 차선 변경을 안전/위험으로 Labeling & Scoring data
* 'Micosoft COCO2017 Dataset으로 사전 학습된 Mask RCNN'을 Detectron2에서 사용


---  
<br><br>
## 1차 시도(165개 영상, scoring 1~3)

### 학습된 처음 5개의 영상 test 결과

![image](https://user-images.githubusercontent.com/83010037/179005489-bd8a9b5e-1f9d-4d41-8ada-d5377b6745e8.png)

 |:--:|

    
### 1차 시도의 결론 및 피드백

1. 모델 완성 직후 진행한 첫 테스트에서는 학습에 이용된 영상 데이터 5개를 그대로 테스트에 적용하였으나 모든 영상에 대한 결과가 동일하게 나옴
2. epoch 1000으로 학습하여 모든 데이터의 점수가 비슷하게 나온 것으로 판단
3.Wandb 툴에서 최적의 epoch가 100으로 나온것을 확인 후 epoch 100으로 수정
4.Scoring을 1~3점으로만 진행하였으며 학습에 사용된 대부분의 영상들이 안전으로 판단되는 영상 이였기 때문에 나온 결과라 판단
5.Scoring을 1~5점으로 다양하게 하였으며 불필요한 데이터라 판단되는 영상은 제거 후 다시 Labeling 진행
6.학습되는 영상 데이터 165개에서 135개로 축소


  

---  
<br><br>

## 2차 시도(135개로 데이터 축소, scoring1~5)

### 학습된 처음 5개의 영상 test 결과

![image](https://user-images.githubusercontent.com/83010037/179006095-8e1ed66c-9843-40db-b0b9-88a21a47f7aa.png)


        
### 2차 시도의 결론 및 피드백

1. First Test 결과에 대한원인 분석 후 Second Test 진행
2. 마찬가지로 학습에 이용된 5개의 영상으로 테스트 진행 결과 다양한 결과 도출
3. 결과에 사용된 첫 번째 영상은 안전한 영상, 두 번째 영상은 위험한 영상으로 판단하고 테스트 진행
4. 첫 번째 영상은 더 안전하고 두 번째 영상은 완전히 위험한 영상의 결과로 나왔어야 하나 기대에 미치지 못함
5. Instance Segmentation 결과를 확인해 보니 같은 객체도 프레임 별로 색이 다르다는 것을 확인


![image](https://user-images.githubusercontent.com/83010037/179006421-9216a30f-8783-401b-b457-993c06c743a9.png)
![image](https://user-images.githubusercontent.com/83010037/179006445-56185118-f052-4e76-94a5-3152b1d89b9f.png)
![image](https://user-images.githubusercontent.com/83010037/179006461-413eab84-104f-4678-8d9d-0b32209c04f5.png)
![image](https://user-images.githubusercontent.com/83010037/179006484-9f3e3f5f-5e98-4546-a142-0c3076da90b9.png)
        
 --- 
 <br>
 
## Alpha 시도(No segmentation)

### 결과에 사용된 두 영상에 대한 테스트 결과
![image](https://user-images.githubusercontent.com/83010037/179006841-d9547307-36f9-4119-9a5e-5bc9d90ac6e5.png)
![image](https://user-images.githubusercontent.com/83010037/179006859-fd2c2597-12bc-4606-aef9-a04a1ec2e8c8.png)



### Alpha 시도 결과 및 피드백

1. Instance Segmentation을 진행하지 않은 영상에 대한 학습 결과 어느정도 기대에 미치는 결과 획득
2. 같은 객체가 프레임 별 색이 다르게 나타난 것이 학습에 영향을 미쳤다고 판단
3. 영상을 프레임으로 쪼개고 쪼갠 프레임에 대한 Instance Segmentation을 진행하였기 때문에 같은 객체도 프레임 별 색이 달랐음


---
<br>



