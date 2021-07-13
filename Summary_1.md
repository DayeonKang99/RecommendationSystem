# Recommendation System
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
<br><br>

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
