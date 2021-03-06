## BP神经网络相关概念
+ 什么是神经网络？
神经网络是由很多神经元组成的，用个比较粗浅的解释，可能不太全面科学，但对初学者很容易理解：
    1. 我们把输入数据，输进去神经网络
    2. 这些数据的每一个都会被乘个数，即权值w，然后这些东东与阀值b相加后求和得到u
    3. 上面只是线性变化，为了达到能处理非线性的目的，u做了个变换，变换的规则和传输函数有关
可能还有人问，那么那个阀值是什么呢？简单理解就是让这些数据做了个平移，这就是神经元工作的过程。处理后的结果又作为输入，可输给别的神经元，很多这样的神经元，就组成了网络。
+ BP网络的特点
 1. 网络实质上实现了一个从输入到输出的映射功能，而数学理论已证明它具有实现任何复杂非线性映射的功能。这使得它特别适合于求解内部机制复杂的问题。我们无需建立模型，或了解其内部过程，只需输入，获得输出。只要BPNN结构优秀，一般20个输入函数以下的问题都能在50000次的学习以内收敛到最低误差附近。而且理论上，一个三层的神经网络，能够以任意精度逼近给定的函数，这是非常诱人的期望；
 2. 网络能通过学习带正确答案的实例集自动提取“合理的”求解规则，即具有自学习能力；
 3. 网络具有一定的推广、概括能力。
+ BP主要应用
回归预测（可以进行拟合，数据处理分析，事物预测，控制等）、 分类识别（进行类型划分，模式识别等）。但无论那种网络，什么方法，解决问题的精确度都无法打到100%的，但并不影响其使用，因为现实中很多复杂的问题，精确的解释是毫无意义的，有意义的解析必定会损失精度。
+ BP算法的学习速度很慢，其原因主要有：
    1. 由于BP算法本质上为梯度下降法，而它所要优化的目标函数又非常复杂，因此，必然会出现“锯齿形现象”，这使得BP算法低效；
    2. 存在麻痹现象，由于优化的目标函数很复杂，它必然会在神经元输出接近0或1的情况下，出现一些平坦区，在这些区域内，权值误差改变很小，使训练过程几乎停顿；
    3. 为了使网络执行BP算法，不能用传统的一维搜索法求每次迭代的步长，而必须把步长的更新规则预先赋予网络，这种方法将引起算法低效。
+ 网络训练失败的可能性较大，其原因有：
    1. 从数学角度看，BP算法为一种局部搜索的优化方法，但它要解决的问题为求解复杂非线性函数的全局极值，因此，算法很有可能陷入局部极值，使训练失败；
    2. 网络的逼近、推广能力同学习样本的典型性密切相关，而从问题中选取典型样本实例组成训练集是一个很困难的问题。
+ 网络结构的选择：
尚无一种统一而完整的理论指导，一般只能由经验选定。为此，有人称神经网络的结构选择为一种艺术。而网络的结构直接影响网络的逼近能力及推广性质。因此，应用中如何选择合适的网络结构是一个重要的问题。
+ 新加入的样本要影响已学习成功的网络，而且刻画每个输入样本的特征的数目也必须相同。
+ 采用s型激活函数，由于输出层各神经元的理想输出值只能接近于1或0，而不能打到1或0，因此设置各训练样本的期望输出分量Tkp时，不能设置为1或0，设置0.9或0.1较为适宜。
 
+ 什么是网络的泛化能力？
一个神经网路是否优良，与传统最小二乘之类的拟合评价不同（主要依据残差，拟合优度等），不是体现在其对已有的数据拟合能力上，而是对后来的预测能力，既泛化能力。

     网络的预测能力（也称泛化能力、推广能力）与训练能力（也称逼近能力、学习能力）的矛盾。一般情况下，训练能力差时，预测能力也差，并且一定程度上，随训练能力地提高，预测能力也提高。但这种趋势有一个极限，当达到此极限时，随训练能力的提高，预测能力反而下降，即出现所谓“过拟合”现象。此时，网络学习了过多的样本细节，而不能反映样本内含的规律。
+ 过拟合是什么，怎么处理？
神经网络计算不能一味地追求训练误差最小，这样很容易出现“过拟合”现象，只要能够实时检测误差率的变化就可以确定最佳的训练次数，比如15000次左右的学习次数，如果你不观察，设成500000次学习，不仅需要很长时间来跑，而且最后结果肯定令人大失所望。

     避免过拟合的一种方法是：在数据输入中，给训练的数据分类，分为正常训练用、变量数据、测试数据，在后面节将讲到如何进行这种分类。
其中变量数据，在网络训练中，起到的作用就是防止过拟合状态。

+ 学习速率有什么作用？
学习速率这个参数可以控制能量函数的步幅，并且如果设为自动调整的话，可以在误差率经过快速下降后，将学习速率变慢，从而增加BPNN的稳定性。

