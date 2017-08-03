---
layout: page
subheadline:  "NLP"
title:  "Distributed Representations of Words and phrases and their Compositionality"
teaser: ""
categories:
    - machine learning
    - deep learning
tags:
    - machine learning
    - deep learning
    - NLP
    - CS224n
header: no
---

- CS224n Lecture2 내용에 포함된 논문으로 Skip-gram에 대한 내용을 주로 다루고 있다.

#### 1. Introduction
Distributed representations of words는 아주 예전부터 연구되던 주제이다. 과거에 연구가 진행되어 오다가
NLP에서 Skip-gram 모델이 발표가 되면서 낮은 연산량으로 training이 가능해졌다.
그리고 이렇게 word를 vector로 표현하게 되면서 언어학적인 규칙이나 패턴을 나타낼 수 있게 되었다.
이 논문에서는 기존 Skip-gram 모델에 몇 가지 연산, 성능 향상을 위한 기법을 소개하고 있다.
*Hierarchical softmax, Negative sampling, Subsampling of frequent word, Phase skip-gram* 이 그것들이다.
순서대로 상세하게 소개하도록 하겠다.

#### 2. The Skip-gram Model
Skip-gram model을 training하는 방법은 특정 word를 중심으로 주변에 가장 있을만한 word를 찾는 것이다.
maximize할 objective function은 아래와 같다. <br>
$${1 \above 1pt T} \sum_{t=1}^T \sum_{-c\leqq j \leqq c, j\neq 0} log\ p(w_{t+j}|w_t)$$ <br>
$$(c : window\ size,\ w_t : center\ word)$$ <br>
그리고 구하고자 하는 center word일 때 output word일 확률은 아래와 같이 정의한다.
$$p(w_O|w_I) = {exp({{v'}_{w_O}}^T v_{w_I}) \above 1pt \sum_{w=1}^W exp({{v'}_w}^T v_{w_I})}$$ <br>
$$(v_w,{v'}_w :\ "input"\ and\ "output"\ vector\ representations\ of\ w,\ W: number\ of\ words\ in\ vocabulary)$$

#### 2.1 Hiearchical Softmax
$$\nabla log\ p(w_O|w_I)$$를 구할 때의 연산량이 $$W$$에 비례하는데 $$W$$는 보통 $$10^5-10^7$$ 정도로 큰 편이라
줄이고자 하여 도입한 개념이다. 줄인 뒤의 연산량은 $$log_2(W)$$에 비례하게 된다.


#### 2.2 Negative sampling




Reference: <br>
Tomas Mikolov, Ilya Sutskever, Kai Chen, Greg Corrado, Jeffrey Dean. [Distributed Representations of Words and Phrases and their Compositionality](http://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf).2013.
