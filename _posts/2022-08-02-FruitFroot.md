---
title:  "[DeepLearning] 과수화상병 CV project - FruitFroot"
excerpt: "딥러닝 CV로 과수 질병 예측 모델 및 사이트 만들기"

categories:
  - DL
tags:
  - [DL, Deeplearning, CV, 과수화상병, MobileNetV2, VGG19, AI, Flask, Front-End]

toc: true
toc_sticky: true
 
date: 2022-08-02
last_modified_at: 2022-08-03
---
# FruitFroot

[개발 레포 - B-oPy github](https://github.com/B-oPy/FruitFroot)

> 서비스 아이콘 이미지

![icon](https://user-images.githubusercontent.com/104330432/182096938-c120bb18-b13a-4505-9927-c7ef29bef564.png)

### 개요
> 이어드림스쿨 2기 CV 미니프로젝트 과정에서 자유주제 과수화상병 이미지분류 프로젝트를 진행하였습니다. AIhub에서 과수화상병에 대한 데이터 셋을 구하여 딥러닝 모델로 CV image classification 질병 예측 모델을 만들었습니다. 과일을 케어해서 `푸릇푸릇` 하게 해보자는 의미로 프로젝트 이름은 `FruitFroot`으로 정하였습니다. 

### 프로젝트 수행 절차 
![수행절차](https://user-images.githubusercontent.com/104330432/182101308-18e850fc-a356-4dac-8b92-8b9462a1e401.png)

### 데이터셋
> - [AI-hub 과수화상병 데이터셋](https://www.aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=146) 에서 제공된 사과, 배 질병 이미지 데이터를 바탕으로 이미지 분류 서비스를 구축하였습니다 
> - 딥러닝 학습에 맞게 사이즈를 (224, 224) 로 줄이고, 데이터셋에 포함된 증강데이터 대신 `tenserflow.keras.preprocessing.image` 의 `ImageDataGenerator`를 사용하여 총 사과 이미지 37024개와 배 이미지 22991개의 이미지 데이터들을 `augmentation` 하여 딥러닝 학습을 진행하였습니다.

### 데이터셋에서 분류가 가능한 사과와 배 질병 목록
![이미지설명](https://user-images.githubusercontent.com/104330432/182098359-d91702ad-ff5b-4d9c-8710-bb4670dbf04c.png)

### Modeling 
> - 처음에는 정상 배, 배검은병무늬병, 배과수화상병, 정상 사과, 사과갈색무늬병, 사과과수화상병, 사과부란병, 사과점무늬낙엽병, 사과탄저병의 9개의 클래스로 구분하여 `Multi-Class Image Classification`을 진행하였습니다. 그러나 테스트 이미지를 넣으면 사과와 배를 정확히 구분하지 못하였습니다. <br>
> - 배와 사과를 구분하여 각각의 질병에 대해서 배는 MobileNetV2, 사과는 VGG19로 모델을 구축하고 아래의 결과를 얻었습니다. 이 모델들을 저장하여 테스트 이미지를 적용 시켰더니 잘 판단하는 모습을 보였습니다.<br>

||배|사과|
|------|---|---|
||MobileNet|VGG19|
|Validation Loss|0.0595|0.0922|
|Validation Accuracy|0.9774|0.9705|

- VGG19 모델 개요
![VGG19](https://miro.medium.com/max/1400/1*_Lg1i7wv1pLpzp2F4MLrvw.png)

- MobileNetV2 모델 개요
![mobilenetv2](https://1.bp.blogspot.com/-M8UvZJWNW4E/WsKk-tbzp8I/AAAAAAAAChw/OqxBVPbDygMIQWGug4ZnHNDvuyK5FBMcQCLcBGAs/s1600/image5.png)

### Flask API
> Flask API와 html, JS, CSS 등을 활용하여 Front-End를 구축하였고, 로컬서버에서 테스트를 진행하였습니다.
<img width="1315" alt="스크린샷 2022-08-01 오후 12 56 06" src="https://user-images.githubusercontent.com/104330432/182100793-283d5f93-dedb-444e-bbe8-4d042b4f7610.png">

> <b>데스크탑 화면</b>

<img height="500" width="600" alt="스크린샷 2022-07-29 오후 5 21 12" src="https://user-images.githubusercontent.com/104330432/182103930-d1466b11-291e-4e40-839d-ceb05e59fdc8.png">
<img height="500" width="600" alt="스크린샷 2022-07-29 오후 5 22 06" src="https://user-images.githubusercontent.com/104330432/182103990-fccc674b-37b4-4523-ae9c-bdd5222c5738.png">

> <b>모바일 화면</b>

![11](https://user-images.githubusercontent.com/104330432/182106789-cccc38df-0304-4ab4-9134-20895b1e5ce1.png)

> <b>자세히 보기 화면</b>

<img height="500" width="800" alt="스크린샷 2022-07-29 오후 4 14 08" src="https://user-images.githubusercontent.com/104330432/182104433-a964eed1-cfff-455d-b0bc-facf4cd4e88a.png">

### reference

- [Flask 딥러닝 front-end](https://github.com/krishnaik06/Deployment-Deep-Learning-Model)
- [html 편집 사이트](https://nicepage.com/)