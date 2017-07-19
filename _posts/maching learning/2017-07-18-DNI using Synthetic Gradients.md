---
layout: page
subheadline:  "Learning method"
title:  "Decoupled Neural Interfaces using Synthetic Gradients"
teaser: ""
categories:
    - machine learning
    - deep learning
tags:
    - machine learning
    - deep learning
    - Learning method
header: no
---
- 2016년에 DeepMind에서 발표한 논문이다.

1. introduction <br>

대략적인 내용 전개
기존의 Neural Network의 Network training 방식은 Gradient descent를 이용하며
forward, backward propagation, gradient update 진행할 때 방향성을 가지고 진행된다. (asynchro..., 병렬X)
그래서 time consuming, intractable 하다는 단점이 있다.
이런 문제를 해결하기 위해 network 사이의 모듈을 끊어서 기존의 방식에서 벗어나
gradient를 predict(approximate)할 수 있는 parametric model을 학습하는 것을 synthhetic gradient라고 한다.


synthetic gradient module이 학습되는 방법은 target(true) gradient를 regress 하는 방향으로 움직인다.
구해지는 방법과 진행되는 구조를 간단히 설명하면
layer를 지날 때 synthetic gradient가 구해지고 feedback을 바로 받는다. 그리고 target gradient가 내려오면 (나중에)
받아서 synthetic gradient module을 update한다.

실험 결과 DNI로 학습한 모델이 backpropagation한 모델과 성능이 거의 동일하게 나왔다고 한다.

특이한 case로 RNN 구조의 모델의 경우는 망이 깊어지면서 backpropagation 진행이 힘들어
BPTT라는 방법을 사용한다. 일정 개수만큼의 layer를 지난 뒤에 backprop을 막고 weight를 update하는 방식인데
이렇게 할 경우 gradient가 이 전 몇 step에만 영향을 받아서 update가 이루어져서 temporal dependency를 제한한다.

그래서 DNI로 backprop가 막히는 구간에 synthetic gradient를 이전 layer로 흘려줘서 weight를 update할 때
이전 gradient를 모두 볼 수 있도록 해준다. 이렇게 학습했을 때 그냥 BPTT를 사용했을 때보다 성능이 향상됨을 확인하였다.

어떤 경우에 사용할 수 있을까?

일단 속도적인 측면에서 모든 module간의 communiication을 할 수 있도록 (locking을 전부다 없애면) 하면
병렬처리가 가능해져서 엄청나게 빨라질 수 있겠다.



Reference: <br>
Jaderberg, Max, et al. ["Decoupled neural interfaces using synthetic gradients."](https://arxiv.org/pdf/1608.05343.pdf) arXiv preprint arXiv:1608.05343 (2016).
DeepMind Blog : https://deepmind.com/blog/decoupled-neural-networks-using-synthetic-gradients/
