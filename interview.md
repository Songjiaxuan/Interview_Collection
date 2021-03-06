---
title: interview experience
date: 2022/2/8
description: Record interview learning experience
tag: interview
author: Songjiaxuan
---

##	梯度消失梯度爆炸

基于梯度下降的思想，以目标负梯度方向对参数进行调整，参数更新为$\omega = \omega - \alpha \frac{\partial Loss}{\partial \omega}$，如果要更新第二隐藏层的权值信息，则参考链式求导法则$\Delta \omega_2 = \frac{\partial Loss}{\partial \omega _2} = \frac{\partial Loss}{\partial f_4}\frac{\partial f_4}{\partial f_3}\frac{\partial f_3}{\partial f_2}\frac{\partial f_2}{\partial f_1}$，其中类似$\frac{\partial f_4}{\partial f_3}$就是对激活函数求导，如果此部分的值大于1，那么随着层数的增加，求出的梯度更新将以指数形式增加，发生梯度爆炸。如果此部分的值小于1，那么求出的梯度更新信息会以指数的形式衰减，发生梯度消失。

从深层网络的角度来说，不同层学习的速度差异很大，表现为网络中靠近输的层学习的情况很好，靠近输入的层学习很慢，甚至即使训练了很久，前几层的权值和刚开始初始化的值差不多，因此梯度消失和梯度爆炸的根本原因在于反向传播算法的不足

