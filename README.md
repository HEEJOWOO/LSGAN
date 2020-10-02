# LSGAN : https://arxiv.org/pdf/1611.04076v2.pdf

#### <I studied while referring to various sites, but it is not enough. Thanks anytime for feedback.>
### <heejo5@naver.com>

LeastSquares GAN
----------------
![image](https://user-images.githubusercontent.com/61686244/94903352-a0a9ea00-04d4-11eb-8775-5f9651adeee9.png)

* 기존 GAN에서는 loss function 으로 sigmoid cross entropy loss를 사용하였고 이로 인해 Gradient Vanishing, Mode Collapse, 생성된 이미지의 결과가 매우 저화질인 문제들이 있었고 학습이 제대로 되지 않았음
* 사용된 손실 함수 되신 제안한 least square를 손실 함수에 적용 시켜 이 문제를 해결 
* Least square는 가짜 샘플들을 결정 경계에 가깝게 이동
* least square loss function은 알맞게 분류되었지만 결정 경계에서 멀리 떨어진 샘플들에 대해 패널티를 부과하여 가짜 샘플들에 대해 샘플들이 제대로 분류되었어도 결정 경계쪽으로 이동시켜 Real Data와 비슷한 생성 이미지를 만듦
* 그림 (a)는 두 손실 함수의 결정 경계이고 결정 경계가 실제 데이터 분포를 가로질러야 성공적으로 GAN이 학습했다고 볼 수 있음LSㅣㅏ

LSGAN 목적
----------
![image](https://user-images.githubusercontent.com/61686244/94903400-b7504100-04d4-11eb-9eac-fdf0d7d5e2a7.png)
* 그림 (b)는 sigmoid cross entropy loss function으로 G를 업데이트 하기 위한 가짜 샘플들이 올바른 경계안에 있기 때문에 에러가 적음
* 하지만 여기서 문제는 sigmoid cross entropy loss 결정 경계 선을 보게되면 아래는 진짜 데이터 위는 가짜데이터라고 했을때 별표로 표시된 가짜 데이터 샘플이 이미 진짜로 클래스가 구별 되어있는 것을 볼 수 있음
* 이 별표로 표시된 가짜데이터가 진짜 샘플들이 모여있는 분포에서 멀리 떨어져 있는것을 확인 할 수 있음
* 이 경우 generator 입장에서는 이미 discriminator를 매우 잘 속이고 있기 때문에 딱히 더 학습 할 의지가 없는 vanishing gradient 문제가 발생 
* 이 부분을 해결하고자 별표 샘플들을 진짜 데이터 방향으로 끌고 와보자는게 이 논문의 핵심 아이디어
* Discriminator에 sigmoid cross entropy loss 대신 least square loss를 사용해서 결정 경계에서 멀리있는 샘플들에게 패널티를 줘 실제 결정 경계와 가깝게 업데이트를 하는 것

LSGNA 목적함수
-------------
![image](https://user-images.githubusercontent.com/61686244/94903568-f8485580-04d4-11eb-8ab7-f36a1609b325.png)
![image](https://user-images.githubusercontent.com/61686244/94903752-3776a680-04d5-11eb-8771-97ddc8dad353.png)
![image](https://user-images.githubusercontent.com/61686244/94903789-43faff00-04d5-11eb-9bf0-9b9adaef088c.png)
![image](https://user-images.githubusercontent.com/61686244/94903807-4c533a00-04d5-11eb-90f0-a4b55215c47e.png)


* 식 1은 기존 GAN에서 사용하던 minmax 목적함수이고 아래의 식 2 논문에서 제안한 LSGAN의 목적 함수 
* LSGAN의 목적함수에 나와있는 a와 b 와 c는 각각 가짜데이터, 진짜 데이터, G입장에서 D가 믿도록 하고 싶은 값을 나타냄
* f-divergence의 관계를 나타내기 위해서는 기존 GAN에서 정리했던 JSD(Jensen-Shannon Divergence)를 사용
* LSGAN의 목적함수에서  G에 대한 항이 아니라서 minVLSGAN(G) 값에 영향이 없는 확장식을 만들 수 있음

LSGAN Architecture
------------------
![image](https://user-images.githubusercontent.com/61686244/94903857-5ffea080-04d5-11eb-9a2a-489947889aa6.png)
![image](https://user-images.githubusercontent.com/61686244/94903900-74db3400-04d5-11eb-80f1-82f547d523c9.png)

* model은 VGG 모델로부터 영감을 받아 사용하였지만 D에서 마지막 단에 Least squares loss function을 추가하였다. 또 DCGAN에 따라 G에는 ReLU를 D에는 LeakyReLU를 activation function으로 사용

LSGAN VS DCGAN
--------------
![image](https://user-images.githubusercontent.com/61686244/94903957-88869a80-04d5-11eb-8c14-d3c7cba7138a.png)

안전성에 대한 실험
-----------------
![image](https://user-images.githubusercontent.com/61686244/94904010-9dfbc480-04d5-11eb-8e0b-efd7c1a8a35d.png)
![image](https://user-images.githubusercontent.com/61686244/94904051-ad7b0d80-04d5-11eb-83d2-6126ec77bc4f.png)

Conclusions
-----------
* Least Square loss function 을 사용하여 G가 조금 더 결정 경계에 가까운 샘플을 만들 수 있음
* Least Square loss function 을 사용하면 오직 한 점에서만 최소값을 가져 안정적인 학습이 가능
* 더 실제와 같은 이미지를 생성하고 LSGAN으로 생성된 문자는 읽을 수 있음
* Label Vectors를 통해 정확한 label의 이미지를 생성할 수 있고 data augumentation에도 사용될 수 있음


