# Recommendation System 1
Offline recommendation: 유명한 것 위주로 추천<br>
Online recommendation: 개인화 되어 있음<br><br><br>

## Overview
### Formal Model
X = set of customers<br>
S = set of items<br>
Utility function: u(x, s) → R 
> 고객이 아이템에 대해 어떤 평점을 매길지 예측<br>
> R is a set of ratings e.g., 0-5 stars, real number in [0,1]
<br>

### Steps in Recommendation
1. Gather known ratings for matrix<br>
rating을 매기는 방법 두 가지 → Explicit ratings, Implicit ratings(사용자의 행동을 기반으로)
2. Predict unknown ratings from the known ratings<br>
기법: Content-based, Collaborative filtering, Latent factor
3. Evaluate predictions
<br>

### Challenges in Recommendation
- Cold-start problem: 새로 가입한 사용자에게 무엇을 추천해야하는가, 새로 나온 아이템은 어떻게 추천해줄 것인가
- Data sparsity problem: 많은 아이템 중 사용자가 실제로 평점을 매긴 상품은 몇 개 되지 않아 matrix가 sparse해짐 
<br><br><br>

## Content-based Recommendations
Main idea: recommend items to user x similar to previous items rated highly by x<br>
Movie recommendations: recommend movies with same genre, actors, directors<br>
News recommendations: recommend news with similar contents<br><br>

**Plan of Action**<br>
사용자가 좋아하는 item의 profile을 기반으로 User profile(사용자가 어떤 아이템을 좋아하는지에 대한 것) 생성 <br>
User profile과 일치하는 정보를 찾아 사용자에게 추천 <br><br>
**Item Profiles**<br>
- For each item, create an item profile
- Profile is a set (vector) of features
- How to select important in text? → TF-IDF (Term Frequency and Inverse Document Frequency) 기법

### TF-IDF for Text
![스크린샷 2021-07-13 오후 5 21 24](https://user-images.githubusercontent.com/67621291/125417508-8cca3af8-54f6-46ef-bcf0-fa604fa0df28.png)<br>
IDF 크면 소수의 문서에 단어가 나온다는 의미<br>
IDF 작으면 다수의 문서에 단어가 나온다는 의미 (e.g., a, the, is)<br><br>

### User profile and Prediction
**User profile**<br>
: 사용자가 평점을 매긴 item profile의 가중 평균 (weighted average) <br>
Option) weight by difference from average rating for them (평균에서 얼마나 + / - 인지)<br><br>
**Prediction**<br>
: 유사도 계산. Use cosine similarity of user profile and item profile <br>
given user profile x and item profile i, predict<br>
![스크린샷 2021-07-13 오후 5 50 13](https://user-images.githubusercontent.com/67621291/125421891-84cc66fc-3588-45b4-9857-e0ff75e141a4.png)<br>
x, i1이 비슷한 방향을 가리킬수록(둘 사이 각도가 작을수록) 유사도 높음<br><br>
**Advatanges**<br>
- Unlike collaborative filtering, other users' data are not needed
  - No sparsity problems
- Recommend to users with unique tastes
- Recommend new items
- Provide explanations
  - By presenting content features that contributed highly to the scores 
<br>

**Disadvantages**<br>
- It is not always possible to find appropriate features (Images, videos, musics...)
- It is not clear how to build a user profile for new users just joined
- Overfitting
  - Does not recommend items outside user's content profile
  - Serendipity: people want to be suprised sometimes
  - Cannot use other people's suggestions
<br><br>

## Collaborative Filtering (CF)
Do not use the contents informantion, but use rating information for recommending items to users<br>
2 approaches
- User-user CF
- Item-item CF
<br>

### User-User CF
Consider user x <br>
Find other users whose ratings are "similar" to x's ratings<br>
Predict x's ratings based on ratings of the similar users<br><br>
**어떻게 사용자간의 유사도를 측정할 수 있을까?**<br>
→ Jaccard similarity: 어떤 상품에 평점을 매겼다, 안매겼다만 표시 (Problem: ignores the rating values)<br>
→ Cosine similarity (Problem: low rating contributes to positive similarity)<br>
→ Pearson correlation coefficient: 평균 벡터를 뺀 값을 사용 (Cosine similarity의 solution)<br>
![스크린샷 2021-07-13 오후 6 04 34](https://user-images.githubusercontent.com/67621291/125424190-d47aed1c-033b-4299-89a5-097015206478.png)<br><br>
**Predicting Ratings**<br>
Let rx be the vector of user x's rating<br>Let N be the set of k users most similar to x who have rated item i<br>
![스크린샷 2021-07-13 오후 6 09 00](https://user-images.githubusercontent.com/67621291/125424861-3fb39d62-d007-4dd9-8bd2-506c56b12940.png)<br>
위 식의 첫 번째는 단순하게 평균을 내는 것, 두 번째는 가중 평균<br><br>

### Item-Item CF
Goal: to predict rating of item i by user x<br>
Main idea:
- using the utility matrix, find top k most similar items to i rated by user x
- predict rating for item i using the weighted average of ratings to the similar items
- use same similarity metrics as in user-user model <br>

![스크린샷 2021-07-13 오후 6 15 01](https://user-images.githubusercontent.com/67621291/125425765-6488f593-3d42-49d9-8bee-2e68dff79635.png)<br><br>
**Example**<br>
![스크린샷 2021-07-13 오후 6 17 55](https://user-images.githubusercontent.com/67621291/125426239-028b236e-8a9c-495c-a200-b8bec1bbae0e.png)<br><br><br>

CF를 개선하는 방법<br>
### Ading Baseline Predictor to CF<br>
![스크린샷 2021-07-13 오후 6 23 10](https://user-images.githubusercontent.com/67621291/125427151-6c0d2d02-d2f5-4c75-ac74-210a4808e5d5.png)<br><br>

**Advantages**<br>
- Works for any item; no feature selection needed
- Good results in many cases
<br>

**Disadvantages**<br>
- Cold start
  - Needs a large number of users in the system to find a match
  - Cannot recommend a new item that has not been rated
- Sparsity: the utility matrix is typically sparse; maybe hard to find users that have rated the same rate (데이터가 없으면 학습이 잘 안된다는 문제)
- Popularity bias: tendency to recommend popular items; insufficient performance to users with unique tastes (성향이 minor한 경우 추천을 잘 못해줌)
<br><br>

## Hybrid Methods
Make an ensemble predictor out of many models
- Average score (여러 모델의 예측값의 평균)
- Majority vote (많은 모델이 동의하는 값)
<br>

Add content-based method to CF
- New item problem: in item-item CF, rating 없이 어떻게 새로운 아이템과 유사한 아이템을 찾을 수 있는가
  - Add item profiles
- New user problem: in user-user CF, rating 없이 어떻게 새로운 user와 유사한 user를 찾을 수 있는가
  - Add demographic information (same age, location, job...)