另一方面，如果激活函数的选择不合适，比如使用Sigmoid，梯度消失会很明显。Logistic函数的导数最大为0.25，其余时候远小于0.25，因此如果每层激活函数都为Logistic函数的话，很容易导致梯度消失问题。同理Tanh函数也容易导致梯度消失。

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="480" height="223pt"viewBox="0 0 360 223"> <defs> <symbol overflow="visible" id="a"> <path d="M5.29-2.191H.55v-.82h4.74zm0 0"/></symbol> <symbol overflow="visible" id="b"> <path d="M3.234 0v-1.715H.125v-.805l3.27-4.636h.714v4.636h.97v.805h-.97V0zm0-2.52v-3.226L.992-2.52zm0 0"/> </symbol> <symbol overflow="visible"id="c"> <path d="M5.035-.844V0H.305a1.54 1.54 0 01.101-.61c.121-.324.313-.64.578-.953.266-.312.649-.671 1.149-1.085.777-.637 1.304-1.141 1.578-1.516.273-.371.41-.723.41-1.055 0-.347-.125-.644-.375-.883-.246-.238-.574-.359-.973-.359-.421 0-.761.129-1.015.383-.254.254-.383.605-.387 1.055L.47-5.117c.062-.672.293-1.188.699-1.54.402-.355.945-.53 1.625-.53.687 0 1.23.19 1.629.57.402.383.601.855.601 1.418 0 .285-.058.566-.175.844-.118.277-.313.566-.582.875-.274.304-.723.726-1.356 1.257-.527.442-.867.743-1.015.903a2.868 2.868 0 00-.372.476zm0 0"/> </symbol> <symbol overflow="visible"id="d"> <path d="M3.727 0h-.88v-5.602c-.21.204-.488.407-.831.606a5.902 5.902 0 01-.926.457v-.852c.492-.23.922-.511 1.289-.84.367-.328.629-.648.781-.957h.567zm0 0"/> </symbol> <symboloverflow="visible" id="e"> <path d="M.906 0v-1H1.91v1zm0 0"/> </symbol> <symbol overflow="visible" id="f"><path d="M.414-3.531c0-.844.09-1.528.262-2.043.176-.516.433-.914.777-1.192.344-.28.774-.421 1.297-.421.383 0 .719.078 1.008.23.289.156.531.379.719.672.187.289.335.644.445 1.062.11.418.16.985.16 1.692 0 .84-.086 1.52-.258 2.035-.172.516-.43.914-.773 1.195-.344.281-.778.422-1.301.422-.691 0-1.234-.246-1.625-.742-.473-.594-.71-1.567-.71-2.91zm.906 0c0 1.176.137 1.957.41 2.347.278.387.614.582 1.02.582.402 0 .742-.195 1.016-.586.277-.39.414-1.171.414-2.343 0-1.18-.137-1.961-.414-2.348-.274-.387-.618-.582-1.028-.582-.402 0-.726.172-.965.516-.304.433-.453 1.238-.453 2.414zm0 0"/> </symbol> <symbol overflow="visible" id="g"> <pathd="M.414-1.875l.922-.078c.07.45.23.785.476 1.012.25.226.551.34.903.34.422 0 .781-.16 1.074-.477.293-.32.438-.742.438-1.27 0-.504-.141-.898-.422-1.187-.282-.29-.649-.434-1.106-.434a1.526 1.526 0 00-1.3.692L.57-3.383l.696-3.68h3.558v.844H1.97l-.387 1.922c.43-.3.879-.45 1.352-.45a2.14 2.14 0 011.582.65c.43.433.644.992.644 1.671 0 .649-.187 1.207-.566 1.68-.457.578-1.086.867-1.88.867-.651 0-1.183-.18-1.593-.547-.41-.363-.648-.847-.707-1.449zm0 0"/> </symbol> </defs> <path d="M7.66 35.668l5.371 8.715 5.367 9.754L23.77 64.78l5.37 11.371 5.372 11.914 5.367 12.274L50.62 125.2l5.371 12.198 5.367 11.786 5.371 11.195 5.372 10.426 5.367 9.492 5.37 8.414 5.372 7.203 5.371 5.883 5.367 4.465 5.371 2.98 5.371 1.453 5.368-.105 5.37-1.656 5.372-3.18 5.37-4.66 5.368-6.063 5.371-7.37 5.371-8.567 5.367-9.625 5.372-10.535 5.37-11.286 5.372-11.851 10.738-24.668 5.371-12.434 5.367-12.234 5.371-11.856 5.371-11.28 5.372-10.536 5.367-9.629 5.37-8.566 5.372-7.371 5.367-6.063 5.371-4.656 5.371-3.184 5.371-1.652 5.368-.105 5.37 1.449 5.372 2.984 5.367 4.465 5.371 5.879 5.371 7.203 5.371 8.418 5.367 9.492 5.372 10.426 5.37 11.191 10.739 23.985 5.371 12.418 5.371 12.441 5.367 12.277 5.371 11.914 5.371 11.368 5.372 10.644 5.367 9.758 5.37 8.715" fill="none" stroke-width="1.6" stroke-linecap="square"stroke="#5e81b5" stroke-miterlimit="3.25"/> <path d="M7.66 111.129v-4" fill="none" stroke-width=".2"stroke="#666" stroke-miterlimit="3.25"/> <use xlink:href="#a" x="1.66" y="123.128" fill="#666"/> <usexlink:href="#b" x="7.66" y="123.128" fill="#666"/> <path d="M29.14 111.129v-2.402M50.621 111.129v-2.402M72.102 111.129v-2.402M93.578 111.129v-4" fill="none" stroke-width=".2" stroke="#666" stroke-miterlimit="3.25"/> <use xlink:href="#a" x="87.58" y="123.128" fill="#666"/> <use xlink:href="#c" x="93.58"y="123.128" fill="#666"/> <path d="M115.059 111.129v-2.402M136.54 111.129v-2.402M158.02 111.129v-2.402M179.5 111.129v-4M200.98 111.129v-2.402M222.46 111.129v-2.402M243.941 111.129v-2.402M265.422 111.129v-4" fill="none" stroke-width=".2" stroke="#666" stroke-miterlimit="3.25"/> <use xlink:href="#c" x="262.42"y="123.128" fill="#666"/> <path d="M286.898 111.129v-2.402M308.379 111.129v-2.402M329.86 111.129v-2.402M351.34 111.129v-4" fill="none" stroke-width=".2" stroke="#666" stroke-miterlimit="3.25"/> <usexlink:href="#b" x="348.34" y="123.128" fill="#666"/> <path d="M.5 111.129h358M179.5 220.813h2.398M179.5 210.84h4" fill="none" stroke-width=".2" stroke="#666" stroke-miterlimit="3.25"/> <use xlink:href="#a" x="156.5"y="213.34" fill="#666"/> <g fill="#666"> <use xlink:href="#d" x="162.5" y="213.34"/> <use xlink:href="#e"x="168.062" y="213.34"/> <use xlink:href="#f" x="170.84" y="213.34"/> </g> <path d="M179.5 200.867h2.398M179.5 190.898h2.398M179.5 180.926h2.398M179.5 170.957h2.398M179.5 160.984h4" fill="none"stroke-width=".2" stroke="#666" stroke-miterlimit="3.25"/> <use xlink:href="#a" x="156.5" y="163.484"fill="#666"/> <g fill="#666"> <use xlink:href="#f" x="162.5" y="163.484"/> <use xlink:href="#e" x="168.062"y="163.484"/> <use xlink:href="#g" x="170.84" y="163.484"/> </g> <path d="M179.5 151.012h2.398M179.5 141.043h2.398M179.5 131.07h2.398M179.5 121.098h2.398M179.5 111.129h4M179.5 101.156h2.398M179.5 91.188h2.398M179.5 81.215h2.398M179.5 71.242h2.398M179.5 61.273h4" fill="none" stroke-width=".2"stroke="#666" stroke-miterlimit="3.25"/> <g fill="#666"> <use xlink:href="#f" x="162.5" y="63.772"/> <usexlink:href="#e" x="168.062" y="63.772"/> <use xlink:href="#g" x="170.84" y="63.772"/> </g> <path d="M179.5 51.3h2.398M179.5 41.328h2.398M179.5 31.36h2.398M179.5 21.387h2.398M179.5 11.418h4" fill="none" stroke-width=".2" stroke="#666" stroke-miterlimit="3.25"/> <g fill="#666"> <use xlink:href="#d" x="162.5" y="13.916"/><use xlink:href="#e" x="168.062" y="13.916"/> <use xlink:href="#f" x="170.84" y="13.916"/> </g> <pathd="M179.5 1.445h2.398M179.5 221.758V.5" fill="none" stroke-width=".2" stroke="#666" stroke-miterlimit="3.25"/></svg>

