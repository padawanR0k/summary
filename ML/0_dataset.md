## 머신러닝이란?
- 규칙을 일일이 프로그래밍하지 않아도 자동으로 데이터에서 규칙을 학습하는 알고리즘을 연구하는 분야
	- 알고리즘으로 특정 문제에 대한 부분을 데이터로 학습시켜 문제를 해결하게끔 하는	모델을 스스로 만들어 나가게 함
- 비교적 최근 오픈소스로 공개된 대표적인 머신러닝 라이브러리
	- 사이킷런
	- 텐서플로우
	- 파이토치

## 딥러닝이란?
- 머신러닝의 하위개념
- 머신러닝 알고리즘 중 인공신경망을 기반으로 한 방법들을 통칭함

### 학습
![](https://blog.kakaocdn.net/dn/bbH3VI/btqE0ySxzHu/JdcXZeK12XSEHAKDoomkOk/img.png)
- 지도 학습 (supervised)
	- 이미 답이 정해져 있는 문제
	- 용어
		- training data
			- input (y = **x***a + b)
				- 알고리즘 들어갈 데이터
			- target (**y** = x*a + b)
				- 알고리즘 들어갔을 때, 나와야하는 정답
		- feature
			- input으로 사용된 데이터의 종류들
				- ex) 크기, 색상, 위치 등

- 비지도 학습 (unsupervised)
	- 답이 미리 정해져있지 않은 문제. 새로운 것을 만들어내거나 예측하는데 사용된다.
		- ex) GAN으로 가상의 사람얼굴을 만들어내는 것
	- 모델이 스스로 학습하며 비슷한 것 끼리 군집시킨다.


- 강화학습 (reinforcement)
	- 보상과 벌이라는 보상을 주어, 최대한 벌을 덜받게끔 유도하는 학습방식

### 지도 학습 훈련과정

#### 1. 데이터 준비
- 도미와 빙어의 크기와 무게에 대한 데이터
#### 2. 데이터를 훈련용, 테스트용으로 나눔
- 만약 모든 데이터를 훈련용으로하면 테스트할 데이터가 없기 때문이다. (무조건 정답을 맞추게된다.)
	-	보편적으로 train:test=7:3으로 나눈다.
- 데이터 분리시, **샘플링 편향**이 생기지 않게 데이터를 적절히 섞어야한다.
	- 테스트용 데이터에 빙어의 데이터만 있으면 안됨. -> 테스트시 차이를 구별하지 못함

### code
```python
# 도미와 빙어의 길이
fish_length = [25.4, 26.3, 26.5, 29.0, 29.0, 29.7, 29.7, 30.0, 30.0, 30.7, 31.0, 31.0,
                31.5, 32.0, 32.0, 32.0, 33.0, 33.0, 33.5, 33.5, 34.0, 34.0, 34.5, 35.0,
                35.0, 35.0, 35.0, 36.0, 36.0, 37.0, 38.5, 38.5, 39.5, 41.0, 41.0, 9.8,
                10.5, 10.6, 11.0, 11.2, 11.3, 11.8, 11.8, 12.0, 12.2, 12.4, 13.0, 14.3, 15.0]
# 도미와 빙어의 무게
fish_weight = [242.0, 290.0, 340.0, 363.0, 430.0, 450.0, 500.0, 390.0, 450.0, 500.0, 475.0, 500.0,
                500.0, 340.0, 600.0, 600.0, 700.0, 700.0, 610.0, 650.0, 575.0, 685.0, 620.0, 680.0,
                700.0, 725.0, 720.0, 714.0, 850.0, 1000.0, 920.0, 955.0, 925.0, 975.0, 950.0, 6.7,
                7.5, 7.0, 9.7, 9.8, 8.7, 10.0, 9.9, 9.8, 12.2, 13.4, 12.2, 19.7, 19.9]

fish_data = [[l,w] for l, w in zip(length, weight)]
fish_target = [1]*len(bream_length) + [0] * len(smelt_length)
kn2 =  KNeighborsClassifier()

import numpy as np

input_arr = np.array(fish_data)
target_arr = np.array(fish_target)
print(input_arr, fish_data) # numpy배열은 보기 쉽게 행을 구분해주고, 몇차원 배열인지도 알려준다.
print(input_arr.shape)

#  데이터를 섞기위해 인덱스용 배열을 만든 후 섞는다.
np.random.seed(20)
index = np.arange(49)
np.random.shuffle(index)

print(len(fish_length))
train_input = input_arr[index[:35]]
train_target = target_arr[index[:35]]
test_input = input_arr[index[35:]]
test_target = target_arr[index[35:]]

# 데이터가 잘 섞여있는지 시각화
plt.scatter(train_input[:,0], train_input[:,1])
plt.scatter(test_input[:,0], test_input[:,1])
plt.xlabel('length')
plt.ylabel('weigth')
plt.show()


kn2 = kn2.fit(train_input, train_target)

print(kn2.score(test_input, test_target)) # 1.0   100점!
```



[notebook](https://colab.research.google.com/drive/1Q1d2LLUuvJxxiusdAAWv3XQFtHf8TIN6?hl=ko#scrollTo=9IQabbLFZ3cZ)