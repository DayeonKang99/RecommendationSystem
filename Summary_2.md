# Recommendation System 2
## Evaluating Predictions
Utility matrix의 일부를 test data로 구분하고 test data를 제외하고 예측 <br><br>
**Measures**<br>
- RMSE (Root-mean-square error)
- Rank Correlation: **spearman correlation** btw true ranking and predicted ranking (실제 값은 달라도 랭킹이 유사하게 정렬되어있는 경우 spearman correlation이 높음)
<br><br>

**Caution in Evaluation**<br>
- Focusing only on accuracy is not always good. diversity, 사용자가 다양한 것을 접하는 것이 좋음
- Focusing only on overall RMSE might not be always good 
  - In many applications, we care only **high ratings**; thus, a model with bad overall performance, but high performance for high rating would be preferred 
<br><br>

## Weight Learning in CF
CF with Baseline Predictor에서 cosine이나 pearson correlation coefficient로 구하는 유사도가 가장 유사도라고 볼 수 없다<br><br>
![스크린샷 2021-07-13 오후 8 55 54](https://user-images.githubusercontent.com/67621291/125447495-018c832c-ceac-4c93-aef9-ffb8f312a9bd.png)<br>
weight는 자동으로 학습해서 결정됨 <br><br>
How to learn weight?<br>
![스크린샷 2021-07-13 오후 8 58 44](https://user-images.githubusercontent.com/67621291/125447847-26825407-fa71-4a90-9e71-abb693461515.png)<br>
SSE를 최소화하는 Wij를 찾는 문제 → Optimization Problem <br><br>
Gradient descent to minimize a function *f()*<br><br>
**Weight Learning via Gradient Descent**<br>
Wij를 random value로 초기화<br>전체 식을 W로 미분 후 gradient vector의 반대 방향으로 W를 이동 <br><br>

## Latent Factor Model (LF)
![스크린샷 2021-07-13 오후 9 19 48](https://user-images.githubusercontent.com/67621291/125450376-4e9bfa06-3efe-4a7d-911d-07880bdf347f.png)<br>
item vector, user vector를 모두 길이가 3인 latent space로 mapping <br>Q는 user를 잘 나타내는 길이가 3dls row dimensional vector <br>P.T는 col을 item을 잘 표현하는 길이가 3인 vector<br>
➡️ user와 item vector의 내적을 통해 두 item과 user가 얼마나 유사한지, 얼마나 좋아할지를 예측하는 모델<br><br>

### SVD (Singular Value Decomposition)
![스크린샷 2021-07-13 오후 9 28 04](https://user-images.githubusercontent.com/67621291/125451532-1284099a-a223-44d8-9cae-760f678e7df9.png)<br>
SVD는 A와 가장 유사한, 또는 재구성 에러를 최소화하는 U, 시그마, V.T를 찾는다<br>
⚠️ 그러나 SVD는 입력 행렬이 모두 차 있어야 한다<br><br>
Instead, we solve an **optimization problem**<br>
![스크린샷 2021-07-13 오후 9 35 12](https://user-images.githubusercontent.com/67621291/125452448-9ae13c9c-219e-490d-96d0-3d5b4066e839.png)<br>
이 때 rating은 실제 있는 값에 대해서만 계산<br> 최적화 문제를 푸는 방법 → Gradient descent 방법을 사용해서 최적의 q와 p를 찾는 과정을 반복
<br><br>

## Extension for LF 
Two extensions for LF
- Regularization
  - Decrease test error
- Modeling biases
  - Seperate bias and interaction terms 

### Regularization
We use training data to find q and p using gradient descent. Then, evaluate the model with test data. <br>
What should be the number r of factors? (# of factor is model parameter)
- **Too small r** would limit the model complexity too much (= **underfitting**). It means a high training error
- **Too large r** would make the model too complicated (= **overfitting**). It means it will not generalize well for unseen test data, although it fits very well on training data

➡️ Regularization (아래는 regularization term을 붙인 식)
![스크린샷 2021-07-13 오후 9 46 37](https://user-images.githubusercontent.com/67621291/125453974-8e005b54-70e0-4b02-a006-fd52e4437f7e.png)<br><br>

### Modeling Biases and Interactions
bias와 interaction을 분리해서 모델링<br>
![스크린샷 2021-07-13 오후 9 50 30](https://user-images.githubusercontent.com/67621291/125454526-ad879cdb-c2ed-4bef-94c4-4530a59dfe31.png)<br>
사용자가 얼마나 평균보다 평점을 많이 주는지, item이 평균보다 평점을 많이 받는지에 대한 baseline predictor를 정할 수 있다. <br>따라서 bias와  interation term을 더한 것을 rxi로 모델링<br>
이렇게 하면 bias와 차이 나는 부분만 모델링하기 때문에 모델을 더 잘 학습 가능<br><br>

### Final Optimization Problem 
![스크린샷 2021-07-13 오후 9 57 28](https://user-images.githubusercontent.com/67621291/125455427-662f54fd-f034-43c1-89d0-2df01b02207d.png)