###	解决方法

- 逐层贪婪预训练与微调：Hinton2006年提出的方法（A Fast Learning Algorithm for Deep Belief Nets）。一层一层的训练，一次只训练一层，所以完全可以在本来已经预训练好的模型后面再加一层，因为这新加的一层的训练只需要它前面的最后一层的数据，前面其他层的参数都是fixed固定的。贪婪即住层，贪婪是一个抽象的概念，即只处理好当前的任务，让当下的子任务达到最优，即是这样做并不会使得全局最优。一次只把一层训练好，训练到完美优秀。预训练基于的假设为：浅层模型总是比深层模型好训练。所以才把整个网络的训练任务分解为多个子任务，一次训练一层。
- 梯度剪切、梯度缩放、正则：梯度缩放涉及对误差梯度向量进行归一化，以使向量范数大小等于定义的值，例如1.0.只要他们超过阈值，就重新缩放它们。如果渐变超过了预期范围，则渐变裁剪会强制将渐变值裁剪为特定的最小值和最大值。当传统的梯度下降算法建议进行一个非常大的步长时，梯度裁剪将步长间小道足够小，以至于它不太可能走到梯度最陡峭的下降方向的区域之外。这种方法仅解决训练深度神经网路模型的数值稳定性，而不能改进网络性能。分为按值截断和按模截断。
- 使用合适的激活函数：Relu在小于0时梯度为0，在大于0时梯度恒为1，每层的梯度更新速度都一样。Relu解决了梯度消失和梯度爆炸问题，加快了计算速度（梯度恒定），加速了网络的训练。但是由于其负数部分恒为0，导致一些神经元无法激活（可通过设置小学习率部分解决，leakyrelu和elu改善），并且其输出并不是零中心化的。
- Batch Normalization
- 残差结构
- LSTM长短期记忆网络

##	Feature Extraction and Classification

###	Librosa

###	Essemtia

###	XGBoost

###	LightGBM

##	RNN

##	PyTorch



##	树模型

###	树模型的过拟合

切分次数变多，树的复杂性提高也就容易过拟合，相应每个切分出的小特征空间的统计信息可能只是噪音。决策树容易对数据产生过拟合，即生长出结构过于复杂的书模型，这时局部的特征空间会越分越小得倒了不靠谱的“统计噪声”。

###	剪枝

剪枝是将一棵子树的子节点全部删掉，根结点作为叶子节点。。考虑极端情况，如果令所有的叶子节点都只含有一个数据点，那么我们能够保证所有的训练数据都能准确分类，但是很有可能得到很高的预测误差，原因是将训练数据中所有的噪声数据都“准确划分”了，强化了噪声数据的作用。

####	[预剪枝](https://zhuanlan.zhihu.com/p/267368825)

提前结束决策树的生长。在构造决策树的过程中，先对每个节点在划分前进行估计，如果当前节点的划分不能带来决策树模型泛化性能的提升（比如分类精度的提升），则不对当前节点进行划分并且将当前节点标记为叶节点。

停止决策树生长最简单的方法：

- 定义一个高度，当决策树达到该高度时就停止决策树的生长
- 达到某个节点的实例具有相同的特征向量，即使这些实例不属于同一类，也可以停止决策树的生长。这个方法对于处理数据冲突问题比较有效。
- 定义一个阈值，当达到某个节点的实例数小于阈值时可以停止决策树的生长
- 定义一个阈值，通过计算每次扩张对系统性能的增益，并比较增益值与该阈值的大小来决定是否停止决策树生长。

####	后剪枝

在决策树生长完成之后再进行剪枝，介绍以下三种：

- REP错误率降低剪枝
- PEP悲观错误剪枝
- CCP代价复杂度剪枝





##	ML一些概念

- 偏差：模型预测的期望值与真实值之间的差，针对训练集。偏差用于描述模型的拟合能力，通常是由于我们对学习算法做了错误的假设，或模型的复杂度不够。比如真实模型是一个二次函数，我们假设模型为一次函数，这就会导致偏差过大。
- 方差：模型预测的期望值与预测值之间的差，针对验证集。方差通常是由于模型的复杂度对于训练集过高导致。比如模型是一个简单的二次函数，而我们假设模型是一个高次函数，会导致过拟合从而方差过大。
- ![TIM截图20180817192259.png](https://s2.loli.net/2022/04/28/zxa9YSjwnPehkN6.png)
- 



##	集成学习

####	[AdaBoost](https://www.youtube.com/watch?v=LsK-xG1cLYA&list=PLKDplsEaykiXiFaLvrAfxK7wAwl6o_bYJ)

- 对噪声样本和异常样本比较敏感



##	计算机网络

####	Vlan

Vlan tag的内容和位置

交换机划分vlan的几种方法：

- 基于端口
- 基于物理mac地址
- 基于ip子网
- 基于协议
