# LR-GCCF
LR-GCCF model and Amazon-Books data
1. Amazon-Books data preprocessing.ipynb에선 amazon의 책 판매부분 data를 사용하고자 하는 모델(GCN based model)에 맞게 가공합니다.
- input data는 1) user가 각 책(item)들에 대해 5개의 별점으로 rating한 log data와 2) 각 책들에 대한 정보가 담겨있는 meta-data입니다.
- output data는 1) 각 user와 책(item)이 숫자로 mapping되어, 어떤 user가 item을 rating 했는지를 나타내는 dictionary(training과 
test data로 나눠짐)와 2) meta-data에서 뽑아낸 각 item들의 ranking입니다.

2. LR-GCCF.ipynb에선 LR-GCCF(Linear Residual Graph Convolutional Collaborative Filtering) model을 다룹니다.
- 해당 모델은 GCN(Graph Convolutional Network)에 기반한 model로, Collaborative Filtering이라는 이름이 붙은 것에서 알 수 있듯이 이웃 user나 item들의
정보를 aggregate하여 embedding하는 모델입니다. Computational Cost를 낮추기 위해 GCN에서 activation function을 제거한 Simplifying GCN model에 기초하여(Linear),
ResNet에서 차용한 아이디어로 skip-connection을(Residual) 사용합니다.
- input으로는 user가 어떤 item을 골랐는지와 같은 Bipartite(양분화)된 graph 형식의 data를 사용합니다.('1.Amazon-Books data preprocessing.ipynb'에서 output으로 나온
user-item-dictionary 사용) 
- output으로는 각 user와 item이 embedding된 커다란 vector matrix가 나옵니다.
- embedding된 결과를 Collaborative Filtering에 사용하여 Recommender처럼 이용하는 방법은, 각 user vector마다 모든 item들의 vector를 내적하여
가장 결과값이 큰 item들을 차례대로 추천해주면 됩니다.

3. Ranking Feature Verification
- '1. Amazon-Books data preprocessing.ipynb'의 meta data에서 뽑아낸 item들의 ranking값을 이용하여 '2. LR-GCCF.ipynb'를 이용한 recommender system의 성능을
올릴 수 있는지 확인하는 것이 목표입니다. 성능평가는 Recall과 NDCG로 진행합니다.
- ranking 값을 가공 및 계산하여 recommender에 적용하기 전, ranking 값의 분포에 대해 가정을 하고 유효성에 대한 간접적인 예측을 합니다. ranking 값의 분포를 분석하여 생각해낸 가정은
'성능이 조금은 올라갈 수 있겠지만 그렇게 큰 폭의 향상은 아닐 것이다' 입니다.
- Min-max scaling을 적절히 해주고 여러 hyper-parameter에 대해 조절해준 결과, recommender에 ranking을 적용은 근소한 성능향상은 있지만 그리 큰 개선은 아니라는 가정이 맞았음을 확인할 수 있었습니다.
