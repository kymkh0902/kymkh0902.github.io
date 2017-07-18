---
layout: page
subheadline:  "image detection"
title:  "GoogleNet, Inception model"
teaser: ""
categories:
    - machine learning
    - deep learning
tags:
    - machine learning
    - deep learning
    - image detection
header: no
---

- GoogleNet은 Google에서 발표한 2014년도에 IRSVRC에서 1등을 차지한 모델이다.  
- 아래 내용은 Going Deeper with Convolution 논문을 읽은 뒤에 요약 및 정리한 것이다.
- 다른 모델들에서 가져온 컨셉들, 후에 개선을 위해 변경한 점들은 지속 업데이트 예정이다.

1. introduction <br>
핵심적으로 이 논문에서 언급하는 내용은 **inceptioon layer** 의 도입으로 쌓이는 layer
에 의한 computational budget 해결 및 mobile, embedded computing을 위한 모델 개발 필요성이다.

2. Related Work <br>
*Despite of concerns that max-pooling layers result in loss of accurate spatial information*
ResNet에 이어서 pooling의 한계점에 대해서 언급하고 있는데 이 부분은 현재 pooling에 대한
trend를 확인한 후에 기록할 예정이다.
Inception layer를 Network in Network - Lin et al.의 접근법에서 착안하여 dimension reduction to
computational bottlenects 를 해결하기 위한 방안으로 고안하였다고 언급하고 있다.
뒷 부분에 R-CNN에 관련한 내용이 나오는데 image segmentation, detection에 대한 부분은 관련
논문을 읽고 마저 정리할 예정이다.

3. Motivation and High Level Considerations <br>
당시 trend는 neural network를 layer width, depth를 늘려 깊게 학습시키면 좋은 성능을 발휘하였다.
근데 깊게 학습할 경우에 overfitting과 computation resource의 문제가 생기는데 이를 둘 다 해결하기
위해서 sparsity의 도입이 필요하다고 얘기하고 있다.(이를 위한 FC → Convolution 대체 필요.)
또한, 데이터의 확률 분포를 아주 큰 신경망으로 표현할 수 있다면 높은 상관성을 가지는 출력들과 이 때
활성화되는 neuron들의 cluster 관계를 분석하여 최적의 topology를 구성할 수 있다고 한다. Arora의 [논문](http://proceedings.mlr.press/v32/arora14.pdf "보기") 참조.
그런데, 컴퓨터 연산은 uniform distribution일 때 가장 효율적이어서 정반대의 문제에 봉착한다. (sparse는 비효율적)
이를 해결하기 위해 sparsity는 결국 망 내의 연결을 줄이면서 세부적인 행렬 연산에서는 최대한 dense한 연산을 하도록 해야 하는데
기존에는 convolution을 활용해서 filter-level sparsity를 얻을 수 있었고, 다음 단계에로 inception architectuure를 연구 중이다.

4. Architectural Details <br>
*There will be a smaller number of more spatially spread out clusters that can be covered by convolutions
over larger patches, and there will be a decreasing number of patches over larger and larger regions*
위의 문제가 어떤 건지 제대로 이해가 안 된다. 위와 같은 patch-alignment 문제를 해결하기 위해 inception architecture
내에선 1x1, 3x3, 5x5 filter size만 사용한다.
inception module을 도입했을 때 문제가 3x3, 5x5 convolution 연산량이 크다는 점인데, 이를 해결하기 위해서 1x1 convolution이
사용된다. (1x1 convolution → 3x3 convolution)
1x1 convolution 개념이 매우 중요한데 연산을 쪼개서 진행할 경우에 parameter수를 줄여 dimension을 낮춰주고
각 단계 별로 ReLu가 적용되어 non-linearity도 높혀준다.

5. GoogleNet <br>
computational efficiency를 다시 한번 언급하면서 embedded의 중요성에 대해 얘기하고 있다.
GoogleNet에서는 이전 버전의 모델에서 자주 사용되진 않는 average pooling이 사용되었다.
그리고 auxiliary classifier라는 것도 등장하는데 regularization 효과를 주기 위함이다.
training 중간에 가지를 내어 output을 내고 이 Loss(x0.3)를 최종 output의 Loss와 함께 게산하여 gradient를 update해준다.
이런 구조를 통해서 layer가 깊어질 수록 overfitting이 되는 점을 완화시켜줄 수 있다.


6. Result, Structure <br>
ILSVRC 결과 및 모델 구조 사진 첨부 예정.

7. Conclusion <br>
*Approximating the expected optimal sparse structure by readily available dense building
block is a viable method for improving neural networks for computer vision*
다시 한번 spasity에 대한 강조를 하고 있다.


Reference <br>
Szegedy, Christian, et al. ["Going deeper with convolutions." Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition.](http://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Szegedy_Going_Deeper_With_2015_CVPR_paper.pdf), 2015.