## 训练数据
下面列表中的数据是某地区20年公路运量数据，在作为下一节的神经网络程序的输入。其中属性“人口数量”、“机动车数量”和“公路面积”作为神经网络的三个输入，属性“公路客运量”和“公路货运量”作为神经网络的两个输出。
<table border="1" width="300" frame="hsides" rules="groups">
     <caption> <font size="3"> 某地区20年公路运量数据</font> </caption>
     <colgroup span="1" width="80"></colgroup>
     <colgroup span="1" width="380"></colgroup>
     <colgroup span="1" width="430"></colgroup>
     <colgroup span="1" width="520"></colgroup>
     <colgroup span="2" width="370"></colgroup>

<thead>
     <tr>
          <td><b>年份</b></td>
          <td><b>人口数量/万人</b></td>
          <td><b>机动车数量/万辆</b></td>
          <td><b>公路面积/万平方千米</b></td>
          <td><b>公路客运量/万人</b></td>
          <td><b>公路货运量/万吨</b></td>
     </tr>
</thead>
<tbody>
     <tr>
          <td>1990</td>
          <td>20.55</td>
          <td>0.6</td>
          <td>0.09</td>
          <td>5126</td>
          <td>1237</td>
     </tr>
     <tr>
          <td>1991</td>
          <td>22.44</td>
          <td>0.75</td>
          <td>0.11</td>
          <td>6217</td>
          <td>1379</td>
     </tr>
     <tr>
          <td>1992</td>
          <td>25.37</td>
          <td>0.85</td>
          <td>0.11</td>
          <td>7730</td>
          <td>1385</td>
     </tr>
     <tr>
          <td>1993</td>
          <td>27.13</td>
          <td>0.90</td>
          <td>0.14</td>
          <td>9145</td>
          <td>1399</td>
     </tr>
</tbody>
<tr>
          <td>1994</td>
          <td>29.45</td>
          <td>1.05</td>
          <td>0.20</td>
          <td>10460</td>
          <td>1663</td>
     </tr>
     <tr>
          <td>1995</td>
          <td>30.1</td>
          <td>1.35</td>
          <td>0.23</td>
          <td>11387</td>
          <td>1714</td>
     </tr>
     <tr>
          <td>1996</td>
          <td>30.96</td>
          <td>1.45</td>
          <td>0.23</td>
          <td>12353</td>
          <td>1834</td>
     </tr>
     <tr>
          <td>1997</td>
          <td>34.06</td>
          <td>1.60</td>
          <td>0.32</td>
          <td>15750</td>
          <td>4322</td>
     </tr>
     <tr>
          <td>1998</td>
          <td>36.42</td>
          <td>1.70</td>
          <td>0.32</td>
          <td>18304</td>
          <td>8132</td>
     </tr>
     <tr>
          <td>1999</td>
          <td>38.09</td>
          <td>1.85</td>
          <td>0.34</td>
          <td>19836</td>
          <td>8936</td>
     </tr>
     <tr>
          <td>2000</td>
          <td>39.13</td>
          <td>2.15</td>
          <td>0.36</td>
          <td>21024</td>
          <td>11099</td>
     </tr>
     <tr>
          <td>2001</td>
          <td>39.99</td>
          <td>2.20</td>
          <td>0.36</td>
          <td>19490</td>
          <td>11203</td>
     </tr>
     <tr>
          <td>2002</td>
          <td>41.93</td>
          <td>2.25</td>
          <td>0.38</td>
          <td>20433</td>
          <td>10524</td>
     </tr>
     <tr>
          <td>2003</td>
          <td>44.59</td>
          <td>2.35</td>
          <td>0.49</td>
          <td>22598</td>
          <td>11115</td>
     </tr>
     <tr>
          <td>2004</td>
          <td>47.30</td>
          <td>2.50</td>
          <td>0.56</td>
          <td>25107</td>
          <td>13320</td>
     </tr>
     <tr>
          <td>2005</td>
          <td>52.89</td>
          <td>2.60</td>
          <td>0.59</td>
          <td>33442</td>
          <td>16762</td>
     </tr>
     <tr>
          <td>2006</td>
          <td>55.73</td>
          <td>2.70</td>
          <td>0.59</td>
          <td>36836</td>
          <td>18673</td>
     </tr>
     <tr>
          <td>2007</td>
          <td>56.76</td>
          <td>2.85</td>
          <td>0.67</td>
          <td>40548</td>
          <td>20724</td>
     </tr>
     <tr>
          <td>2008</td>
          <td>59.17</td>
          <td>2.95</td>
          <td>0.69</td>
          <td>42927</td>
          <td>20803</td>
     </tr>
</tbody>
<tfoot>
     <tr>
          <td>2009</td>
          <td>60.63</td>
          <td>3.10</td>
          <td>0.79</td>
          <td>43462</td>
          <td>21804</td>
     </tr>
</tfoot>
</table>

## 效果图

![](https://github.com/wolfbrother/HeuristicApproach/blob/master/BPNeuralNetwork/_pic1.png?raw=true)
![](https://github.com/wolfbrother/HeuristicApproach/blob/master/BPNeuralNetwork/_pic2.png?raw=true)
