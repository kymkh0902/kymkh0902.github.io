---
layout: page
subheadline:  "image detection"
title:  "GoogleNet, Inception model"
teaser: ""
categories:
    - machine learning
tags:
    - machine learning
    - image detection
header: no
---

- GoogleNet은 Google에서 발표한 2014년도에 IRSVRC에서 1등을 차지한 모델이다.  
- 아래 내용은 Going Deeper with Convolution 논문을 읽은 뒤에 요약 및 정리한 것이다.
- 다른 모델들에서 가져온 컨셉들, 후에 개선을 위해 변경한 점들은 지속 업데이트 예정이다.

1. introduction <br>
핵심적으로 이 논문에서 나타내고 있는 내용은 **inceptioon layer** 의 도입으로 쌓이는 layer
에 의한 computational budget 해결, mobile, embedded computing을 위한 모델 개발 필요성 언급이다.

2. Related Work <br>
*Despite of concerns that max-pooling layers result in loss of accurate spatial information*
ResNet에 이어서 pooling의 한계점에 대해서 언급하고 있는데 이 부분은 현재 pooling에 대한
trend를 확인한 후에 기록할 예정이다.

Inception layer를 Network in Network - Lin et al.의 접근법에서 착안하여 dimension reduction to
computational bottlenects 를 해결하기 위한 방안으로 고안하였다고 언급하고 있다.

뒷 부분에 R-CNN에 관련한 내용이 나오는데 image segmentation, detection에 대한 부분은 관련
논문을 읽고 마저 정리할 예정이다.

3. Motivation and High Level Considerations <br>
당시 trend는 neural network를 layer width, depth를 늘려 깊게 학습시키며 regularization을
적당히 해주면 좋은 성능을 발휘하였다.

근데 깊게 학습할 경우에 overfitting과 computation resource의 문제가 생기는데 이를 둘 다 해결하기
위해서 sparsity의 도입이 필요하다고 얘기하고 있다.

또한, 데이터의 확률 분포를 아주 큰 신경망으로 표현할 수 있다면 높은 상관성을 가지는 출력들과 이 때
활성화되는 neuron들의 cluster 관계를 분석하여 최적의 topology를 구성할 수 있다고 한다.

그런데, 컴퓨터 연산은 uniform distribution일 때 가장 효율적이어서 정반대의 문제에 봉착한다. (sparse는 비효율적)
이러한 문제를 Arora의 논문(http://proceedings.mlr.press/v32/arora14.pdf)을 보고 참고해서 아래와 같은 방법을 생각하였다.
*sparsity는 결국 망 내의 연결을 줄이면서 세부적인 행렬 연산에서는 최대한 dense한 연산을 하도록 처리하는 것이다.*

4. Architectural Details <br>
*There will be a smaller number of more spatially spread out clusters that can be covered by convolutions
over larger patches, and there will be a decreasing number of patches over larger and larger regions*
위의 문제가 어떤 건지 제대로 이해가 안 된다. 위와 같은 patch-alignment 문제를 해결하기 위해 inception architecture
내에선 1x1, 3x3, 5x5 filter size만 사용한다.

inception module을 도입했을 때 문제가 3x3, 5x5 convolution 연산량이 크다는 점인데, 이를 해결하기 위해서 1x1 convolution이
사용된다. (1x1 convolution → 3x3 convolution)
이 경우에 각 단계 별로 ReLu도 적용해주는데 non-linearity도 높혀주는 효과가 있다.
