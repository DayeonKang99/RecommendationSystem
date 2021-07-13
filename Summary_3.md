# Recommendation System 3
앞선 content 기반, collaborative filtering, latent factor model은 Utility matrix 기반 <br>
그래프 기반 추천 시스템에 대해 알아보자<br><br>

## Graph Based Recommendation
- Friend recommendation in SNS
- Web search: query suggestion
- TV program recommendation
- Suggest promising interactions or collaborations in a company with hierarchical organization
<br>

**Problem Definition**<br>
Given(입력): a snapshot of a social network (or graph)<br>
Infer: which new interactions among its menbers are likely to occur in the near future<br>
This problem is also called "Link Prediction" (연결이 생길 것 같은 두 노드, 즉 링크를 예측하는 문제)<br><br>

**Data**<br>
Co-authorship network from arXiv (여러 분야에서 어떤 저자들이 다른 저자들과 같이 논문을 썼는지 나타내는 그래프 데이터)
- Astro-ph: astrophysics 
- Cond-mat: condensed matter
- Gr-qc: general relativity and quantum cosmology 
- Hep-ph: high enery physics-phenomenology
- Hep-th: high enery physics-theory
<br>

**Evaluation**<br>
Choose four timestamp (t0 < t0' < t1 < t1')<br>
Use G[t0, t0'] as training data
> Predict future links
Use G[t1, t1'] as test data
> Evaluate the prediction
<br>

**Method for Link Prediction**<br>
Rank according to the 'similarity': score(x, y) (노드 x를 기준으로 유사도가 높은 노드들 가운데 아직 연결이 안된 노드가 가장 연결 가능성이 높다고 판단)
- Graph distance
- Methods based on node neighborhoods
- Methods based on the ensemble of all paths
- Higher-level approaches 
  - Can be combined with the approaches above 
<br>

### Graph Distance
score(x, y) = (negated) length of shortest path btw x and y (x, y 사이 path가 짧으면 score가 높으므로 앞에 - 붙임)<br><br>

### Methods based on node neighborhoods
![스크린샷 2021-07-13 오후 10 28 46](https://user-images.githubusercontent.com/67621291/125460228-f419c521-c621-4a6f-8b19-bc9bf78ff921.png)<br><br>
![스크린샷 2021-07-13 오후 10 55 21](https://user-images.githubusercontent.com/67621291/125464427-a0125d47-49f5-4bcf-bcd7-7649526953c6.png)<br>
Adamic/Adar 방법은 neighbor의 수가 많으면 common neighbor의 가중치를 낮추고 neighbor 수가 없으면 가중치를 높인다<br><br>

### Ensemble of All Paths
- Katz proximity: path가 짧은 게 많을수록 좋다 (길이가 긴 path에 작은 weight) <br>![스크린샷 2021-07-13 오후 11 00 05](https://user-images.githubusercontent.com/67621291/125465211-88dfd555-7ebf-4611-8469-c315109fdff5.png)
- Hitting time, commute time
  - Hitting time Hx,y: expected time for random walk from x to reach y (작으면 가까운 노드)
  - Commute time: Hx,y + Hy,x (갔다가 돌아오는 시간)
- Random Walk with Restart (RWR)
  - Similar to Topic-specific PageRank (RWR에서는 단 하나의 페이지를 topic page로 봄) 
  - Stationary distribution weight of y under the following random walk 
    - with probability a, jump to x
    - with probability 1 - a, go to random neighbor of current node
- SimRank: score(x, y) is defined by
  - if x=y, score(x, y)=1
  - otherwise, score(x, y) = ![스크린샷 2021-07-13 오후 11 08 44](https://user-images.githubusercontent.com/67621291/125466663-5a5f5a40-5176-4baa-9b08-988265cf0c66.png)
  - The expected value of gamma^l, where l is a random variable giving the time at which random walks started from x and y first meet 
  - x, y가 얼마나 빨리 만나는지에 비례하는 score
<br>

### Higher-Level Approaches
![스크린샷 2021-07-13 오후 11 13 22](https://user-images.githubusercontent.com/67621291/125467421-fcfc7e50-85b9-4d22-940f-108693e9ef75.png)<br>
Mk로 그래프를 재구성할 수 있어서 그 그래프로 Katx measure 또는 common neighbors를 구할 수 있다<br>
Mk(x, y)는 노이즈를 제거한 x, y의 유사도 또는 거리라고 볼 수도 있다 <br><br>
![스크린샷 2021-07-13 오후 11 16 50](https://user-images.githubusercontent.com/67621291/125467950-97aafe1b-a2eb-4bfb-94bc-c9d353dac7e0.png)<br>
cluster 간을 연결하는 edge는 유사도 관점에서 별로 중요하지 않은 edge라 이 edge를 지우고 score를 구하면 더 정확한 값을 얻을 수 있다<br><br>

### Compare the Performances
![스크린샷 2021-07-13 오후 11 22 29](https://user-images.githubusercontent.com/67621291/125468937-4883d7cc-e1d5-4afe-8143-04bcbfa2b725.png)<br><br>
![스크린샷 2021-07-13 오후 11 25 48](https://user-images.githubusercontent.com/67621291/125469478-a3b64fc3-1e0e-48ef-89a4-13aeb2109660.png)<br>
small world: graph에서 임의의 두 노드가 거리가 짧기 때문에 그래프 상의 거리를 가지고 예측하는 것이 일반적으로 잘 안될 수 있다<br>
이 이유로 graph distance predictor의 성능이 낮은 이유를 설명할 수 있다<br><br>
![스크린샷 2021-07-13 오후 11 27 38](https://user-images.githubusercontent.com/67621291/125469797-93882f19-4687-4848-afdc-c321bac241e2.png)<br>
common neighbor는 비교적 간단하고 계산도 쉬우면서도 성능이 괜찮다
